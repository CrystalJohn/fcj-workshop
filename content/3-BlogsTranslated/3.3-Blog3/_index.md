---
title: "Building Golden Images with CIS Linux Build Kit in Amazon EC2 Image Builder"
date: 2025-05-11
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

## By: Rajat Chatterjee

**Date:** May 11, 2025

**Topics:** [Amazon Inspector](https://aws.amazon.com/blogs/mt/category/security-identity-compliance/amazon-inspector/), [Enterprise Governance and Control](https://aws.amazon.com/blogs/mt/category/management-and-governance/enterprise-governance-and-control/), [Expert (400)](https://aws.amazon.com/blogs/mt/category/learning-levels/expert-400/), [Management Tools](https://aws.amazon.com/blogs/mt/category/management-tools/), [Technical How-to](https://aws.amazon.com/blogs/mt/category/post-types/technical-how-to/)

---

## Introduction

Building and deploying hardened and security-certified operating systems (OS) is an essential requirement for any Cloud Operations (CloudOps) or Cloud Center of Excellence (CCoE) team in an organization. The security guidelines and controls used to certify these images typically come from internal security teams, based on widely recognized industry standards.

However, you also need the ability to control the hardening process, with flexibility to choose appropriate remediation steps and application deployment scenarios. The result is the formation of "[Golden Amazon Machine Images](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html) (AMI)" – images that Business Units use directly.

This process requires automation, so that building and testing can occur at scale and images are distributed through approved [Amazon Machine Images](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html) (AMI). One of the popular standards used as a baseline for image hardening is the [Center for Internet Security](https://www.cisecurity.org/) (CIS) — an organization that creates and maintains a set of configuration guidelines called [CIS Benchmarks](https://www.cisecurity.org/benchmark), providing best practice configurations for specific technologies such as operating systems, cloud platforms, applications, and databases.

[CIS Benchmarks](https://www.cisecurity.org/benchmark) are widely recognized in the industry and referenced by many standards such as PCI DSS, HIPAA, DoD Cloud Computing SRG, FISMA, DFARS, and FEDRAMP.

In this post, we choose the CIS benchmark for Amazon Linux, and build a process to create certified AMIs through automation that can operate at scale.

You can use one of two approaches to create Golden Images by applying [CIS Benchmark](https://www.cisecurity.org/cis-benchmarks) for the corresponding operating system (OS) to harden the AMI:

1. **Use managed components with CIS AMI from [AWS Marketplace](https://aws.amazon.com/marketplace/)** – You will also have access to the accompanying hardening component, which runs scripts to apply [CIS Benchmark](https://www.cisecurity.org/cis-benchmarks) Level 1 guidelines to your configuration. These components are owned and maintained by CIS to ensure they are always updated with the latest guidelines. (Details on this approach can be found in the post [Building CIS hardened Golden Images and Pipelines with EC2 Image Builder](https://aws.amazon.com/blogs/mt/building-cis-hardened-golden-images-and-pipelines-with-ec2-image-builder/)).

2. **Use a self-managed hardening process with [Amazon EC2 Image Builder](https://aws.amazon.com/image-builder/)** – This approach provides flexibility for CloudOps teams to customize the hardening process, allowing them to exclude benchmark recommendations that may affect applications intended for deployment on instances using the AMI.

You can choose one of these two options after carefully analyzing the factors explained in the post [How to Decide Between Building or Buying a CIS Hardened Image](https://aws.amazon.com/blogs/apn/how-to-decide-between-building-or-buying-a-cis-hardened-image/).

In this post, we will demonstrate how to use scripts published by CIS Linux Build Kit (LBK) to create a self-managed hardening and validation process using [EC2 Image Builder](https://aws.amazon.com/image-builder/) and [Amazon Inspector](https://aws.amazon.com/inspector/). The scripts needed to harden a specific operating system (according to [CIS Benchmark](https://www.cisecurity.org/cis-benchmarks)) are published on the CIS Workbench page, downloadable if your organization is a CIS SecureSuite® member. These scripts can be reused with minimal modifications to create [EC2 Image Builder](https://aws.amazon.com/image-builder/) Components, then used in Image Builder Pipelines.

We will implement this approach by creating a hardened [Amazon Linux 2](https://aws.amazon.com/amazon-linux-2/) AMI, and validating it using [Amazon Inspector](https://aws.amazon.com/inspector/).

---

## Solution overview

The solution consists of the following main steps:

**Prerequisites:**

1. Have an AWS account with appropriate permissions.
2. Download, explore, and update the CIS LBK Utility.
3. Create an [Amazon S3](https://aws.amazon.com/s3/) bucket as a staging environment to upload the CIS LBK Utility.
4. Configure the S3 bucket to send notifications via [Amazon EventBridge](https://aws.amazon.com/eventbridge/).

**Main deployment steps:**

1. Build the Image Building Pipeline using [EC2 Image Builder](https://aws.amazon.com/image-builder/).
2. Validate the image and generate reports using [Amazon Inspector](https://aws.amazon.com/inspector/).

---

## Solution prerequisites

### Download, explore, and update CIS LBK Utility

The LBK for [Amazon Linux 2](https://aws.amazon.com/amazon-linux-2/) can be downloaded from the CIS website. This toolkit is developed and maintained by the CIS community, currently distributed as the file `amazon_linux_2.tar.gz`.

After extracting the tar file, you will see the following directory structure:

![CIS Linux Build Kit folder structure](/images/3-BlogsTranslated/3.3-Blog3/cis-folders.jpg)

*Figure 1: CIS Linux Build Kit folder structure*

The main directory contains the file `amazon_linux_2.sh` — the main script that calls sub-scripts in the "functions" directory. If you want to exclude certain recommendations, add them to the `exclusion_list.txt` file.

Usage instructions and toolkit contents are described in detail in the Quick Start Guide included in the package. The `amazon_linux_2.sh` script includes two interactive steps when running. For automation, you should modify this script to hardcode the input values.

Specifically, you need to:

1. **Accept the Terms and Conditions** – you should comment out this line in the script, and the CloudOps team can provide separate terms content for users in internal documentation.

2. **Select the hardening profile** – there are four profiles: L1S, L2S, L1W, L2W corresponding to Level 1/2 of [CIS Benchmark](https://www.cisecurity.org/cis-benchmarks), and Server (S) or Workstation (W) type.

In this example, we will use the L1S profile to harden [Amazon Linux 2](https://aws.amazon.com/amazon-linux-2/) according to [CIS Benchmark](https://www.cisecurity.org/cis-benchmarks) Level 1.

Lines to modify in the script:

```bash
#terms_of_use
#Display CIS Linux Build Kit warning banner
#WARBNR ---> Comment this line
run_profile=L1S # Uncomment this line to specify the profile to run
```

After editing, repackage the LBK file:

```bash
tar cvf amazon_linux_2.tar.gz CIS-LBK
```

### Create staging S3 bucket and upload LBK

Create an S3 bucket (e.g., `cis-lbk-al2`) using AWS Management Console or [AWS CLI](https://aws.amazon.com/cli/). This bucket is used to store the LBK script, serving [EC2 Image Builder](https://aws.amazon.com/image-builder/) for downloading and uploading logs after execution.

```bash
aws s3api put-object --bucket cis-lbk-al2 --key amazon_linux_2.tar.gz --body amazon_linux_2.tar.gz
```

### Configure S3 bucket to send notifications via Amazon EventBridge

Enable [Amazon S3](https://aws.amazon.com/s3/) Event Notifications for the bucket, selecting to send notifications to [Amazon EventBridge](https://aws.amazon.com/eventbridge/). You can enable this feature in the Properties tab of the bucket.

![Enable sending notifications to EventBridge from S3 bucket](/images/3-BlogsTranslated/3.3-Blog3/s3-eventbridge.jpg)

*Figure 2: Enable sending notifications to EventBridge from S3 bucket*

---

## Deployment steps

![Solution architecture](/images/3-BlogsTranslated/3.3-Blog3/arch-diagram.jpg)

*Figure 3: Solution architecture*

---

## Building Image Building Pipeline with EC2 Image Builder

This solution uses the following AWS services:

* **[EC2 Image Builder](https://aws.amazon.com/image-builder/)**: runs LBK from [Amazon S3](https://aws.amazon.com/s3/) to harden and package the AMI. [EC2 Image Builder](https://aws.amazon.com/image-builder/) uses AWS Task Orchestrator and Executor ([AWSTOE](https://docs.aws.amazon.com/imagebuilder/latest/userguide/toe-component-manager.html)) to orchestrate complex workflows. You will create a [custom component](https://docs.aws.amazon.com/imagebuilder/latest/userguide/create-custom-components.html) to call LBK from S3, while defining an image recipe as the base image and an [image workflow](https://docs.aws.amazon.com/imagebuilder/latest/userguide/manage-image-workflows.html) that defines the sequence of build steps.

* **[Amazon Inspector](https://aws.amazon.com/inspector/)**: performs security checks (CIS scan) to validate the AMI after hardening.

* **[Amazon EventBridge](https://aws.amazon.com/eventbridge/)**: integrates natively with [EC2 Image Builder](https://aws.amazon.com/image-builder/) and S3 to emit events. These events can be forwarded to [Amazon SNS](https://aws.amazon.com/sns/) or [AWS Lambda](https://aws.amazon.com/lambda/).

* **[Amazon SNS](https://aws.amazon.com/sns/)**: used to send email notifications about approval steps.

### Create custom Component for EC2 Image Builder

You can use an [AWS CloudFormation](https://aws.amazon.com/cloudformation/) template (provided in the original post) to initialize a stack that creates all resources mentioned in the sections below.

Example command to deploy the stack:

```bash
aws cloudformation create-stack --stack-name cis-al2-harden-stack \
--template-body file://cis-lbk-automation-template.yml \
--parameters
```

This CloudFormation template will automatically create the components described in the solution architecture.

In this post, we define a [custom component](https://docs.aws.amazon.com/imagebuilder/latest/userguide/create-custom-components.html) named **CIS-AL2-L1S-Hardening**, with the following characteristics:

* **Type**: build
* **Platform**: Linux and compatible operating systems
* **OS Version**: [Amazon Linux 2](https://aws.amazon.com/amazon-linux-2/)

Each component is defined by a YAML document, where the S3 bucket created in the prerequisite step is used to download scripts and upload logs. The following table lists a series of steps in the build phase.

![Build phase steps](/images/3-BlogsTranslated/3.3-Blog3/workflow-steps.jpg)

*Figure 4: Build phase steps*

Example component definition:

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

### Create Image Recipe based on Component

An Image Recipe is a document that defines the **components** applied to a **base image** to create the desired configuration for the **output image**.

The recipe named **CIS-AL2-L1S-Hardening-Recipe** is created using the **custom component CIS-AL2-L1S-Hardening** as the main build phase.

**Sample AWS CloudFormation template** is as follows:

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

As you can see, the **custom component ([custom component](https://docs.aws.amazon.com/imagebuilder/latest/userguide/create-custom-components.html))** is referenced in the recipe, while the recipe also references the **latest version of [Amazon Linux 2](https://aws.amazon.com/amazon-linux-2/)** as the **base image** through the ParentImage property.

The **CIS-AL2-L1S-Hardening** component takes **WorkingDirectory** as input.

{{% notice note %}}
This value must be set to `/var/tmp`, because the default `/tmp` directory will have its permissions changed by the CIS LBK script, causing access errors when running.
{{% /notice %}}

### Create custom Workflow for Image Build process

**Image Workflow** defines the sequence of steps [EC2 Image Builder](https://aws.amazon.com/image-builder/) performs during the **build** and **test** phases of the image creation process.

This workflow introduces the **WaitForAction** action, which adds a **manual approval** step after the hardening process completes. At this step, operators can review logs and continue creating the image. This is also when you can trigger and view **[Amazon Inspector](https://aws.amazon.com/inspector/) scan** results.

The **WaitForAction** action pauses the running workflow and waits for a response from the **SendWorkflowStepAction** API of [EC2 Image Builder](https://aws.amazon.com/image-builder/). This event emits an **EventBridge event** with detail-type "[EC2 Image Builder](https://aws.amazon.com/image-builder/) Workflow Step Waiting".

For this blog post, the authors create a **custom workflow** named **build-cis-hardened-manual-action**, type **BUILD**, with the following YAML content:

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

### Create Image Pipeline

In the blog example, the authors create an **Image Pipeline** named **CIS-AL2-L1S-Hardening-pipeline**.

[EC2 Image Builder](https://aws.amazon.com/image-builder/) uses **[Amazon Inspector](https://aws.amazon.com/inspector/)** to scan test instances for security vulnerabilities in the operating system or software packages. However, in this case, the default vulnerability scanning feature is **disabled**, as the team only uses [Amazon Inspector](https://aws.amazon.com/inspector/) to **perform CIS scan** on the **output image**, not general vulnerability scanning.

The **Build Schedule** is set to **Manual** mode. You can schedule it periodically using **Schedule Builder (CRON expression)**.

Next, the team selects the **Image Recipe (CIS-AL2-L1S-Hardening-Recipe)** created in the previous step. The image creation process is defined using a **Custom Workflow**, where **build-cis-hardened-manual-action** is selected as the **Build workflow**.

The IAM Role selected is **AWSServiceRoleForImageBuilder**, which provides the necessary access for [EC2 Image Builder](https://aws.amazon.com/image-builder/).

The Infrastructure Configuration is kept at default, except for adding a **Resource Tag** for the instance:
- **Key**: `IBCISHardened`
- **Value**: `Yes`

→ This tag is used in the [Amazon Inspector](https://aws.amazon.com/inspector/) configuration as a filter criterion for CIS scanning.

**Distribution Settings** are kept at default – meaning [EC2 Image Builder](https://aws.amazon.com/image-builder/) will launch instances in the **default VPC**, and **distribute the output AMI** in the **current Region**.

This **Infrastructure Configuration** section creates the IAM Role and Instance Profile used by the build instance to configure the AMI. This Instance Role is attached to **EC2InstanceProfileForImageBuilder** and **AmazonSSMManagedInstanceCore managed policy**.

To successfully run **[Amazon Inspector](https://aws.amazon.com/inspector/) CIS scan**, the **Instance Profile** needs to attach the additional policy **AmazonInspector2ManagedCisPolicy**.

The IAM Instance Profile Role configuration is as follows:

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

### Attach Bucket Policy to S3 Bucket cis-lbk-al2

This policy only allows the IAM Role of the Instance Profile to access the bucket:

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

### Create Amazon SNS Topic for Email Notification

The team creates an **SNS Topic** named `ImageBuilderQueue`, along with an **Email Subscription** to send notifications to the person responsible for the process.

### Create Amazon EventBridge Rules to send notifications

Two **EventBridge rules** are created to send notifications via SNS:

**Rule 1 – Notification when workflow is waiting for approval (WaitForAction):**

When [EC2 Image Builder](https://aws.amazon.com/image-builder/) emits a "Workflow Step Waiting" event for the **CheckBuildLogsAndApprove** step, this rule sends a message to the SNS Topic `ImageBuilderQueue`.

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

**Rule 2 – Notification when CIS log is created in S3 bucket:**

When a `CIS-LBK.log` object is created in the `/logs/.../` path, EventBridge sends a notification to the SNS topic `ImageBuilderQueue`.

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

### Run Image Pipeline

You can launch the pipeline from **AWS Console** or **[AWS CLI](https://aws.amazon.com/cli/)**.

Sample CLI command:

```bash
aws imagebuilder start-image-pipeline-execution \
--image-pipeline-arn arn:aws:imagebuilder:{REGION}:{ACCOUNT_ID}:image-pipeline/CIS-AL2-L1S-Hardening-pipeline
```

Response:

```json
{
  "requestId": "a1b2c3d4-5678-90ab-cdef-EXAMPLE11111",
  "clientToken": "a1b2c3d4-5678-90ab-cdef-EXAMPLE22222",
  "imageBuildVersionArn": "arn:aws:imagebuilder:{REGION}:{ACCOUNT_ID}:image/cis-al2-l1s-hardening/1.0.1/1"
}
```

Note the `imageBuildVersionArn` and `stepExecutionId`. The pipeline will **pause** at the `CheckBuildLogsAndApprove` step.

Check the waiting status with the command:

```bash
aws imagebuilder list-waiting-workflow-step
```

When running successfully, you will receive **2 email notifications**:

* One email from **S3 EventBridge** containing the CIS-LBK log file link.
* One email from **Image Builder** requesting approval for the waiting step.

Example log notification content:

```
"Review the output log for the CIS Hardening process in 'cis-lbk-al2' bucket, location 'logs/08_27_2024_1210/CIS-LBK.log'..."
```

---

## Testing Image with Amazon Inspector

During the waiting phase, you create a **one-time CIS scan** using [Amazon Inspector](https://aws.amazon.com/inspector/):

```bash
aws inspector2 create-cis-scan-configuration \
--scan-name cis-hardened-scan \
--schedule oneTime={} \
--security-level "LEVEL_1" \
--targets "accountIds=<<account_id>>,targetResourceTags={IBCISHardened=Yes}"
```

Monitor scan progress:

```bash
aws inspector2 list-cis-scans \
--filter-criteria "scanConfigurationArnFilters=[{comparison=EQUALS,value=<<scanConfigurationArn>>}]"
```

After completion, download the **PDF report**:

```bash
aws inspector2 get-cis-scan-report --scan-arn <<scan-arn>> --report-format PDF
```

The result returns the URL of the report, which can be opened in a browser logged into AWS.

### Continue or stop workflow

After reviewing logs and reports:

**Continue pipeline:**

```bash
aws imagebuilder send-workflow-step-action \
--step-execution-id <<stepExecutionId>> \
--image-build-version-arn <<imageBuildVersionArn>> \
--action RESUME
```

**Or stop pipeline:**

```bash
aws imagebuilder send-workflow-step-action \
--step-execution-id <<stepExecutionId>> \
--image-build-version-arn <<imageBuildVersionArn>> \
--action STOP \
--reason "Please fix the issue and recreate the Image"
```

When complete, view the output AMI:

```bash
aws imagebuilder get-image \
--image-build-version-arn <<imageBuildVersionArn>> \
--query 'image.outputResources.amis[0].image'
```

---

## Conclusion

In this post, we set up a process based on [EC2 Image Builder](https://aws.amazon.com/image-builder/) to harden an [Amazon Linux 2](https://aws.amazon.com/amazon-linux-2/) AMI according to the CIS [Amazon Linux 2](https://aws.amazon.com/amazon-linux-2/) Benchmark using the CIS Linux Build Kit provided by the CIS community.

We also leveraged event-based notifications from [Amazon S3](https://aws.amazon.com/s3/) and [Amazon EventBridge](https://aws.amazon.com/eventbridge/) to trigger approval emails and introduce a manual approval process.

This framework can be extended to harden other AMIs such as **Ubuntu** or **CentOS**, using the corresponding LBK kits from the CIS community.

Contact your [AWS representative](https://aws.amazon.com/contact-us/sales-support-cloudops/) for support in implementing this process for your organization.

---

## Further reading

* [CIS hardening components for EC2 Image Builder](https://docs.aws.amazon.com/imagebuilder/latest/userguide/toe-cis.html)
* [Build a pipeline for hardened container images using EC2 Image Builder and Terraform](https://aws.amazon.com/blogs/containers/build-a-pipeline-for-hardened-container-images-using-ec2-image-builder-and-terraform/)
* [Amazon Inspector Best Practices](https://docs.aws.amazon.com/inspector/latest/user/inspector_best_practices.html)

---

## About the author

**Rajat Chatterjee**

Rajat is a Senior Solutions Architect, working with customers in the Financial Services sector including Banking, Insurance, and Capital Markets, helping them build reliable, secure, and high-performance applications on the cloud.

He has expertise in large-scale deployments of microservices-based workloads on container platforms for enterprises.
