---
title: "Xây dựng Golden Image với CIS Linux Build Kit trong Amazon EC2 Image Builder"
date: 2025-05-11
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

## Bởi: Rajat Chatterjee

**Ngày:** 11 Tháng 5, 2025

**Chủ đề:** [Amazon Inspector](https://aws.amazon.com/blogs/mt/category/security-identity-compliance/amazon-inspector/), [Enterprise Governance and Control](https://aws.amazon.com/blogs/mt/category/management-and-governance/enterprise-governance-and-control/), [Expert (400)](https://aws.amazon.com/blogs/mt/category/learning-levels/expert-400/), [Management Tools](https://aws.amazon.com/blogs/mt/category/management-tools/), [Technical How-to](https://aws.amazon.com/blogs/mt/category/post-types/technical-how-to/)

---

## Giới thiệu

Việc xây dựng và triển khai các hệ điều hành (Operating System – OS) đã được harden và chứng nhận bảo mật là yêu cầu thiết yếu đối với bất kỳ đội ngũ Cloud Operations (CloudOps) hoặc Cloud Center of Excellence (CCoE) nào trong tổ chức. Các hướng dẫn và biện pháp kiểm soát bảo mật được sử dụng để chứng nhận các image này thường đến từ đội ngũ bảo mật nội bộ, dựa trên các tiêu chuẩn được công nhận rộng rãi trong ngành.

Tuy nhiên, bạn cũng cần khả năng kiểm soát quy trình hardening, với sự linh hoạt để chọn lựa các bước khắc phục và kịch bản triển khai ứng dụng phù hợp. Kết quả là hình thành nên "[Golden Amazon Machine Images](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html) (AMI)" – các image được các đơn vị nghiệp vụ (Business Units) sử dụng trực tiếp.

Quy trình này cần có tự động hóa, để việc xây dựng và kiểm thử diễn ra ở quy mô lớn và các image được phân phối thông qua [Amazon Machine Images](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html) (AMI) đã được phê duyệt. Một trong những tiêu chuẩn phổ biến được sử dụng làm cơ sở (baseline) cho việc harden image là [Center for Internet Security](https://www.cisecurity.org/) (CIS) — tổ chức tạo và duy trì một bộ hướng dẫn cấu hình được gọi là [CIS Benchmarks](https://www.cisecurity.org/benchmark), cung cấp các thực hành cấu hình tốt nhất (best practices) cho các công nghệ cụ thể như hệ điều hành, nền tảng đám mây, ứng dụng và cơ sở dữ liệu.

[CIS Benchmarks](https://www.cisecurity.org/benchmark) được công nhận rộng rãi trong ngành và được nhiều tiêu chuẩn như PCI DSS, HIPAA, DoD Cloud Computing SRG, FISMA, DFARS, và FEDRAMP tham chiếu.

Trong bài viết này, chúng tôi chọn benchmark của CIS cho Amazon Linux, và xây dựng một quy trình để tạo AMI được chứng nhận thông qua tự động hóa có thể vận hành ở quy mô lớn.

Bạn có thể sử dụng một trong hai cách tiếp cận để tạo Golden Images bằng cách áp dụng [CIS Benchmark](https://www.cisecurity.org/cis-benchmarks) cho hệ điều hành (OS) tương ứng nhằm harden AMI:

1. **Sử dụng component được quản lý với CIS AMI từ [AWS Marketplace](https://aws.amazon.com/marketplace/)** – Bạn cũng sẽ có quyền truy cập vào hardening component đi kèm, chạy các script để áp dụng hướng dẫn [CIS Benchmark](https://www.cisecurity.org/cis-benchmarks) Level 1 cho cấu hình của mình. Các component này do tổ chức CIS sở hữu và duy trì để đảm bảo luôn cập nhật theo hướng dẫn mới nhất. (Chi tiết về cách tiếp cận này có thể xem tại bài viết [Building CIS hardened Golden Images and Pipelines with EC2 Image Builder](https://aws.amazon.com/blogs/mt/building-cis-hardened-golden-images-and-pipelines-with-ec2-image-builder/)).

2. **Sử dụng quy trình hardening tự quản bằng [Amazon EC2 Image Builder](https://aws.amazon.com/image-builder/)** – Cách tiếp cận này mang lại sự linh hoạt cho đội ngũ CloudOps trong việc tùy chỉnh quy trình hardening, giúp họ có thể loại trừ các khuyến nghị trong benchmark có thể ảnh hưởng đến ứng dụng dự kiến triển khai trên instance sử dụng AMI.

Bạn có thể chọn một trong hai phương án trên sau khi phân tích kỹ các yếu tố đã được giải thích trong bài viết [How to Decide Between Building or Buying a CIS Hardened Image](https://aws.amazon.com/blogs/apn/how-to-decide-between-building-or-buying-a-cis-hardened-image/).

Trong bài này, chúng tôi sẽ minh họa cách sử dụng các script được công bố bởi CIS Linux Build Kit (LBK) để tạo quy trình hardening và xác thực tự quản sử dụng [EC2 Image Builder](https://aws.amazon.com/image-builder/) và [Amazon Inspector](https://aws.amazon.com/inspector/). Các script cần thiết để harden hệ điều hành cụ thể (theo [CIS Benchmark](https://www.cisecurity.org/cis-benchmarks)) được công bố trên trang CIS Workbench, có thể tải về nếu tổ chức của bạn là thành viên CIS SecureSuite®. Những script này có thể sử dụng lại với rất ít chỉnh sửa để tạo [EC2 Image Builder](https://aws.amazon.com/image-builder/) Component, sau đó dùng trong các Image Builder Pipeline.

Chúng ta sẽ thực hiện cách tiếp cận này bằng việc tạo một AMI [Amazon Linux 2](https://aws.amazon.com/amazon-linux-2/) đã được harden, và kiểm tra (validate) nó bằng [Amazon Inspector](https://aws.amazon.com/inspector/).

---

## Tổng quan giải pháp

Giải pháp bao gồm các bước chính sau:

**Tiền đề (Prerequisites):**

1. Có tài khoản AWS với quyền thích hợp.
2. Tải xuống, khám phá và cập nhật CIS LBK Utility.
3. Tạo bucket [Amazon S3](https://aws.amazon.com/s3/) làm môi trường staging để tải lên CIS LBK Utility.
4. Cấu hình bucket S3 để phát thông báo qua [Amazon EventBridge](https://aws.amazon.com/eventbridge/).

**Các bước triển khai chính:**

1. Xây dựng Image Building Pipeline bằng [EC2 Image Builder](https://aws.amazon.com/image-builder/).
2. Xác thực image và tạo báo cáo bằng [Amazon Inspector](https://aws.amazon.com/inspector/).

---

## Tiền đề cho giải pháp

### Tải xuống, khám phá và cập nhật CIS LBK Utility

LBK dành cho [Amazon Linux 2](https://aws.amazon.com/amazon-linux-2/) có thể được tải từ trang CIS. Bộ công cụ này được cộng đồng CIS phát triển và duy trì, hiện được phân phối dưới dạng file `amazon_linux_2.tar.gz`.

Sau khi giải nén file tar, bạn sẽ thấy cấu trúc thư mục như sau:

![Cấu trúc thư mục của Bộ công cụ xây dựng CIS Linux](/images/3-BlogsTranslated/3.3-Blog3/cis-folders.jpg)

*Hình 1: Cấu trúc thư mục của Bộ công cụ xây dựng CIS Linux*

Thư mục chính chứa file `amazon_linux_2.sh` — script chính gọi các script phụ trong thư mục "functions". Nếu bạn muốn loại trừ một số khuyến nghị, hãy thêm chúng vào file `exclusion_list.txt`.

Hướng dẫn sử dụng và nội dung của bộ công cụ được mô tả chi tiết trong Quick Start Guide đi kèm trong package. Script `amazon_linux_2.sh` bao gồm hai bước tương tác khi chạy. Để tự động hóa, bạn nên chỉnh sửa script này để cố định (hardcode) các giá trị đầu vào.

Cụ thể, bạn cần:

1. **Chấp nhận điều khoản sử dụng (Terms and Conditions)** – nên comment dòng này trong script, và nhóm CloudOps có thể cung cấp nội dung điều khoản riêng cho người dùng trong tài liệu nội bộ.

2. **Chọn profile hardening** – có bốn profile: L1S, L2S, L1W, L2W tương ứng với Level 1/2 của [CIS Benchmark](https://www.cisecurity.org/cis-benchmarks), và loại Server (S) hoặc Workstation (W).

Trong ví dụ này, chúng ta sẽ dùng profile L1S để harden [Amazon Linux 2](https://aws.amazon.com/amazon-linux-2/) theo chuẩn [CIS Benchmark](https://www.cisecurity.org/cis-benchmarks) Level 1.

Các dòng cần chỉnh trong script:

```bash
#terms_of_use
#Display CIS Linux Build Kit warning banner
#WARBNR ---> Comment dòng này
run_profile=L1S # Uncomment dòng này để chỉ định profile được chạy
```

Sau khi chỉnh sửa, đóng gói lại file LBK:

```bash
tar cvf amazon_linux_2.tar.gz CIS-LBK
```

### Tạo bucket S3 staging và tải lên LBK

Tạo bucket S3 (ví dụ: `cis-lbk-al2`) bằng AWS Management Console hoặc [AWS CLI](https://aws.amazon.com/cli/). Bucket này được dùng để lưu trữ script LBK, phục vụ cho [EC2 Image Builder](https://aws.amazon.com/image-builder/) tải xuống và tải log sau khi chạy.

```bash
aws s3api put-object --bucket cis-lbk-al2 --key amazon_linux_2.tar.gz --body amazon_linux_2.tar.gz
```

### Cấu hình bucket S3 để gửi thông báo qua Amazon EventBridge

Bật [Amazon S3](https://aws.amazon.com/s3/) Event Notifications cho bucket, chọn gửi thông báo đến [Amazon EventBridge](https://aws.amazon.com/eventbridge/). Bạn có thể kích hoạt tính năng này trong tab Properties của bucket.

![Cho phép gửi thông báo đến EventBridge từ thùng S3](/images/3-BlogsTranslated/3.3-Blog3/s3-eventbridge.jpg)

*Hình 2: Cho phép gửi thông báo đến EventBridge từ thùng S3*

---

## Các bước triển khai

![Kiến trúc của giải pháp](/images/3-BlogsTranslated/3.3-Blog3/arch-diagram.jpg)

*Hình 3: Kiến trúc của giải pháp*

---

## Xây dựng Image Building Pipeline bằng EC2 Image Builder

Giải pháp này sử dụng các dịch vụ AWS sau:

* **[EC2 Image Builder](https://aws.amazon.com/image-builder/)**: chạy LBK từ [Amazon S3](https://aws.amazon.com/s3/) để harden và đóng gói AMI. [EC2 Image Builder](https://aws.amazon.com/image-builder/) dùng AWS Task Orchestrator and Executor ([AWSTOE](https://docs.aws.amazon.com/imagebuilder/latest/userguide/toe-component-manager.html)) để điều phối workflow phức tạp. Bạn sẽ tạo [custom component](https://docs.aws.amazon.com/imagebuilder/latest/userguide/create-custom-components.html) để gọi LBK từ S3, đồng thời định nghĩa image recipe làm image cơ sở (base image) và [image workflow](https://docs.aws.amazon.com/imagebuilder/latest/userguide/manage-image-workflows.html) xác định trình tự các bước build.

* **[Amazon Inspector](https://aws.amazon.com/inspector/)**: thực hiện kiểm tra bảo mật (CIS scan) để xác thực AMI sau khi harden.

* **[Amazon EventBridge](https://aws.amazon.com/eventbridge/)**: tích hợp sẵn với [EC2 Image Builder](https://aws.amazon.com/image-builder/) và S3 để phát sự kiện (event). Các sự kiện này có thể được chuyển tiếp đến [Amazon SNS](https://aws.amazon.com/sns/) hoặc [AWS Lambda](https://aws.amazon.com/lambda/).

* **[Amazon SNS](https://aws.amazon.com/sns/)**: dùng để gửi thông báo qua email về các bước kiểm duyệt (approval step).

### Tạo Component tùy chỉnh cho EC2 Image Builder

Bạn có thể sử dụng [AWS CloudFormation](https://aws.amazon.com/cloudformation/) template (được cung cấp trong bài gốc) để khởi tạo stack tạo toàn bộ các tài nguyên được đề cập trong các phần dưới đây.

Lệnh ví dụ để triển khai stack:

```bash
aws cloudformation create-stack --stack-name cis-al2-harden-stack \
--template-body file://cis-lbk-automation-template.yml \
--parameters
```

CloudFormation template này sẽ tự động tạo các thành phần được mô tả trong kiến trúc giải pháp.

Trong bài viết này, ta định nghĩa một [custom component](https://docs.aws.amazon.com/imagebuilder/latest/userguide/create-custom-components.html) có tên **CIS-AL2-L1S-Hardening**, với các đặc điểm sau:

* **Type**: build
* **Platform**: Linux và các hệ điều hành tương thích
* **OS Version**: [Amazon Linux 2](https://aws.amazon.com/amazon-linux-2/)

Mỗi component được định nghĩa bằng một tài liệu YAML, trong đó bucket S3 được tạo ở bước tiền đề được dùng để tải script và tải log lên. Bảng sau đây liệt kê một loạt các bước trong giai đoạn xây dựng.

![Các bước của giai đoạn Xây dựng](/images/3-BlogsTranslated/3.3-Blog3/workflow-steps.jpg)

*Hình 4: Các bước của giai đoạn Xây dựng*

Ví dụ định nghĩa component:

```yaml
name: 'CIS-Build-AL2-Level1'
description: 'Applies the L1S CIS settings for Amazon Linux 2 (AL2)'
schemaVersion: 1.0
parameters:
  - StagingS3BucketName:
      type: string
  - FileName:
      type: string
      default: 'amazon_linux_2.tar.gz'
  - Version:
      type: string
      default: '2024.2.0'
  - Level:
      type: string
      default: 'L1S'
phases:
  - name: build
    steps:
      - name: OperatingSystemRelease
        action: ExecuteBash
        inputs:
          commands:
            - |
                FILE=/etc/os-release
                if [ -e $FILE ]; then
                . $FILE
                echo "$ID${VERSION_ID:+.${VERSION_ID}}"
                else
                echo "The file $FILE does not exist. Exiting."
                exit 1
                fi
      - name: GetPackageManager
        action: ExecuteBash
        inputs:
          commands:
            - |
                RELEASE='{{build.OperatingSystemRelease.outputs.stdout}}'
                case "${RELEASE}" in
                amzn*|centos*|rhel*)
                    echo 'yum'
                    ;;
                ubuntu*)
                    echo 'apt'
                    ;;
                *)
                    echo "Operating System '${RELEASE}' is not supported. Exiting."
                    exit 1
                    ;;
                esac
      - name: VerifyPrerequisite
        action: ExecuteBash
        onFailure: Abort
        inputs:
          commands:
            - |
                INSTALL_TYPE='{{build.GetPackageManager.outputs.stdout}}'
                case "${INSTALL_TYPE}" in
                'yum')
                    if ! yum -q list installed tar &>/dev/null; then
                    yum install -q -y tar
                    echo "Installed tar."
                    else
                    echo "Tar is already installed."
                    fi
                    ;;
                'apt')
                    apt install -q -y tar
                    echo "Installed tar."
                    ;;
                *)
                    echo "Install type '${INSTALL_TYPE}' is not supported at this time. Exiting."
                    exit 1
                    ;;
                esac
      - name: MakeStagingDIR
        action: ExecuteBash
        inputs:
          commands:
            - |
                mkdir cis-stage
                cd cis-stage
                pwd
      - name: SettingStagingDirPermissions
        action: SetFolderPermissions
        inputs:
          - path: '{{build.MakeStagingDIR.outputs.stdout}}'
            permissions: 0700
      - name: Download_CIS_LBK
        action: S3Download
        inputs:
          - source: s3://{{ StagingS3BucketName }}/{{ FileName }}
            destination: '{{build.MakeStagingDIR.outputs.stdout}}/{{ FileName }}'
      - name: Unzip_CIS_LBK
        action: ExecuteBash
        onFailure: Continue
        inputs:
          commands:
            - sudo tar -xvf '{{build.MakeStagingDIR.outputs.stdout}}/{{ FileName }}' -C '{{build.MakeStagingDIR.outputs.stdout}}/' || ( echo "File extraction failed. Exiting" ; exit 1; )
      - name: AL2_ConfigureCIS
        action: ExecuteBash
        onFailure: Continue
        inputs:
          commands:
            - |
                RELEASE='{{ build.OperatingSystemRelease.outputs.stdout }}'
                if [ ! `echo "$RELEASE" | grep -E "^(amzn\.2$|(centos|rhel)\.7)"` ]; then
                echo 'Skipping this step for the current operating system.'
                exit 0
                fi
                cd {{build.MakeStagingDIR.outputs.stdout}}/CIS-LBK/cis_lbk_amazon_linux_2
                sudo ./amazon_linux_2.sh || ( echo "CIS configuration script failed to run. Exiting." ; exit 1; )
      - name: UploadLogFiles
        action: S3Upload
        onFailure: Abort
        maxAttempts: 3
        inputs:
          - source: '{{build.MakeStagingDIR.outputs.stdout}}/CIS-LBK/cis_lbk_amazon_linux_2/logs/*'
            destination: 's3://{{ StagingS3BucketName }}/logs/'
            recurse: true
            expectedBucketOwner: <<your account number>>
```

### Tạo Image Recipe dựa trên Component

Image Recipe là một tài liệu định nghĩa các **component** được áp dụng lên **base image** để tạo ra cấu hình mong muốn cho **output image**.

Recipe có tên **CIS-AL2-L1S-Hardening-Recipe** được tạo bằng **component tùy chỉnh CIS-AL2-L1S-Hardening** làm giai đoạn build chính.

**Mẫu AWS CloudFormation template** có dạng như sau:

```yaml
ImageRecipeCISAL2L1S:
  Type: "AWS::ImageBuilder::ImageRecipe"
  Properties:
    Components:
    - ComponentArn: !GetAtt CISAL2HardeningComponent.Arn
      Parameters:
      - Value:
        - !Sub "${StagingS3BucketName}"
        Name: "StagingS3BucketName"
    WorkingDirectory: "/var/tmp"
    ParentImage: !Sub "arn:aws:imagebuilder:${AWS::Region}:aws:image/amazon-linux-2-x86/x.x.x"
    Version: "1.0.0"
    AdditionalInstanceConfiguration:
      SystemsManagerAgent:
        UninstallAfterBuild: true
    Name: "CIS-AL2-L1S-Hardening"
  DependsOn:
    - CISAL2HardeningComponent
```

Như bạn thấy, **component tùy chỉnh ([custom component](https://docs.aws.amazon.com/imagebuilder/latest/userguide/create-custom-components.html))** được tham chiếu trong recipe, đồng thời recipe cũng tham chiếu **phiên bản mới nhất của [Amazon Linux 2](https://aws.amazon.com/amazon-linux-2/)** làm **base image** thông qua thuộc tính ParentImage.

Component **CIS-AL2-L1S-Hardening** nhận **WorkingDirectory** làm đầu vào.

{{% notice note %}}
Giá trị này phải được đặt là `/var/tmp`, vì thư mục `/tmp` mặc định sẽ bị thay đổi quyền truy cập (permission) bởi script CIS LBK, gây lỗi truy cập khi chạy.
{{% /notice %}}

### Tạo Workflow tùy chỉnh cho quá trình Build Image

**Image Workflow** định nghĩa chuỗi các bước [EC2 Image Builder](https://aws.amazon.com/image-builder/) thực hiện trong giai đoạn **build** và **test** của quy trình tạo image.

Workflow này giới thiệu hành động **WaitForAction**, giúp thêm bước **phê duyệt thủ công (manual approval)** sau khi quá trình hardening hoàn tất. Tại bước này, người vận hành có thể xem log và tiếp tục tạo image. Đồng thời, đây cũng là lúc có thể kích hoạt và xem kết quả **[Amazon Inspector](https://aws.amazon.com/inspector/) scan**.

Hành động **WaitForAction** sẽ tạm dừng workflow đang chạy và chờ phản hồi từ API **SendWorkflowStepAction** của [EC2 Image Builder](https://aws.amazon.com/image-builder/). Sự kiện này sẽ phát **EventBridge event** với detail-type là "[EC2 Image Builder](https://aws.amazon.com/image-builder/) Workflow Step Waiting".

Đối với bài blog này, nhóm tác giả tạo **workflow tùy chỉnh** có tên **build-cis-hardened-manual-action**, loại **BUILD**, với nội dung YAML như sau:

```yaml
name: build-cis-hardened-manual-action
description: Workflow to build an AMI
schemaVersion: 1.0

steps:
  - name: LaunchBuildInstance
    action: LaunchInstance
    onFailure: Abort
    inputs:
      waitFor: "ssmAgent"

  - name: ApplyBuildComponents
    action: ExecuteComponents
    onFailure: Abort
    inputs:
      instanceId.$: "$.stepOutputs.LaunchBuildInstance.instanceId"

  - name: InventoryCollection
    action: CollectImageMetadata
    onFailure: Abort
    if:
      and:
        - stringEquals: "AMI"
          value: "$.imagebuilder.imageType"
        - booleanEquals: true
          value: "$.imagebuilder.collectImageMetadata"
    inputs:
      instanceId.$: "$.stepOutputs.LaunchBuildInstance.instanceId"

  - name: RunSanitizeScript
    action: SanitizeInstance
    onFailure: Abort
    if:
      and:
        - stringEquals: "AMI"
          value: "$.imagebuilder.imageType"
        - stringEquals: "Linux"
          value: "$.imagebuilder.platform"
    inputs:
      instanceId.$: "$.stepOutputs.LaunchBuildInstance.instanceId"

  - name: CheckBuildLogsAndApprove
    action: WaitForAction

  - name: CreateOutputAMI
    action: CreateImage
    onFailure: Abort
    if:
      stringEquals: "AMI"
      value: "$.imagebuilder.imageType"
    inputs:
      instanceId.$: "$.stepOutputs.LaunchBuildInstance.instanceId"

  - name: TerminateBuildInstance
    action: TerminateInstance
    onFailure: Continue
    inputs:
      instanceId.$: "$.stepOutputs.LaunchBuildInstance.instanceId"

outputs:
  - name: "ImageId"
    value: "$.stepOutputs.CreateOutputAMI.imageId"
```

### Tạo Image Pipeline

Trong ví dụ của bài blog, nhóm tạo **Image Pipeline** có tên **CIS-AL2-L1S-Hardening-pipeline**.

[EC2 Image Builder](https://aws.amazon.com/image-builder/) sử dụng **[Amazon Inspector](https://aws.amazon.com/inspector/)** để quét các instance test tìm lỗ hổng bảo mật trong hệ điều hành hoặc gói phần mềm. Tuy nhiên, trong trường hợp này, tính năng quét lỗ hổng mặc định được **vô hiệu hóa**, vì nhóm chỉ sử dụng [Amazon Inspector](https://aws.amazon.com/inspector/) để **thực hiện CIS scan** trên **output image**, không thực hiện quét lỗ hổng chung.

Lịch **Build Schedule** được đặt ở chế độ **Manual**. Bạn có thể định kỳ hóa bằng **Schedule Builder (CRON expression)**.

Tiếp theo, nhóm chọn **Image Recipe (CIS-AL2-L1S-Hardening-Recipe)** đã tạo ở bước trước. Quá trình tạo image được định nghĩa bằng **Custom Workflow**, trong đó **build-cis-hardened-manual-action** được chọn làm **Build workflow**.

IAM Role được chọn là **AWSServiceRoleForImageBuilder**, cung cấp quyền truy cập cần thiết cho [EC2 Image Builder](https://aws.amazon.com/image-builder/).

Cấu hình hạ tầng (Infrastructure Configuration) giữ nguyên mặc định, ngoại trừ việc thêm **Resource Tag** cho instance:
- **Key**: `IBCISHardened`
- **Value**: `Yes`

→ Thẻ này được dùng trong cấu hình [Amazon Inspector](https://aws.amazon.com/inspector/) làm tiêu chí lọc (filter) để quét CIS.

**Distribution Settings** giữ nguyên mặc định – tức là [EC2 Image Builder](https://aws.amazon.com/image-builder/) sẽ khởi chạy instance trong **default VPC**, và **phân phối AMI đầu ra** trong **Region hiện tại**.

Phần **Infrastructure Configuration** này tạo IAM Role và Instance Profile được sử dụng bởi build instance để cấu hình AMI. Instance Role này gắn với **EC2InstanceProfileForImageBuilder** và **AmazonSSMManagedInstanceCore managed policy**.

Để chạy **[Amazon Inspector](https://aws.amazon.com/inspector/) CIS scan** thành công, **Instance Profile** cần gắn thêm policy **AmazonInspector2ManagedCisPolicy**.

Cấu hình IAM Instance Profile Role có dạng như sau:

```yaml
IAMManagedPolicyIBCisComponentS3Access:
  Type: "AWS::IAM::ManagedPolicy"
  Properties:
    ManagedPolicyName: "IBCisComponentS3Access"
    Path: "/"
    Description: "Access to S3 Bucket from EC2 ImageBuilder CIS Custom component"
    PolicyDocument:
      Version: "2012-10-17"
      Statement:
      - Resource: "arn:aws:s3:::cis-lbk-al2/*"
        Action:
        - "s3:PutObject"
        - "s3:GetObject"
        Effect: "Allow"

IAMRoleForImageBuilderCISAL2:
  Type: "AWS::IAM::Role"
  Properties:
    ManagedPolicyArns:
    - "arn:aws:iam::aws:policy/EC2InstanceProfileForImageBuilder"
    - Ref: "IAMManagedPolicyIBCisComponentS3Access"
    - "arn:aws:iam::aws:policy/AmazonInspector2ManagedCisPolicy"
    - "arn:aws:iam::aws:policy/AmazonSSMManagedInstanceCore"
    AssumeRolePolicyDocument:
      Version: "2012-10-17"
      Statement:
      - Action: "sts:AssumeRole"
        Effect: "Allow"
        Principal:
          Service: "ec2.amazonaws.com"

EC2InstanceProfileForImageBuilderCISAL2:
  Type: "AWS::IAM::InstanceProfile"
  Properties:
    Roles:
    - !Ref "IAMRoleForImageBuilderCISAL2"
    InstanceProfileName: "EC2InstanceProfileForImageBuilderCISAL2"
```

### Gắn Bucket Policy cho S3 Bucket cis-lbk-al2

Chính sách này chỉ cho phép IAM Role của Instance Profile truy cập bucket:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Statement1",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::7284282*****:role/IAMRoleForImageBuilderCISAL2"
      },
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": "arn:aws:s3:::cis-lbk-al2"
    }
  ]
}
```

### Tạo Amazon SNS Topic cho Email Notification

Nhóm tạo một **SNS Topic** có tên `ImageBuilderQueue`, kèm theo **Email Subscription** để gửi thông báo cho người phụ trách quy trình.

### Tạo Amazon EventBridge Rules để gửi thông báo

Hai **EventBridge rule** được tạo để gửi thông báo qua SNS:

**Rule 1 – Thông báo khi workflow chờ phê duyệt (WaitForAction):**

Khi [EC2 Image Builder](https://aws.amazon.com/image-builder/) phát sự kiện "Workflow Step Waiting" cho bước **CheckBuildLogsAndApprove**, rule này gửi thông điệp đến SNS Topic `ImageBuilderQueue`.

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: CloudFormation template for EventBridge Rule ec2-imagebuilder
Resources:
  Rule41742a2e:
    Type: AWS::Events::Rule
    Properties:
      Name: ec2-imagebuilder
      EventPattern: >-
        {"source":["aws.imagebuilder"],"detail-type":["EC2 Image Builder Workflow Step Waiting"],
         "detail":{"workflow-step-name":["CheckBuildLogsAndApprove"]}}
      State: ENABLED
      EventBusName: default
      Targets:
        - Arn: !Sub "arn:${AWS::Partition}:sns:${AWS::Region}:${AWS::AccountId}:ImageBuilderQueue"
```

**Rule 2 – Thông báo khi log CIS được tạo trong S3 bucket:**

Khi một object `CIS-LBK.log` được tạo trong đường dẫn `/logs/.../`, EventBridge sẽ gửi thông báo đến SNS topic `ImageBuilderQueue`.

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: CloudFormation template for EventBridge Rule CISHardeningExitLogEvent
Resources:
  Rule104d463b:
    Type: AWS::Events::Rule
    Properties:
      Name: CISHardeningExitLogEvent
      EventPattern: >-
        {"source":["aws.s3"],"detail-type":["Object Created"],
         "detail":{"bucket":{"name":["cis-lbk-al2"]},
         "object":{"key":[{"wildcard":"logs/*CIS-LBK.log"}]}}}
      State: ENABLED
      EventBusName: default
      Targets:
        - Arn: !Sub "arn:${AWS::Partition}:sns:${AWS::Region}:${AWS::AccountId}:ImageBuilderQueue"
          InputTransformer:
            InputPathsMap:
              bucketname: $.detail.bucket.name
              objectkey: $.detail.object.key
              region: $.region
            InputTemplate: >-
              "Review the output log for the CIS Hardening process in <bucketname> bucket at <objectkey>.
               Log URL: https://<bucketname>.s3.<region>.amazonaws.com/<objectkey>"
```

### Chạy Image Pipeline

Bạn có thể khởi chạy pipeline từ **AWS Console** hoặc **[AWS CLI](https://aws.amazon.com/cli/)**.

Lệnh CLI mẫu:

```bash
aws imagebuilder start-image-pipeline-execution \
--image-pipeline-arn arn:aws:imagebuilder:{REGION}:{ACCOUNT_ID}:image-pipeline/CIS-AL2-L1S-Hardening-pipeline
```

Kết quả trả về:

```json
{
  "requestId": "a1b2c3d4-5678-90ab-cdef-EXAMPLE11111",
  "clientToken": "a1b2c3d4-5678-90ab-cdef-EXAMPLE22222",
  "imageBuildVersionArn": "arn:aws:imagebuilder:{REGION}:{ACCOUNT_ID}:image/cis-al2-l1s-hardening/1.0.1/1"
}
```

Ghi chú `imageBuildVersionArn` và `stepExecutionId`. Pipeline sẽ **tạm dừng** tại bước `CheckBuildLogsAndApprove`.

Kiểm tra trạng thái chờ qua lệnh:

```bash
aws imagebuilder list-waiting-workflow-step
```

Khi chạy thành công, bạn sẽ nhận **2 email thông báo**:

* Một email từ **S3 EventBridge** chứa link log file CIS-LBK.
* Một email từ **Image Builder** yêu cầu phê duyệt bước chờ.

Ví dụ nội dung log notification:

```
"Review the output log for the CIS Hardening process in 'cis-lbk-al2' bucket, location 'logs/08_27_2024_1210/CIS-LBK.log'..."
```

---

## Kiểm thử Image bằng Amazon Inspector

Trong giai đoạn chờ, bạn tạo **CIS scan** một lần (one-time) bằng [Amazon Inspector](https://aws.amazon.com/inspector/):

```bash
aws inspector2 create-cis-scan-configuration \
--scan-name cis-hardened-scan \
--schedule oneTime={} \
--security-level "LEVEL_1" \
--targets "accountIds=<<account_id>>,targetResourceTags={IBCISHardened=Yes}"
```

Theo dõi tiến trình scan:

```bash
aws inspector2 list-cis-scans \
--filter-criteria "scanConfigurationArnFilters=[{comparison=EQUALS,value=<<scanConfigurationArn>>}]"
```

Sau khi hoàn tất, tải **báo cáo PDF**:

```bash
aws inspector2 get-cis-scan-report --scan-arn <<scan-arn>> --report-format PDF
```

Kết quả trả về URL của báo cáo, có thể mở bằng trình duyệt đã đăng nhập AWS.

### Tiếp tục hoặc dừng workflow

Sau khi xem log và báo cáo:

**Tiếp tục pipeline:**

```bash
aws imagebuilder send-workflow-step-action \
--step-execution-id <<stepExecutionId>> \
--image-build-version-arn <<imageBuildVersionArn>> \
--action RESUME
```

**Hoặc dừng pipeline:**

```bash
aws imagebuilder send-workflow-step-action \
--step-execution-id <<stepExecutionId>> \
--image-build-version-arn <<imageBuildVersionArn>> \
--action STOP \
--reason "Please fix the issue and recreate the Image"
```

Khi hoàn tất, xem AMI output:

```bash
aws imagebuilder get-image \
--image-build-version-arn <<imageBuildVersionArn>> \
--query 'image.outputResources.amis[0].image'
```

---

## Kết luận

Trong bài viết này, chúng tôi đã thiết lập quy trình dựa trên [EC2 Image Builder](https://aws.amazon.com/image-builder/) để harden AMI [Amazon Linux 2](https://aws.amazon.com/amazon-linux-2/) theo CIS [Amazon Linux 2](https://aws.amazon.com/amazon-linux-2/) Benchmark bằng cách sử dụng CIS Linux Build Kit do cộng đồng CIS cung cấp.

Chúng tôi cũng tận dụng thông báo dựa trên sự kiện (event-based) từ [Amazon S3](https://aws.amazon.com/s3/) và [Amazon EventBridge](https://aws.amazon.com/eventbridge/) để kích hoạt email phê duyệt và giới thiệu quy trình manual approval.

Framework này có thể mở rộng để harden các AMI khác như **Ubuntu** hoặc **CentOS**, sử dụng các bộ LBK tương ứng từ cộng đồng CIS.

Hãy liên hệ với [đại diện AWS](https://aws.amazon.com/contact-us/sales-support-cloudops/) để được hỗ trợ triển khai quy trình này cho doanh nghiệp của bạn.

---

## Đọc thêm

* [CIS hardening components for EC2 Image Builder](https://docs.aws.amazon.com/imagebuilder/latest/userguide/toe-cis.html)
* [Build a pipeline for hardened container images using EC2 Image Builder and Terraform](https://aws.amazon.com/blogs/containers/build-a-pipeline-for-hardened-container-images-using-ec2-image-builder-and-terraform/)
* [Amazon Inspector Best Practices](https://docs.aws.amazon.com/inspector/latest/user/inspector_best_practices.html)

---

## Về tác giả

**Rajat Chatterjee**

Rajat là Kiến trúc sư Giải pháp cấp cao (Senior Solutions Architect), làm việc cùng các khách hàng trong lĩnh vực Dịch vụ Tài chính bao gồm Ngân hàng, Bảo hiểm và Thị trường Vốn, nhằm hỗ trợ họ xây dựng các ứng dụng đáng tin cậy, an toàn và hiệu năng cao trên nền tảng đám mây.

Anh có chuyên môn trong việc triển khai quy mô lớn các khối lượng công việc dựa trên microservices trên nền tảng container dành cho các doanh nghiệp.
