---
title: "Blog 2"
date: "2025-07-07"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# Vượt qua những Thách thức của Kafka Connect với Amazon Data Firehose

Tác giả: Swapna Bandla và Austin Groeneveld — 07 tháng 7, 2025 | trong [Amazon Data Firehose](https://aws.amazon.com/vi/data-firehose/), [Amazon Kinesis](https://aws.amazon.com/vi/kinesis/), [Amazon Managed Streaming for Apache Kafka (Amazon MSK)](https://aws.amazon.com/vi/msk/), [Phân tích dữ liệu](https://aws.amazon.com/vi/analytics/), Trung cấp(200)  

---

Apache Kafka là một nền tảng phân phối luồng (streaming) mã nguồn mở phổ biến, được sử dụng rộng rãi trong hệ sinh thái AWS. Nó được thiết kế để xử lý các luồng dữ liệu thời gian thực với lưu lượng lớn, khiến nó rất phù hợp để xây dựng các đường ống dữ liệu thời gian thực nhằm đáp ứng nhu cầu streaming của các ứng dụng dựa trên đám mây hiện đại.

Đối với những khách hàng AWS muốn chạy Apache Kafka nhưng không muốn lo lắng về những công việc nặng nhọc không tạo ra sự khác biệt liên quan đến việc tự quản lý các cụm Kafka của họ, [Amazon Managed Streaming for Apache Kafka](https://aws.amazon.com/msk/) (Amazon MSK) cung cấp Apache Kafka được quản lý hoàn toàn. Điều này có nghĩa là Amazon MSK sẽ cấp phát máy chủ của bạn, cấu hình các cụm Kafka, thay thế máy chủ khi chúng bị lỗi, điều phối các bản vá và nâng cấp máy chủ, đảm bảo các cụm được kiến trúc để có tính sẵn sàng cao, đảm bảo dữ liệu được lưu trữ bền vững và bảo mật, thiết lập giám sát và cảnh báo, cũng như chạy mở rộng quy mô để hỗ trợ thay đổi tải. Với một dịch vụ được quản lý, bạn có thể dành thời gian của mình để phát triển và chạy các ứng dụng sự kiện streaming.

Để các ứng dụng sử dụng dữ liệu được gửi đến Kafka, bạn cần viết, triển khai và quản lý mã ứng dụng tiêu thụ dữ liệu từ Kafka.

Kafka Connect là một thành phần mã nguồn mở của dự án Kafka, cung cấp một khuôn khổ (framework) để kết nối với các hệ thống bên ngoài như cơ sở dữ liệu, kho lưu trữ key-value, chỉ mục tìm kiếm và hệ thống tệp từ các cụm Kafka của bạn. Trên AWS, khách hàng của chúng tôi thường viết và quản lý các connector (trình kết nối) sử dụng framework Kafka Connect để di chuyển dữ liệu ra khỏi cụm Kafka của họ vào bộ lưu trữ bền vững, như Amazon Simple Storage Service (Amazon S3), để lưu trữ dài hạn và phân tích lịch sử.

Ở quy mô lớn, khách hàng cần quản lý cơ sở hạ tầng Kafka Connect theo lập trình để triển khai nhất quán khi cần cập nhật, cũng như mã xử lý lỗi, thử lại, nén hoặc chuyển đổi dữ liệu khi nó được chuyển từ cụm Kafka của bạn. Tuy nhiên, điều này nảy sinh nhu cầu đầu tư vào vòng đời phát triển phần mềm (SDLC) của phần mềm quản lý này. Mặc dù SDLC là một quy trình hiệu quả về chi phí và thời gian để giúp các nhóm phát triển xây dựng phần mềm chất lượng cao, nhưng đối với nhiều khách hàng, quy trình này không mong muốn cho trường hợp sử dụng phân phối dữ liệu của họ, đặc biệt là khi họ có thể dành nhiều nguồn lực hơn cho việc đổi mới các yếu tố khác biệt kinh doanh chính khác. Ngoài những thách thức về SDLC, nhiều khách hàng phải đối mặt với lưu lượng streaming dữ liệu dao động. Ví dụ:

- Các doanh nghiệp game trực tuyến trải qua sự thay đổi lưu lượng dựa trên việc sử dụng trò chơi
- Các ứng dụng phát trực tuyến video thấy sự thay đổi về lưu lượng tùy thuộc vào lượng người xem
- Các doanh nghiệp truyền thống có biến động lưu lượng gắn liền với hoạt động của người tiêu dùng

Việc đạt được sự cân bằng phù hợp giữa tài nguyên và khối lượng công việc có thể là một thách thức. Cấp phát thiếu (under-provisioning) có thể dẫn đến độ trễ tiêu thụ (consumer lag), chậm trễ xử lý và khả năng mất dữ liệu trong thời gian tải cao điểm, cản trở các luồng dữ liệu thời gian thực và hoạt động kinh doanh. Mặt khác, cấp phát thừa (over-provisioning) dẫn đến tài nguyên không được sử dụng hết và chi phí cao không cần thiết, làm cho việc thiết lập trở nên kém hiệu quả về mặt kinh tế đối với khách hàng. Ngay cả hành động mở rộng quy mô cơ sở hạ tầng của bạn cũng gây ra thêm sự chậm trễ vì tài nguyên cần được cấp phát và thu thập cho cụm Kafka Connect của bạn.

Ngay cả khi bạn có thể ước tính lưu lượng tổng hợp, việc dự đoán lưu lượng trên mỗi luồng riêng lẻ vẫn rất khó khăn. Kết quả là, để đạt được hoạt động trơn tru, bạn có thể phải dùng đến cách cấp phát thừa tài nguyên (CPU) Kafka Connect cho các luồng của mình. Cách tiếp cận này, mặc dù hoạt động được, nhưng có thể không phải là giải pháp hiệu quả nhất hoặc tối ưu chi phí nhất.

Khách hàng đã yêu cầu một giải pháp hoàn toàn serverless (không máy chủ) không chỉ xử lý việc quản lý phân bổ tài nguyên mà còn chuyển đổi mô hình chi phí sang việc chỉ trả tiền cho dữ liệu họ phân phối từ topic Kafka, thay vì các tài nguyên cơ bản đòi hỏi sự giám sát và quản lý liên tục.

Vào tháng 9 năm 2023, chúng tôi đã công bố một tích hợp mới giữa Amazon MSK và Amazon Data Firehose, cho phép các nhà xây dựng phân phối dữ liệu từ các topic MSK của họ đến các đích đến (sink) với một giải pháp serverless được quản lý hoàn toàn. Với tích hợp mới này, bạn không còn cần phải phát triển và quản lý mã của riêng mình để đọc, chuyển đổi và ghi dữ liệu vào đích bằng Kafka Connect nữa. Firehose trừu tượng hóa logic thử lại (retry logic) cần thiết khi đọc dữ liệu từ cụm MSK của bạn và phân phối nó đến đích mong muốn, cũng như việc cấp phát cơ sở hạ tầng, bởi vì nó có thể tự động mở rộng ra (scale out) và thu vào (scale in) để điều chỉnh theo khối lượng dữ liệu cần chuyển. Không yêu cầu hoạt động cấp phát hoặc bảo trì nào từ phía bạn.

Tại thời điểm phát hành, thời gian checkpoint để bắt đầu tiêu thụ dữ liệu từ topic MSK là thời gian tạo luồng Firehose. Firehose không thể bắt đầu đọc từ các điểm khác trên luồng dữ liệu. Điều này đã gây ra thách thức cho một số trường hợp sử dụng khác nhau.

Đối với những khách hàng đang thiết lập cơ chế để đẩy dữ liệu từ cụm của họ lần đầu tiên, tất cả dữ liệu trong topic cũ hơn mốc thời gian tạo luồng Firehose sẽ cần một cách khác để được lưu trữ bền vững. Ví dụ, những khách hàng sử dụng các connector của Kafka Connect, những người dùng này bị hạn chế trong việc sử dụng Firehose vì họ muốn đẩy tất cả dữ liệu từ topic đến đích, nhưng Firehose không thể đọc dữ liệu từ trước mốc thời gian tạo luồng Firehose.

Đối với những khách hàng khác đang chạy Kafka Connect và cần di chuyển từ cơ sở hạ tầng Kafka Connect của họ sang Firehose, điều này đòi hỏi một số sự phối hợp thêm. Chức năng khi phát hành của Firehose có nghĩa là bạn không thể trỏ luồng Firehose của mình đến một điểm cụ thể trên topic nguồn, vì vậy việc di chuyển đòi hỏi phải dừng nạp dữ liệu vào topic MSK nguồn và đợi Kafka Connect đẩy hết tất cả dữ liệu đến đích. Sau đó, bạn có thể tạo luồng Firehose và khởi động lại các producer (nhà sản xuất) sao cho luồng Firehose có thể tiêu thụ các tin nhắn mới từ topic. Điều này thêm chi phí bổ sung, và không hề nhỏ, vào nỗ lực di chuyển khi cố gắng chuyển đổi (cut over) từ cơ sở hạ tầng Kafka Connect hiện có sang một luồng Firehose mới.

Để giải quyết những thách thức này, chúng tôi vui mừng thông báo một tính năng mới trong tích hợp Firehose với Amazon MSK. Giờ đây, bạn có thể chỉ định luồng Firehose đọc từ vị trí sớm nhất trên topic Kafka hoặc từ một mốc thời gian tùy chỉnh để bắt đầu đọc từ topic MSK của bạn.

Trong bài viết đầu tiên của loạt bài này, chúng tôi tập trung vào việc phân phối dữ liệu được quản lý từ Kafka đến hồ dữ liệu (data lake) của bạn. Trong bài viết này, chúng tôi mở rộng giải pháp để chọn một mốc thời gian tùy chỉnh cho topic MSK của bạn để được đồng bộ hóa với Amazon S3.

## Tổng quan về tích hợp Firehose với Amazon MSK

Firehose tích hợp với Amazon MSK để cung cấp một giải pháp được quản lý hoàn toàn giúp đơn giản hóa việc xử lý và phân phối dữ liệu streaming từ các cụm Kafka vào các hồ dữ liệu được lưu trữ trên Amazon S3. Chỉ với vài cú nhấp chuột, bạn có thể liên tục tải dữ liệu từ các cụm Kafka mong muốn của mình vào một bucket S3 trong cùng tài khoản, loại bỏ nhu cầu phát triển hoặc chạy các ứng dụng connector của riêng bạn. Sau đây là một số lợi ích chính của cách tiếp cận này:

- Dịch vụ được quản lý hoàn toàn – Firehose là một dịch vụ được quản lý hoàn toàn, xử lý các tác vụ cấp phát, mở rộng quy mô và vận hành, cho phép bạn tập trung vào việc cấu hình đường ống phân phối dữ liệu.
- Cấu hình đơn giản hóa – Với Firehose, bạn có thể thiết lập đường ống phân phối dữ liệu từ Amazon MSK đến đích của mình chỉ với vài cú nhấp chuột trên AWS Management Console.
- Tự động mở rộng quy mô – Firehose tự động mở rộng quy mô để phù hợp với lưu lượng dữ liệu Amazon MSK của bạn mà không cần quản trị liên tục.
- Chuyển đổi và tối ưu hóa dữ liệu – Firehose cung cấp các tính năng như chuyển đổi JSON sang Parquet/ORC và tổng hợp theo lô (batch aggregation) để tối ưu hóa kích thước tệp được phân phối, đơn giản hóa các quy trình xử lý phân tích dữ liệu.
- Xử lý lỗi và thử lại – Firehose tự động thử lại việc phân phối dữ liệu trong trường hợp thất bại, với thời gian thử lại và các tùy chọn sao lưu có thể cấu hình.
Tùy chọn chọn Offset – Với Firehose, bạn có thể chọn vị trí bắt đầu cho luồng phân phối MSK sẽ được phân phối trong một topic từ ba tùy chọn:
  - Thời gian tạo luồng Firehose – Tùy chọn này cho phép bạn phân phối dữ liệu bắt đầu từ thời gian tạo luồng Firehose. Khi di chuyển sang Firehose, nếu bạn có tùy chọn tạm dừng producer, bạn có thể xem xét tùy chọn này.
  - Sớm nhất (Earliest) – Tùy chọn này cho phép bạn phân phối dữ liệu bắt đầu từ thời gian tạo topic MSK. Bạn có thể chọn tùy chọn này nếu bạn đang thiết lập một đường ống phân phối mới với Firehose từ Amazon MSK đến Amazon S3.
  - Tại mốc thời gian (At timestamp) – Tùy chọn này cho phép bạn cung cấp ngày và giờ bắt đầu cụ thể trong topic từ nơi bạn muốn luồng Firehose đọc dữ liệu. Thời gian được tính theo múi giờ địa phương của bạn. Bạn có thể chọn tùy chọn này nếu bạn không muốn dừng các ứng dụng producer của mình trong khi di chuyển từ Kafka Connect sang Firehose. Bạn có thể tham khảo tập lệnh Python và các bước được cung cấp sau trong bài viết này để lấy mốc thời gian cho các sự kiện mới nhất trong topic của bạn đã được Kafka Connect tiêu thụ.

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/3-BlogsTranslated/Blog-2/1.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>

Sau đây là những lợi ích của tính năng chọn mốc thời gian mới với Firehose:

- Bạn có thể chọn vị trí bắt đầu của topic MSK, không chỉ từ thời điểm luồng Firehose được tạo, mà từ bất kỳ điểm nào từ mốc thời gian sớm nhất của topic.
- Bạn có thể phát lại (replay) việc phân phối luồng MSK nếu cần, ví dụ trong trường hợp các kịch bản kiểm thử để chọn từ các mốc thời gian khác nhau với tùy chọn chọn từ một mốc thời gian cụ thể.
- Khi di chuyển từ Kafka Connect sang Firehose, các khoảng trống hoặc bản ghi trùng lặp có thể được quản lý bằng cách chọn mốc thời gian bắt đầu cho việc phân phối Firehose từ điểm mà Kafka Connect kết thúc việc phân phối. Bởi vì tính năng mốc thời gian tùy chỉnh mới không giám sát offset của Kafka consumer trên mỗi phân vùng (partition), mốc thời gian bạn chọn cho topic Kafka của mình nên trước vài phút so với mốc thời gian bạn đã dừng Kafka Connect. Mốc thời gian bạn chọn càng sớm, bạn sẽ càng có nhiều bản ghi trùng lặp ở hạ nguồn. Mốc thời gian càng gần với thời gian dừng Kafka Connect, khả năng mất dữ liệu càng cao nếu một số phân vùng bị tụt lại phía sau. Hãy chắc chắn chọn một mốc thời gian phù hợp với yêu cầu của bạn.

## Tổng quan về Giải pháp

Chúng tôi thảo luận về hai kịch bản để stream dữ liệu.

#### Trong Kịch bản 1, chúng tôi di chuyển sang Firehose từ Kafka Connect với các bước sau:
- Lấy mốc thời gian mới nhất từ các sự kiện MSK mà Kafka Connect đã phân phối đến Amazon S3.
Create a Firehose delivery stream với Amazon MSK là nguồn và Amazon S3 là đích với vị trí bắt đầu topic là Earliest (Sớm nhất).
- Truy vấn Amazon S3 để xác thực dữ liệu đã tải.

#### Trong Kịch bản 2, chúng tôi tạo một đường ống dữ liệu mới từ Amazon MSK đến Amazon S3 với Firehose:
- Tạo một luồng phân phối Firehose với Amazon MSK là nguồn và Amazon S3 là đích với vị trí bắt đầu topic là At timestamp (Tại mốc thời gian).
- Truy vấn Amazon S3 để xác thực dữ liệu đã tải.
Kiến trúc giải pháp được mô tả trong sơ đồ sau.

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/3-BlogsTranslated/Blog-2/2.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>

## Điều kiện tiên quyết

Bạn nên có các điều kiện tiên quyết sau:

- Một [tài khoản AWS](https://portal.aws.amazon.com/billing/signup/iam?nc2=h_ct&redirect_url=https%3A%2F%2Faws.amazon.com%2Fregistration-confirmation&src=header_signup#/support) và quyền truy cập vào các dịch vụ AWS sau:
  - [Amazon Elastic Compute Cloud](https://aws.amazon.com/ec2/) (Amazon EC2)
  - Amazon Data Firehose
  - [AWS Identity and Access Management](https://aws.amazon.com/iam/) (IAM)
  - Amazon MSK
  - Amazon S3
- Một cụm MSK được cấp phát hoặc MSK serverless với các topic đã được tạo và dữ liệu đang stream đến đó. Topic mẫu được sử dụng trong bài này là order.
- Một instance EC2 được cấu hình để sử dụng làm Kafka admin client. Tham khảo [Tạo vai trò IAM](https://docs.aws.amazon.com/msk/latest/developerguide/create-iam-role.html) để biết hướng dẫn tạo máy khách và vai trò IAM mà bạn sẽ cần để chạy các lệnh đối với cụm MSK của mình.
- Một bucket S3 để phân phối dữ liệu từ Amazon MSK sử dụng Firehose.
- Kafka Connect để phân phối dữ liệu từ Amazon MSK đến Amazon S3 nếu bạn muốn di chuyển từ Kafka Connect (Kịch bản 1).

## Migrate to Firehose from Kafka Connect

Để giảm trùng lặp và giảm thiểu mất dữ liệu, bạn cần cấu hình timestamp tùy chỉnh để Firehose đọc dữ liệu gần với timestamp của offset được Kafka Connect commit sớm nhất. Bạn có thể làm theo các bước trong phần này để hình dung cách timestamp của mỗi offset được commit sẽ thay đổi tùy theo từng partition của topic bạn muốn đọc. Đây chỉ là ví dụ minh họa và không thể mở rộng cho workload có số lượng partition lớn.

Dữ liệu mẫu được tạo theo hướng dẫn trong [GitHub repo này](https://github.com/aws-samples/clickstream-producer-for-apache-kafka). Chúng tôi thiết lập một ứng dụng producer mẫu để tạo các sự kiện clickstream mô phỏng người dùng duyệt web và thực hiện thao tác trên một trang thương mại điện tử giả lập.

Để lấy timestamp mới nhất từ các sự kiện MSK mà Kafka Connect gửi vào Amazon S3, hãy thực hiện các bước sau:

1. Từ Kafka client của bạn, truy vấn Amazon MSK để lấy Kafka Connect consumer group ID:

```bash
./kafka-consumer-groups.sh --bootstrap-server $bs --list --command-config client.properties
```

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/3-BlogsTranslated/Blog-2/3.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>

2. Dừng Kafka Connect.

3. Truy vấn Amazon MSK để lấy offset mới nhất và timestamp tương ứng của consumer group thuộc Kafka Connect.

Bạn có thể sử dụng script Python `get_latest_offsets.py` trong [GitHub repo](https://github.com/aws-samples/How-to-Overcome-your-Kafka-Connect-Challenges-with-Amazon-Data-Firehose) để tham khảo cách lấy timestamp tương ứng với các offset mới nhất của Kafka Connect consumer group.  
Để bật xác thực và phân quyền cho client không sử dụng Java trong MSK được cấu hình IAM, hãy tham khảo hướng dẫn cài đặt package `aws-msk-iam-sasl-signer-python` trong [repo sau](https://github.com/aws/aws-msk-iam-sasl-signer-python/blob/main/README.rst) để xem hướng dẫn cài đặt package `aws-msk-iam-sasl-signer-python` cho client của bạn.

```
python3 get_latest_offsets.py --broker-list $bs --topic-name “order” --consumer-group-id “connect-msk-serverless-connector-090224” --aws-region “eu-west-1”
```

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/3-BlogsTranslated/Blog-2/4.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>

Hãy ghi lại timestamp sớm nhất trong tất cả các partition.

## Create a data pipeline from Amazon MSK to Amazon S3 with Firehose

Các bước trong phần này áp dụng cho cả hai kịch bản. Hãy thực hiện các bước sau để tạo data pipeline của bạn:

1. Trên giao diện Firehose console, chọn Firehose streams trong thanh điều hướng.
2. Chọn Create Firehose stream.

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/3-BlogsTranslated/Blog-2/7.jpg"
         width="1024"
         style="display:block; margin-left:0;">
</div>

3. Ở mục Source, chọn Amazon MSK.
4. Ở mục Destination, chọn Amazon S3

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/3-BlogsTranslated/Blog-2/8.jpg"
         width="1024"
         style="display:block; margin-left:0;">
</div>

5. Trong phần Source settings, duyệt đến MSK cluster và nhập tên topic mà bạn đã tạo trong bước chuẩn bị.
6. Cấu hình vị trí bắt đầu (starting position) của Firehose stream dựa trên kịch bản của bạn:

<ol type="a">
  <li>For Scenario 1, set Topic starting position as At Timestamp and enter the timestamp you noted in the previous section.</li>
</ol>

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/3-BlogsTranslated/Blog-2/9.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>

<ol type="a">
  <li>For Scenario 2, set Topic starting position as Earliest.</li>
</ol>
<div style="text-align: left; margin-top: 10px;">
    <img src="/images/3-BlogsTranslated/Blog-2/10.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>

7. Đối với Firehose stream name, giữ nguyên tên mặc định được tạo hoặc nhập tên bạn muốn.
8. Trong phần Destination settings, duyệt đến S3 bucket đã được tạo trong bước chuẩn bị để stream dữ liệu.
  
Bên trong S3 bucket này, theo mặc định, một cấu trúc thư mục theo dạng YYYY/MM/dd/HH sẽ được tự động tạo ra. Dữ liệu sẽ được gửi vào các thư mục con tương ứng với thư mục HH dựa trên timestamp mà Firehose ghi nhận khi đưa dữ liệu vào Amazon S3.


<div style="text-align: left; margin-top: 10px;">
    <img src="/images/3-BlogsTranslated/Blog-2/11.jpg"
         width="1024"
         style="display:block; margin-left:0;">
</div>

9. Trong phần Advanced settings, bạn có thể chọn tạo IAM role mặc định với đầy đủ quyền mà Firehose cần, hoặc chọn một IAM role hiện có đã được gán các policy phù hợp cho Firehose.

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/3-BlogsTranslated/Blog-2/12.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>

10. Chọn Create Firehose stream.

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/3-BlogsTranslated/Blog-2/13.jpg"
         width="1024"
         style="display:block; margin-left:0;">
</div>

Trên giao diện Amazon S3 console, bạn có thể kiểm tra dữ liệu đã được stream vào thư mục S3 theo đúng cấu hình offset mà bạn đã chọn.

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/3-BlogsTranslated/Blog-2/14.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>

## Clean up
Để tránh phát sinh chi phí trong tương lai, hãy xóa các tài nguyên bạn đã tạo trong bài thực hành này nếu bạn không có kế hoạch sử dụng chúng thêm.

## Conclusion
Firehose cung cấp một cách đơn giản để truyền dữ liệu từ Amazon MSK đến Amazon S3, giúp bạn tiết kiệm chi phí và giảm độ trễ xuống chỉ còn vài giây.  
Để trải nghiệm Firehose với Amazon S3, hãy tham khảo bài lab [Delivery to Amazon S3 using Amazon Data Firehose](https://catalog.workshops.aws/msk-labs/en-US/amazon-data-firehose-integration).

## About the Authors 

<div style="display: flex; align-items: center; border: 1px solid #e1e4e8; padding: 20px; margin-bottom: 20px; background-color: #fff; text-align: left;">
    <img src="/images/3-BlogsTranslated/Blog-2/author1.png" alt = "Aditya Shahani" style="width: 100px; height: 100px; object-fit: cover; margin: 0 20px 0 0 !important; flex-shrink: 0; display: block;">
    <div style="flex: 1;">
        <h3 style="margin: 0 0 5px 0; font-size: 18px; color: #232f3e; font-weight: bold; font-family: sans-serif;">Aditya Shahani</h3>
        <p style="margin: 0; color: #555; line-height: 1.5; font-size: 14px; font-family: sans-serif;">
            Aditya Shahani là một Startup Solutions Architect tập trung vào việc hỗ trợ và tăng tốc các startup giai đoạn đầu trong hành trình xây dựng trên AWS. Anh đam mê việc tận dụng các công nghệ mới nhất để tối ưu hóa và giải quyết các vấn đề kinh doanh ở quy mô lớn.
        </p>
    </div>
</div>

<div style="display: flex; align-items: center; border: 1px solid #e1e4e8; padding: 20px; margin-bottom: 20px; background-color: #fff; text-align: left;">
    <img src="/images/3-BlogsTranslated/Blog-2/author2.png" alt="Bonnie McClure" style="width: 100px; height: 100px; object-fit: cover; margin: 0 20px 0 0 !important; flex-shrink: 0; display: block;">
    <div style="flex: 1;">
        <h3 style="margin: 0 0 5px 0; font-size: 18px; color: #232f3e; font-weight: bold; font-family: sans-serif;">Bonnie McClure</h3>
        <p style="margin: 0; color: #555; line-height: 1.5; font-size: 14px; font-family: sans-serif;">
            Bonnie là một biên tập viên chuyên tạo ra nội dung dễ tiếp cận và hấp dẫn cho mọi đối tượng và nền tảng. Cô luôn tận tâm cung cấp định hướng biên tập toàn diện nhằm mang lại trải nghiệm người dùng liền mạch và nhất quán.
        </p>
    </div>
</div>

