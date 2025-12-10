---
title: "Blog 1"
date: "2025-09-08"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---


# Các mẫu kiến trúc Generative AI Serverless – Phần 1
*bởi Michael Hume và Parnab Basak | vào 04/09/2025 | trong Advanced (300), Generative AI, Serverless, Technical How-to | [Link cố định](https://docs.google.com/document/d/1Rwwsy9gce_iqj3hUYwqMI0sjJP6kWSaN6uVlFOxr2bM/edit?usp=sharing)*

Các tổ chức thuộc mọi quy mô và loại hình đang khai thác các mô hình ngôn ngữ lớn (LLM) và mô hình nền tảng (FM) để xây dựng các ứng dụng generative AI mang lại trải nghiệm mới cho khách hàng và nhân viên. Serverless computing cung cấp giải pháp hoàn hảo, giúp các tổ chức tập trung vào đổi mới, tính linh hoạt và hiệu quả chi phí mà không cần phải quản lý phức tạp cơ sở hạ tầng. Các tổ chức chuyển đổi các triển khai thử nghiệm của họ thành các ứng dụng sẵn sàng cho sản xuất có thể áp dụng các mẫu thiết kế phần mềm đã được chứng minh, có khả năng mở rộng và dễ bảo trì làm nền tảng cho kiến trúc của họ.
Loạt bài gồm hai phần này khám phá các mẫu kiến trúc, thực tiễn tốt nhất, triển khai mã nguồn và các cân nhắc thiết kế cần thiết để tích hợp thành công các giải pháp generative AI vào cả ứng dụng mới và hiện có. Trong bài viết này, chúng tôi tập trung vào các mẫu áp dụng cho việc kiến trúc các ứng dụng generative AI thời gian thực. Phần 2 sẽ đề cập đến các mẫu để xây dựng các triển khai generative AI theo lô sử dụng các dịch vụ serverless.

## Separation of concerns

Một nguyên tắc cơ bản trong việc xây dựng các ứng dụng generative AI mạnh mẽ là separation of concerns, bao gồm việc phân chia stack ứng dụng thành ba thành phần riêng biệt: lớp front-end, lớp middleware và lớp dịch vụ back-end. Cách tiếp cận kiến trúc này (như được hiển thị trong sơ đồ sau) mang lại nhiều lợi ích, bao gồm giảm độ phức tạp, tăng khả năng bảo trì và khả năng mở rộng các thành phần một cách độc lập. Bằng cách triển khai sự tách biệt này, bạn có thể phát triển các giải pháp đa nền tảng trong khi vẫn duy trì tính linh hoạt để phát triển từng thành phần theo các yêu cầu cụ thể.

Mặc dù các tầng này chỉ là sự mở rộng của stack phần mềm truyền thống, chúng thực hiện một số tác vụ cụ thể trong các ứng dụng generative AI.

### Lớp front-end

Lớp front-end đóng vai trò là giao diện chính giữa người dùng cuối và ứng dụng generative AI. Đối với các tổ chức tích hợp generative AI vào các ứng dụng hiện có, lớp này có thể đã được thiết lập. Front-end xử lý các trách nhiệm quan trọng bao gồm xác thực người dùng, trình bày UI/UX và giao tiếp API. AWS cung cấp một bộ dịch vụ serverless mạnh mẽ để hỗ trợ triển khai front-end, bao gồm AWS Amplify cho full-stack development, Amazon CloudFront kết hợp với Amazon Simple Storage Service (Amazon S3) cho content delivery, và các container services như  Amazon Elastic Container Service (Amazon ECS) và Amazon Elastic Kubernetes Service (Amazon EKS) cho host ứng dụng. Các dịch vụ chuyên biệt như Amazon Lex có thể nâng cao user experience thông qua conversational interfaces và intelligent search capabilities để xây dựng interactive chatbots.

### Lớp middleware

Lớp này đại diện cho lớp tích hợp, bao gồm ba lớp nhỏ thiết yếu quản lý các khía cạnh khác nhau của logic ứng dụng và luồng dữ liệu:
* Lớp API – Lớp này cung cấp các dịch vụ back-end thông qua các giao thức khác nhau, bao gồm REST, GraphQL, và WebSockets. Nó xử lý các chức năng thiết yếu như xác thực input, quản lý lưu lượng và hỗ trợ CORS. Lớp API cũng triển khai các cơ chế ủy quyền và kiểm soát truy cập, quản lý phiên bản API và cung cấp khả năng giám sát. Nó đảm bảo giao tiếp an toàn và hiệu quả giữa các thành phần front-end và back-end trong khi duy trì. Các dịch vụ quản lý của AWS như Amazon API Gateway và AWS AppSync có thể giúp tạo ra một AI gateway để đơn giản hóa access và API management.
* Lớp prompt engineering – Lớp này bao gồm logic nghiệp vụ cần thiết để tương tác với LLM. Nó xử lý dynamic prompt generation, model selection, prompt caching, model routing, guardrails và security enforcement. Layer này triển khai token và context window optimization, sensitive information filtering, output content moderation, error handling, retry logic và audit trails. Bằng cách tập trung hoá các chức năng này, bạn có thể duy trì consistent prompt strategies, enforce security và optimize model interactions trên các ứng dụng. Bạn có thể sử dụng Amazon DynamoDB để store prompt templates và configurations, đồng thời sử dụng Amazon Bedrock Guardrails, Amazon Bedrock prompt caching, và Amazon Bedrock Intelligent Prompt, để triển khai responsible AI safeguards, reuse của prompt prefixes và dynamic routing, tương ứng.
* Lớp điều phối – Lớp này quản lý các tương tác phức tạp giữa các thành phần hệ thống khác nhau. Nó phối hợp các API call bên ngoài và các agent call, quản lý các truy vấn cơ sở dữ liệu vector, lưu trữ phiên người dùng và lịch sử hội thoại, đồng thời duy trì ngữ cảnh hội thoại qua nhiều tương tác LLM. Các framework như LangChain và LlamaIndex thường được sử dụng để đơn giản hóa các hoạt động này và cung cấp các cách tiếp cận tiêu chuẩn cho các tác vụ generative AI phổ biến. AWS Step Functions có direct integrations với hơn 220 AWS services, bao gồm Amazon Bedrock, cho phép bạn xây dựng luồng công việc Generative AI workflows mà không cần thêm tài nguyên tính toán. Ngoài ra, với Amazon Bedrock Flows, bạn có thể tạo complex, flexible, multi-prompt workflows để evaluate, compare và version.

### Dịch vụ back-end, agent và nguồn dữ liệu private

Lớp back-end tạo thành lõi của phản hồi generative AI được hỗ trợ bởi LLM. Nó bao gồm lưu trữ và gọi mô hình LLM, agent, knowledge bases hoặc một máy chủ Model Context Protocol (MCP), Amazon Bedrock, Amazon SageMaker JumpStart và  Amazon SageMaker cung cấp nhiều FM hiệu năng cao từ công ty AI hàng đầu hoặc cấu trúc tùy chọn để tự mang mô hình bạn. Bạn có thể chạy một máy chủ MCP sử dụng container, như được mô tả trong Hướng dẫn triển khai máy chủ Model Context Protocol trên AWS.
Nguồn dữ liệu private bổ sung cho LLM bằng cách cung cấp kiến thức độc quyền có thẩm quyền ngoài dữ liệu huấn luyện của nó. Đối với triển khai Retrieval Augmented Generation (RAG), Amazon Kendra, Amazon OpenSearch Serverless, và Amazon Aurora PostgreSQL-Compatible Edition với pgVector cung cấp tùy chọn cơ sở dữ liệu vector mạnh mẽ, có khả năng mở rộng. Để tìm hiểu sâu hơn, vui lòng đọc Vai trò của cơ sở dữ liệu vector trong các ứng dụng generative AI về các lựa chọn dịch vụ AWS có sẵn để lưu trữ embedding trong cơ sở dữ liệu vector chuyên dụng.
Các ứng dụng thời gian thực xử lý và cung cấp phản hồi với độ trễ tối thiểu, nâng cao trải nghiệm người dùng và hỗ trợ ra quyết định nhanh hơn. Trong các phần sau, chúng tôi khám phá một số mẫu kiến trúc có thể được sử dụng để triển khai các ứng dụng generative AI thời gian thực.

## Mẫu 1: Phản hồi yêu cầu đồng bộ (synchronous request response)

Trong mẫu này, các phản hồi được tạo và truyền ngay lập tức, trong khi client chặn/chờ phản hồi. Mặc dù mẫu này dễ triển khai, có luồng dự đoán được và cung cấp tính nhất quán mạnh, nó gặp phải các vấn đề như hoạt động chặn, độ trễ cao và khả năng xảy ra timeout. Khi được triển khai cho các ứng dụng generative AI, mẫu này đặc biệt phù hợp với một số phương thức như tạo video hoặc hình ảnh. Đối với các tương tác LLM nhanh, nó có thể xử lý nhiều yêu cầu đồng thời trong khi duy trì hiệu suất ổn định dưới các tải khác nhau. Mô hình này có thể được triển khai thông qua nhiều cách tiếp cận kiến trúc.

### REST API

Bạn có thể sử dụng RESTful API để giao tiếp với back-end qua các yêu cầu HTTP. Bạn có thể sử dụng API REST hoặc HTTP trong API Gateway hoặc một Application Load Balancer cho định tuyến dựa trên đường dẫn đến middleware. API Gateway cung cấp các tính năng bổ sung như xác thực dựa trên token, custom authorizers, quyền dựa trên tài nguyên, ánh xạ và chuyển đổi yêu cầu/phản hồi, quản lý phiên bản và giới hạn tốc độ. Tuy nhiên, với REST/HTTP APIs trong API Gateway, phản hồi phải được tạo trong vòng 29 giây để đáp ứng timeout tích hợp mặc định. Bạn có thể mở rộng giới hạn mặc định này lên 5 phút cho REST API với khả năng giảm quota giới hạn cấp vùng AWS cho tài khoản của bạn. Để xem ví dụ triển khai, hãy tham khảo Tương tác với mô hình Bedrock từ một hàm Lambda được đặt trước bởi API Gateway.

### GraphQL HTTP API

Bạn có thể sử dụng AWS AppSync làm tầng API để tận dụng các lợi ích của GraphQL API. GraphQL API cung cấp khả năng lấy dữ liệu khai báo và hiệu quả bằng cách sử dụng định nghĩa typed schema, cache dữ liệu serverless, đồng bộ hóa dữ liệu ngoại tuyến, bảo mật và kiểm soát truy cập chi tiết. Nó cũng cung cấp các nguồn dữ liệu và resolver để viết logic nghiệp vụ. Nếu bạn không cần tầng mutation, AWS AppSync có thể gọi trực tiếp một LLM trong Amazon Bedrock. Thời gian tích hợp của AWS AppSync được đặt mặc định là 30 giây và không thể mở rộng. Nếu bạn cần thực hiện các hoạt động có thể mất nhiều thời gian hơn, hãy xem xét triển khai các mẫu không đồng bộ hoặc chia nhỏ hoạt động thành các phần nhỏ hơn. Để xem ví dụ tích hợp, hãy tham khảo Gọi mô hình Amazon Bedrock từ resolver HTTP của AWS AppSync.

### Giao diện chatbot hội thoại

Amazon Lex là một dịch vụ để xây dựng các giao diện hội thoại bằng giọng nói và văn bản, cung cấp khả năng nhận diện giọng nói và hiểu ngôn ngữ. Nó đơn giản hóa việc phát triển đa phương thức và cho phép xuất bản chatbot lên các dịch vụ trò chuyện và thiết bị di động khác nhau. Nó cung cấp tích hợp gốc với Lambda để đơn giản hóa việc phát triển chatbot. Khi một hàm Lambda được sử dụng cho việc hoàn thành, thời gian phản hồi mặc định được đặt là 30 giây. Để vượt qua, bạn có thể sử dụng các cập nhật hoàn thành để cung cấp các cập nhật định kỳ cho người dùng, để người dùng biết rằng chatbot vẫn đang xử lý yêu cầu của họ. Để xem ví dụ triển khai, hãy tham khảo Tăng cường Amazon Connect và Lex với khả năng của generative AI. Sơ đồ sau minh họa kiến trúc giải pháp.

### Sử dụng điều phối để gọi mô hình

AWS Step Functions cho phép điều phối và phối hợp nhiều tác vụ, với tích hợp gốc trên các dịch vụ AWS như Amazon API Gateway, AWS Lambda và Amazon DynamoDB. AWS Step Functions cung cấp các tính năng tích hợp như điều phối hàm, phân nhánh, xử lý lỗi, xử lý song song và khả năng human-in-the-loop. Nó cũng có tích hợp tối ưu với Amazon Bedrock, cho phép gọi trực tiếp các mô hình nền tảng Amazon Bedrock từ các luồng công việc AWS Step Functions. Với tích hợp này, bạn có thể thực hiện các tác vụ sau:
* Làm phong phú thêm việc xử lý dữ liệu Step Functions với khả năng generative AI cho các tác vụ như tóm tắt văn bản, tạo hình ảnh hoặc cá nhân hóa.
* Truy xuất và đưa dữ liệu cập nhật (như giá sản phẩm hoặc hồ sơ người dùng) vào lời nhắc LLM để cải thiện độ chính xác.
* Điều phối các cuộc gọi LLM và tác nhân trong một chuỗi xử lý tùy chỉnh, sử dụng các mô hình phù hợp nhất ở mỗi giai đoạn.
* Triển khai các tương tác human-in-the-loop để kiểm duyệt phản hồi và xử lý hiện tượng ảo giác của FM.

Để xem ví dụ triển khai sử dụng API Gateway, hãy tham khảo Prompt chaining với Amazon API Gateway và AWS Step Functions. Để xem ví dụ triển khai sử dụng AWS AppSync, hãy tham khảo Prompt chaining với AWS AppSync, AWS Step Functions và Amazon Bedrock. Sơ đồ sau minh họa một kiến trúc ví dụ.

## Mẫu 2: Phản hồi yêu cầu bất đồng bộ (asynchronous request response)

Mẫu này cung cấp kênh giao tiếp full-duplex, hai chiều giữa client và server mà không yêu cầu clients phải chờ cập nhật. Lợi ích lớn nhất là tính non-blocking có thể xử lý các hoạt động kéo dài. Tuy nhiên, chúng phức tạp hơn để triển khai vì yêu cầu quản lý kênh, message và trạng thái. Mô hình này có thể được triển khai thông qua hai cách tiếp cận kiến trúc.

### WebSocket API

Giao thức WebSocket cho phép giao tiếp đồng bộ thời gian thực giữa front-end và middleware, cho phép nhắn tin full-duplex, hai chiều qua kết nối TCP liên tục. Hành vi hai chiều này nâng cao tương tác client/dịch vụ, cho phép các dịch vụ đẩy dữ liệu đến client mà không cần yêu cầu rõ ràng. Sử dụng API Gateway, bạn có thể tạo một WebSocket API làm giao diện trạng thái cho một dịch vụ AWS (như Lambda hoặc DynamoDB) hoặc cho một điểm cuối HTTP. WebSocket API gọi back-end của bạn dựa trên nội dung của các thông điệp nó nhận được từ ứng dụng client. Sau khi thông điệp được tạo, back-end có thể gửi các thông điệp gọi lại đến các client được kết nối. Mỗi chu kỳ yêu cầu-phản hồi phải hoàn thành trong vòng 29 giây, như được xác định bởi timeout tích hợp API Gateway cho WebSockets. Thời gian kết nối cho WebSocket APIs của API Gateway có thể lên đến 2 giờ với timeout kết nối idle là 10 phút, những điều này không thể được mở rộng. Để xem ví dụ triển khai, hãy tham khảo AI Chat with Amazon API Gateway (WebSockets), AWS Lambda and Amazon Bedrock. Sơ đồ sau minh họa một kiến trúc ví dụ.

AWS AppSync có thể thiết lập và duy trì các kết nối WebSocket an toàn cho các hoạt động đăng ký GraphQL, cho phép các ứng dụng middleware phân phối dữ liệu thời gian thực từ các nguồn dữ liệu đến các subscriber. Nó cũng hỗ trợ một mô hình publish-subscribe đơn giản, nơi các giao diện client có thể lắng nghe các kênh hoặc chủ đề cụ thể, với AWS AppSync quản lý nhiều kênh pub/sub tạm thời và các kết nối WebSocket để phân phối và lọc dữ liệu dựa trên tên kênh. Để xem ví dụ triển khai, hãy tham khảo AI Chat với AWS AppSync (WebSockets), AWS Lambda và Amazon Bedrock. Sơ đồ sau minh họa một kiến trúc ví dụ.

## Mẫu 3: Phản hồi luồng bất đồng bộ (asynchronous streaming response)
Mẫu luồng này cho phép luồng phản hồi thời gian thực đến client theo từng phần, nâng cao trải nghiệm người dùng và giảm thiểu độ trễ phản hồi đầu tiên. Mẫu này sử dụng các khả năng luồng tích hợp trong các dịch vụ như Amazon Bedrock (các API InvokeModelWithResponseStream hoặc ConverseStream) và suy luận thời gian thực với SageMaker, cho phép các ứng dụng hiển thị kết quả từng phần thay vì chờ phản hồi hoàn chỉnh. Mẫu này đặc biệt hiệu quả cho các ứng dụng triển khai phương thức văn bản như giao diện trò chuyện và công cụ tạo nội dung dựa trên từ.
Việc triển khai được thực hiện thông qua API Gateway WebSocket API hoặc AWS AppSync WebSocket API hoặc đăng ký GraphQL, với sự cân nhắc cẩn thận về quản lý thời gian chờ và xử lý kết nối.

Nếu bạn không cần tầng API, phản hồi luồng Lambda (Lambda response streaming) cho phép một hàm Lambda truyền dần các tải trọng phản hồi trở lại client. Để biết thêm chi tiết, hãy tham khảo Sử dụng Amazon Bedrock với AWS Lambda.

## Kết luận
Bài viết này đã giới thiệu ba mẫu thiết kế áp dụng cho các ứng dụng generative AI thời gian thực: phản hồi yêu cầu đồng bộ, phản hồi yêu cầu bất đồng bộ và streaming response bất đồng bộ. Chúng tôi cũng đã nêu bật cách triển khai các mẫu này bằng các dịch vụ serverless của AWS. Khi lựa chọn mẫu phù hợp cho triển khai của bạn, điều quan trọng là phải xem xét trải nghiệm người dùng cuối mong đợi, technical stack hiện có, giới hạn dịch vụ AWS và độ trễ của phản hồi LLM của bạn. Trong Phần 2, chúng tôi sẽ thảo luận về các mẫu để xây dựng các triển khai batch-oriented generative AI sử dụng các dịch vụ serverless của AWS.
