---
title: "Xây dựng nhà khí tượng ảo bằng Amazon Bedrock Agents"
date: 2025-02-11
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

## Bởi: Salman Ahmed, Ankush Goyal, Sergio Barraza, và Ravi Kumar

**Ngày:** 11 FEB 2025

**Chủ đề:** [Amazon Bedrock](https://aws.amazon.com/bedrock/), [Amazon Bedrock Agents](https://aws.amazon.com/bedrock/agents/), [Amazon Cognito](https://aws.amazon.com/cognito/), [AWS Amplify](https://aws.amazon.com/amplify/), [AWS Lambda](https://aws.amazon.com/lambda/), [Generative AI](https://aws.amazon.com/blogs/machine-learning/category/artificial-intelligence/generative-ai/), Intermediate (200)

---

## Giới thiệu

Việc tích hợp các khả năng generative AI đang thúc đẩy những thay đổi mang tính chuyển hóa trên nhiều ngành. Mặc dù thông tin thời tiết có thể truy cập qua nhiều kênh, các doanh nghiệp phụ thuộc nhiều vào dữ liệu khí tượng cần những giải pháp vững chắc và có khả năng mở rộng để quản lý và khai thác hiệu quả các insight quan trọng này, đồng thời giảm các quy trình thủ công. Giải pháp trong bài viết này minh họa cách xây dựng một nhà khí tượng ảo vận hành bằng AI có thể trả lời các truy vấn phức tạp liên quan đến thời tiết bằng ngôn ngữ tự nhiên. Chúng tôi sử dụng nhiều dịch vụ AWS để triển khai một giải pháp hoàn chỉnh mà bạn có thể dùng để tương tác với một API cung cấp dữ liệu thời tiết theo thời gian thực. Trong giải pháp này, chúng tôi sử dụng [Amazon Bedrock Agents](https://aws.amazon.com/bedrock/agents/).

[Amazon Bedrock Agents](https://aws.amazon.com/bedrock/agents/) giúp tinh giản quy trình làm việc và tự động hóa các tác vụ lặp lại. [Amazon Bedrock Agents](https://aws.amazon.com/bedrock/agents/) có thể kết nối an toàn tới các nguồn dữ liệu của doanh nghiệp bạn và bổ sung cho yêu cầu của người dùng bằng các phản hồi chính xác. Bạn có thể dùng [Amazon Bedrock Agents](https://aws.amazon.com/bedrock/agents/) để thiết kế một action schema phù hợp yêu cầu, qua đó kiểm soát được mỗi khi agent khởi tạo hành động được chỉ định. Cách tiếp cận linh hoạt này cho phép bạn tích hợp và thực thi logic nghiệp vụ trong dịch vụ backend ưa dùng, tạo nên một sự kết hợp hài hòa giữa tính năng và tính linh hoạt. Hệ thống cũng có khả năng ghi nhớ xuyên suốt tương tác, mang lại trải nghiệm được cá nhân hóa hơn cho người dùng.

Trong bài viết này, chúng tôi giới thiệu một cách tiếp cận tinh gọn để triển khai agent dùng AI bằng cách kết hợp [Amazon Bedrock Agents](https://aws.amazon.com/bedrock/agents/) và một foundation model (FM). Chúng tôi hướng dẫn bạn quy trình cấu hình agent và hiện thực phần logic chuyên biệt cần thiết để nhà khí tượng ảo đưa ra các phản hồi chính xác liên quan đến thời tiết. Ngoài ra, chúng tôi còn sử dụng nhiều dịch vụ AWS, bao gồm AWS Amplify để lưu trữ front end, các hàm [AWS Lambda](https://aws.amazon.com/lambda/) để xử lý logic yêu cầu, Amazon Cognito để xác thực người dùng, và AWS Identity and Access Management (IAM) để kiểm soát quyền truy cập vào agent.

---

## Tổng quan giải pháp

Sơ đồ dưới đây cung cấp cái nhìn tổng thể và làm nổi bật các thành phần chính của hệ thống.
![virtual-meteorologist-figure-1](/images/3-BlogsTranslated/3.1-Blog1/virtual-meteorologist-figure-1.png)
Kiến trúc sử dụng **Amazon Cognito** để xác thực người dùng và **AWS Amplify** làm môi trường lưu trữ cho ứng dụng giao diện front-end.

[**Amazon Bedrock Agents**](https://aws.amazon.com/bedrock/agents/) tiếp nhận truy vấn từ người dùng và chuyển thông tin đến các **action group**, mỗi nhóm này sẽ kích hoạt một **Lambda function** tùy chỉnh tương ứng. Mỗi action group và Lambda function xử lý một nhiệm vụ riêng biệt:

- **geo-coordinates** – Xử lý thông tin tọa độ địa lý để lấy dữ liệu về một địa điểm cụ thể.
- **weather** – Thu thập thông tin thời tiết cho vị trí đã được cung cấp.
- **date-time** – Xác định ngày và giờ hiện tại theo múi thời gian phù hợp.

Giải pháp tận dụng tính năng tự động mở rộng, bảo mật tích hợp và mô hình serverless của AWS để giảm thiểu chi phí vận hành trong khi vẫn đảm bảo khả năng phản hồi thời gian thực.

---

## Các yêu cầu tiên quyết

Để triển khai đầy đủ giải pháp này, bạn cần chuẩn bị những thành phần sau:

- Một **tài khoản AWS** đang hoạt động.
- Quyền truy cập vào **foundation model (FM)** trong [**Amazon Bedrock**](https://aws.amazon.com/bedrock/), cụ thể là [**Anthropic Claude**](https://aws.amazon.com/bedrock/claude/) **3.5 Sonnet**, tại cùng khu vực (Region) mà bạn sẽ triển khai giải pháp.
- Tải xuống **mẫu [AWS CloudFormation](https://aws.amazon.com/cloudformation/)** đi kèm từ [**aws-samples GitHub repository**](https://github.com/aws-samples) để tự động khởi tạo tài nguyên.

---

## Triển khai tài nguyên bằng AWS CloudFormation

Sau khi chuẩn bị các yêu cầu tiên quyết, bạn sẽ triển khai toàn bộ tài nguyên cần thiết bằng [**AWS CloudFormation**](https://aws.amazon.com/cloudformation/). Khi bạn chạy mẫu CloudFormation, hệ thống sẽ tự động tạo ra các thành phần như sau (lưu ý rằng các dịch vụ AWS sử dụng trong giải pháp này sẽ phát sinh chi phí):

### Amazon Cognito

- [User pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools.html): CognitoUserPoolforVirtualMeteorologistApp
- [App client](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-settings-client-apps.html): VirtualMeteorologistApp
- [Identity pool](https://docs.aws.amazon.com/cognito/latest/developerguide/identity-pools.html): cognito-identity-pool-vm

### AWS Lambda

- `<Stack name>-geo-coordinates-<auto-generated>`
- `<Stack name>-weather-<auto-generated>`
- `<Stack name>-date-time-<auto-generated>`

### Amazon Bedrock Agents

- Agent: virtual-meteorologist
- Action group (1): obtain-latitude-longitude-from-place-name
- Action group (2): obtain-weather-information-with-coordinates
- Action group (3): get-current-date-time-from-timezone

Sau khi stack CloudFormation được triển khai thành công, bạn cần sao chép các giá trị từ tab **Outputs** trong [**AWS CloudFormation**](https://aws.amazon.com/cloudformation/) **Console** để cấu hình ứng dụng trong **AWS Amplify**. Các giá trị cần lưu lại bao gồm:

- AWSRegion
- BedrockAgentAliasId
- BedrockAgentId
- BedrockAgentName
- IdentityPoolId
- UserPoolClientId
- UserPoolId
![virtual-meteorologist-figure-2](/images/3-BlogsTranslated/3.1-Blog1/virtual-meteorologist-figure-2.png)
---

## Triển khai ứng dụng AWS Amplify

Ứng dụng giao diện người dùng được lưu trữ trong tệp AWS-Amplify-Frontend.zip có sẵn trên [**GitHub**](https://github.com/aws-samples).

Bạn cần thực hiện các bước sau để triển khai thủ công ứng dụng:

1. Tải tệp mã nguồn **AWS-Amplify-Frontend.zip** từ kho [**GitHub**](https://github.com/aws-samples).
2. Sử dụng tệp .zip này để [**triển khai**](https://docs.aws.amazon.com/amplify/latest/userguide/manual-deploys.html) thủ công ứng dụng trong [AWS Amplify](https://aws.amazon.com/amplify/).
3. Sau khi triển khai hoàn tất, quay lại trang **AWS Amplify Console** và sử dụng **tên miền (domain)** được tạo tự động để **truy cập ứng dụng web**.

---

## Sử dụng Amazon Cognito để xác thực người dùng

[Amazon Cognito](https://aws.amazon.com/cognito/) là dịch vụ quản lý danh tính (identity service) cho phép bạn xác thực (authenticate) và phân quyền (authorize) người dùng. Trong giải pháp này, chúng tôi sử dụng Amazon Cognito để xác minh danh tính người dùng trước khi họ có thể truy cập và sử dụng ứng dụng. Bên cạnh đó, chúng tôi còn sử dụng Identity Pool để cấp các thông tin xác thực tạm thời (temporary AWS credentials) cho người dùng trong quá trình họ tương tác với [**Amazon Bedrock API**](https://aws.amazon.com/bedrock/).

---

## Sử dụng Amazon Bedrock Agents để tự động hóa các tác vụ trong ứng dụng

Với [**Amazon Bedrock Agents**](https://aws.amazon.com/bedrock/agents/), bạn có thể xây dựng và cấu hình các **agent tự động** trong ứng dụng của mình. Một agent giúp người dùng cuối thực hiện các hành động dựa trên dữ liệu của tổ chức và thông tin mà họ cung cấp. Các agent này điều phối toàn bộ quá trình tương tác giữa **foundation models (FMs)**, nguồn dữ liệu, phần mềm ứng dụng và cuộc hội thoại với người dùng. Nhờ đó, hệ thống có thể hiểu và thực thi các yêu cầu phức tạp theo ngữ cảnh tự nhiên, đồng thời tự động hóa các quy trình xử lý dữ liệu một cách hiệu quả.

---

## Sử dụng Action Group để định nghĩa các hành động mà Amazon Bedrock Agent thực hiện

Một action group xác định tập hợp các hành động có liên quan mà một [Amazon Bedrock](https://aws.amazon.com/bedrock/) Agent có thể thực hiện để hỗ trợ người dùng. Khi cấu hình một action group, bạn có nhiều lựa chọn để xử lý dữ liệu do người dùng cung cấp, bao gồm: thêm thông tin đầu vào của người dùng vào action group của agent, chuyển dữ liệu đến một [AWS Lambda](https://aws.amazon.com/lambda/) để xử lý logic nghiệp vụ tùy chỉnh, hoặc trả quyền điều khiển trực tiếp lại cho ứng dụng thông qua phản hồi từ phương thức InvokeAgent.

Trong ứng dụng này, chúng tôi tạo ra ba action group giúp [Amazon Bedrock](https://aws.amazon.com/bedrock/) Agent có thể thực hiện các chức năng cốt lõi: lấy tọa độ của địa điểm cụ thể, truy xuất ngày giờ hiện tại, và thu thập dữ liệu thời tiết tại địa điểm đó. Các nhóm hành động này cho phép agent truy cập và xử lý những thông tin quan trọng, giúp nâng cao độ chính xác và khả năng phản hồi toàn diện đối với các truy vấn liên quan đến vị trí địa lý và điều kiện thời tiết của người dùng.

---

## Sử dụng AWS Lambda cho các Action Group của Amazon Bedrock

Trong giải pháp này, ba hàm [AWS Lambda](https://aws.amazon.com/lambda/) được triển khai để hỗ trợ các action group mà [Amazon Bedrock](https://aws.amazon.com/bedrock/) Agent định nghĩa:

- **Location Coordinates Lambda Function** – Hàm này được kích hoạt bởi action group obtain-latitude-longitude-from-place-name. Nó nhận tên địa điểm làm đầu vào và trả về tọa độ địa lý (vĩ độ và kinh độ) tương ứng. Hàm sử dụng một dịch vụ hoặc cơ sở dữ liệu định vị (geocoding service) để tìm kiếm thông tin này.
- **Date and Time Lambda Function** – Hàm này được kích hoạt bởi action group get-current-date-time-from-timezone. Nó xác định múi giờ và trả về thông tin ngày giờ hiện tại.
- **Weather Information Lambda Function** – Hàm này được kích hoạt bởi action group obtain-weather-information-with-coordinates. Nó nhận tọa độ được cung cấp từ hàm đầu tiên và truy xuất dữ liệu thời tiết hiện tại hoặc dự báo cho khu vực tương ứng thông qua API thời tiết.

Mỗi hàm Lambda nhận một sự kiện đầu vào (input event) chứa các metadata và trường dữ liệu được điền từ thao tác API của [Amazon Bedrock](https://aws.amazon.com/bedrock/) Agent hoặc các tham số của hàm. Các hàm này xử lý dữ liệu, thực thi nhiệm vụ tương ứng và trả về phản hồi chứa thông tin cần thiết. Phản hồi đó sau đó được [Amazon Bedrock](https://aws.amazon.com/bedrock/) Agent tổng hợp để hình thành câu trả lời hoàn chỉnh cho người dùng.

Nhờ việc sử dụng các hàm Lambda, [Amazon Bedrock](https://aws.amazon.com/bedrock/) Agent có thể truy cập vào các nguồn dữ liệu bên ngoài và thực hiện các phép tính phức tạp, giúp tăng đáng kể năng lực của agent trong việc xử lý các yêu cầu liên quan đến vị trí, thời gian và điều kiện thời tiết.

---

## Sử dụng AWS Amplify cho mã nguồn giao diện

[AWS Amplify](https://aws.amazon.com/amplify/) cung cấp môi trường phát triển cho các ứng dụng web và di động an toàn, có khả năng mở rộng. Các nhà phát triển có thể tập trung vào việc viết mã thay vì quản lý cơ sở hạ tầng. Amplify còn tích hợp với nhiều nền tảng lưu trữ mã nguồn như GitHub, Bitbucket hoặc GitLab. Trong giải pháp này, chúng tôi tải thủ công mã nguồn front-end lên AWS Amplify, theo quy trình đã được mô tả trong phần trước của bài viết.

---

## Trải nghiệm ứng dụng

Sau khi triển khai ứng dụng trong AWS Amplify, bạn có thể truy cập URL được tạo tự động để mở giao diện người dùng. Khi truy cập vào đường dẫn này, ứng dụng sẽ yêu cầu nhập các thông tin liên quan đến Amazon Cognito và [Amazon Bedrock Agents](https://aws.amazon.com/bedrock/agents/). Những thông tin này cần thiết để xác thực người dùng an toàn và cho phép giao diện front-end tương tác với agent trên Bedrock. Nhờ vậy, ứng dụng có thể quản lý phiên đăng nhập (user session) và thực hiện các lệnh gọi API được ủy quyền tới các dịch vụ AWS thay mặt người dùng.

Bạn cần nhập các thông tin được lấy từ phần Outputs của CloudFormation, bao gồm:

- User Pool ID
- User Pool Client ID
- Identity Pool ID
- Region
- Agent Name
- Agent ID
- Agent Alias ID
![virtual-meteorologist-figure-3](/images/3-BlogsTranslated/3.1-Blog1/virtual-meteorologist-figure-3.png)

Đăng nhập bằng tên người dùng và mật khẩu của bạn. Một mật khẩu tạm thời đã được tạo tự động trong quá trình triển khai và gửi đến email bạn đã cung cấp khi chạy mẫu CloudFormation. Ở lần đăng nhập đầu tiên, bạn sẽ được yêu cầu thay đổi mật khẩu để hoàn tất quá trình xác thực.
![virtual-meteorologist-figure-4-2](/images/3-BlogsTranslated/3.1-Blog1/virtual-meteorologist-figure-4-2.gif)
Sau khi đăng nhập thành công, bạn có thể bắt đầu đặt câu hỏi cho ứng dụng, ví dụ:

> "Hôm nay ở Dallas, Texas có thể tổ chức tiệc nướng BBQ được không?"
![virtual-meteorologist-figure-5](/images/3-BlogsTranslated/3.1-Blog1/virtual-meteorologist-figure-5.gif)
Chỉ trong vài giây, ứng dụng sẽ hiển thị câu trả lời chi tiết cho biết liệu hôm nay có thích hợp để tổ chức tiệc BBQ ở Dallas hay không.

---

## Các ví dụ truy vấn mẫu

Dưới đây là một số câu hỏi minh họa khả năng hội thoại của nhà khí tượng ảo:

1. "Thời tiết hôm nay ở New York City như thế nào?"
2. "Tôi có nên lên kế hoạch tổ chức tiệc sinh nhật ngoài trời ở Miami vào cuối tuần tới không?"
3. "Denver có tuyết rơi vào ngày Giáng sinh không?"
4. "Hôm nay tôi có thể đi bơi ở bãi biển Chicago được không?"

Những truy vấn này thể hiện cách mà agent có thể cung cấp thông tin thời tiết hiện tại, tư vấn hoạt động dựa trên dự báo, và dự đoán điều kiện thời tiết trong tương lai. Người dùng có thể hỏi những câu liên quan đến hoạt động cụ thể (như bơi lội, tiệc ngoài trời…), và hệ thống sẽ tự động phân tích điều kiện thời tiết để đưa ra khuyến nghị phù hợp.

---

## Dọn dẹp tài nguyên

Nếu bạn không còn muốn sử dụng nhà khí tượng ảo (virtual meteorologist), hãy thực hiện các bước sau để xóa ứng dụng và toàn bộ tài nguyên liên quan được triển khai bằng [AWS CloudFormation](https://aws.amazon.com/cloudformation/) và [AWS Amplify](https://aws.amazon.com/amplify/):

### 1. Xóa stack CloudFormation

1. Trong AWS CloudFormation Console, chọn **Stacks** trong thanh điều hướng.
2. Xác định stack mà bạn đã tạo trong quá trình triển khai (tên stack được đặt khi khởi tạo).
3. Chọn stack đó và nhấp **Delete** để xóa toàn bộ tài nguyên liên quan.

### 2. Xóa ứng dụng Amplify và các tài nguyên đi kèm

Để biết hướng dẫn chi tiết, vui lòng tham khảo phần [Clean Up Resources](https://aws.amazon.com/getting-started/hands-on/build-web-app-s3-lambda-api-gateway-dynamodb/module-six/) trong tài liệu chính thức của AWS.

---

## Kết luận

Giải pháp này minh họa sức mạnh của việc kết hợp [Amazon Bedrock Agents](https://aws.amazon.com/bedrock/agents/) với các dịch vụ AWS khác để tạo nên một trợ lý thời tiết thông minh, có khả năng hội thoại. Thông qua việc ứng dụng AI và công nghệ điện toán đám mây, doanh nghiệp có thể tự động hóa các truy vấn phức tạp và mang đến cho người dùng các thông tin có giá trị thực tế.

---

## Tài nguyên bổ sung

Để tìm hiểu thêm về [Amazon Bedrock](https://aws.amazon.com/bedrock/), bạn có thể tham khảo:

- [GitHub repo: Amazon Bedrock Workshop](https://github.com/aws-samples/amazon-bedrock-workshop)
- [Amazon Bedrock User Guide](https://docs.aws.amazon.com/bedrock/latest/userguide/what-is-bedrock.html)
- Workshop: Using Generative AI on AWS for diverse content types

Để biết thêm về mô hình Anthropic Claude 3.5 Sonnet, xem tại:

- [Anthropic's Claude in Amazon Bedrock](https://aws.amazon.com/bedrock/claude/)

---

## Về các tác giả

**Salman Ahmed** là Senior Technical Account Manager tại AWS Enterprise Support. Anh hỗ trợ các khách hàng trong ngành du lịch và khách sạn thiết kế, triển khai và vận hành hạ tầng đám mây. Với niềm đam mê về các dịch vụ mạng và nhiều năm kinh nghiệm, anh giúp khách hàng áp dụng hiệu quả các giải pháp AWS Networking. Ngoài công việc, Salman yêu thích nhiếp ảnh, du lịch và thể thao.

**Sergio Barraza** là Senior Enterprise Support Lead tại AWS, hỗ trợ khách hàng ngành năng lượng thiết kế và tối ưu giải pháp đám mây. Với niềm đam mê phát triển phần mềm, anh hướng dẫn khách hàng khai thác các dịch vụ AWS. Ngoài công việc, Sergio là nhạc công chơi nhiều nhạc cụ như guitar, piano, trống, và luyện tập Wing Chun Kung Fu.

**Ravi Kumar** là Senior Technical Account Manager tại AWS Enterprise Support, giúp khách hàng ngành du lịch và khách sạn tối ưu hóa hoạt động trên AWS. Ông có hơn 20 năm kinh nghiệm CNTT và luôn chú trọng hiệu quả vận hành. Ngoài công việc, Ravi thích hội họa, chơi cricket và khám phá các địa điểm mới.

**Ankush Goyal** là Enterprise Support Lead tại AWS Enterprise Support, giúp khách hàng đơn giản hóa hoạt động đám mây. Với hơn 20 năm kinh nghiệm, anh tập trung vào việc tối ưu hóa hạ tầng và mang lại giá trị lâu dài cho doanh nghiệp.
