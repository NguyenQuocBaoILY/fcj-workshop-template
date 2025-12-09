---
title: "Blog 3"  
date: "2023-02-24"  
weight: 1  
chapter: false  
pre: " <b> 3.3. </b> "  
---

# Cách Brisa Robotics Sử Dụng AWS để Cải Thiện Hoạt Động Robotics

bởi Erica Goldberger và Sophie Pagalday vào ngày 24 THÁNG 02, 2023
trong [Amazon Kinesis](https://aws.amazon.com/blogs/robotics/category/analytics/amazon-kinesis/), [Amazon Simple Storage Service (S3)](https://aws.amazon.com/blogs/robotics/category/storage/amazon-simple-storage-services-s3/), [Amazon Timestream](https://aws.amazon.com/blogs/robotics/category/database/amazon-timestream/), [Analytics](https://aws.amazon.com/blogs/robotics/category/analytics/), [AWS IoT Greengrass](https://aws.amazon.com/blogs/robotics/category/internet-of-things/aws-greengrass/), [AWS Lambda](https://aws.amazon.com/blogs/robotics/category/compute/aws-lambda/), [Customer Solutions](https://aws.amazon.com/blogs/robotics/category/post-types/customer-solutions/), [Database](https://aws.amazon.com/blogs/robotics/category/database/), [Internet of Things](https://aws.amazon.com/blogs/robotics/category/internet-of-things/), [Kinesis Data Analytics](https://aws.amazon.com/blogs/robotics/category/analytics/amazon-kinesis/kinesis-data-analytics/), [Robotics](https://aws.amazon.com/blogs/robotics/category/robotics/), [Storage](https://aws.amazon.com/blogs/robotics/category/storage/), [Technical How-to](https://aws.amazon.com/blogs/robotics/category/post-types/technical-how-to/) | [Permalink](https://aws.amazon.com/blogs/robotics/how-brisa-robotics-uses-aws-to-improve-robotics-operations/) | [Share](https://aws.amazon.com/vi/blogs/robotics/how-brisa-robotics-uses-aws-to-improve-robotics-operations/#)  

---

Trong bài viết này, bạn sẽ tìm hiểu cách [Brisa Robotics](https://www.brisa.tech/en/home) tận dụng Amazon Web Services (AWS) để thu thập, lưu trữ và xử lý dữ liệu từ các đội phương tiện hỗn hợp nhằm cải thiện hoạt động của khách hàng.

Brisa chuyển đổi các máy móc không tự động thành các đội xe tự động có khả năng thu thập dữ liệu, giúp khách hàng theo dõi các chỉ số hiệu suất quan trọng và cải thiện vận hành. Sứ mệnh của họ là nâng cao hiệu quả vận hành cho khách hàng bằng cách tận dụng cơ sở hạ tầng hiện có và tái sử dụng các máy cũ thay vì bán phế liệu và mua mới. Brisa cung cấp các bộ kit robot mô-đun độc đáo để nâng cấp thiết bị xử lý vật liệu (MHE) như xe nâng, palletizer và telehandler. Các bộ kit này được lắp bổ sung vào MHE để tích hợp nền tảng thu thập dữ liệu độc quyền của Brisa. Chúng hỗ trợ nhiều trường hợp sử dụng như: theo dõi SKU, kiểm tra (lỗi, vật thể, mã vạch), và di chuyển vật liệu. Khả năng quan sát tốt hơn vào những trường hợp sử dụng này thông qua dữ liệu và chỉ số giúp khách hàng Brisa tối ưu hóa bố cục kho và lập kế hoạch tốt hơn.

## Challenge: Build a Flexible Data Collection Solution

Tập đoàn sản xuất bia lớn nhất thế giới cần khả năng quan sát tốt hơn đối với hoạt động kho và hàng tồn để tăng năng suất và cải thiện an toàn. Vì vậy Brisa bắt tay vào xây dựng một giải pháp để thu thập và truyền dữ liệu nhằm giúp khách hàng đưa ra quyết định tốt hơn.

Brisa cam kết không thay đổi cơ sở hạ tầng của khách hàng nhằm tránh tăng chi phí vận hành và bảo trì cho họ. Họ muốn hỗ trợ khách hàng mà không yêu cầu thay đổi bất kỳ hệ thống hiện có nào. Bên cạnh đó, Brisa cần cung cấp một giải pháp duy nhất có thể hoạt động với các khách hàng có quy trình và yêu cầu khác nhau.

Tùy thuộc vào khách hàng, Brisa phải xem xét nhiều yêu cầu khác nhau. Ví dụ, một số khách hàng muốn bảng điều khiển dữ liệu chỉ khả dụng trong mạng nội bộ của họ, trong khi những khách hàng khác muốn bảng điều khiển trực tuyến. Thêm vào đó, có khách hàng muốn sử dụng một máy tính cục bộ để thu thập dữ liệu trước khi gửi lên đám mây, trong khi những khách hàng khác muốn dữ liệu được gửi thẳng từ robot. Brisa cần một công cụ linh hoạt có thể chạy trên nhiều nền tảng cho các kịch bản khác nhau.

Mục tiêu là phát triển một giải pháp linh hoạt để thu thập và cung cấp dữ liệu và chỉ số cho các khách hàng đa dạng mà không yêu cầu họ phải thay đổi bất kỳ cơ sở hạ tầng nào.

## Solution Overview

Brisa đã phát triển một giải pháp thu thập dữ liệu từ các robot MHE; truyền, xử lý và lưu trữ dữ liệu trên AWS; đồng thời đưa dữ liệu vào các dashboard tùy chỉnh theo thời gian thực cho khách hàng. Toàn bộ quy trình này diễn ra mà không cần thay đổi cơ sở hạ tầng của khách hàng, và workflow vẫn có thể hoạt động dù kết nối internet không ổn định hoặc bị gián đoạn.

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/3-BlogsTranslated/Blog-3/1.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>

1. Các thành phần của [AWS IoT Greengrass V2](https://aws.amazon.com/greengrass/) được triển khai lên các robot, bao gồm cả các thành phần dựng sẵn như [stream manager](https://docs.aws.amazon.com/greengrass/v2/developerguide/stream-manager-component.html).
2. Dữ liệu được thu thập từ ứng dụng client chạy trên server (có thể là robot hoặc một máy khác trong mạng của robot). Ứng dụng này có thể chạy như một custom Greengrass component hoặc chạy độc lập ngoài Greengrass.
3. Thành phần stream manager sẽ stream dữ liệu trực tiếp đến [Amazon Kinesis Data Streams (Amazon KDS)](https://aws.amazon.com/kinesis/data-streams/) và [Amazon Simple Storage Service (Amazon S3)](https://aws.amazon.com/s3/).
4. Một hàm [AWS Lambda](https://aws.amazon.com/lambda/) viết bằng Python sẽ xử lý dữ liệu thô từ Kinesis Data Stream và lưu dữ liệu vào cơ sở dữ liệu [Amazon Timestream](https://aws.amazon.com/timestream/).

Khi dữ liệu đã nằm trong AWS, ứng dụng web của Brisa có thể truy vấn Amazon Timestream để lấy dữ liệu hiển thị trên dashboard. Bạn sẽ tìm hiểu chi tiết hơn về workflow này ở phần dưới.

## Collecting the Data

Brisa thu thập dữ liệu trên robot, bao gồm phát hiện vật thể, vị trí robot, tốc độ robot, chuyển động của càng nâng, và giám sát hệ thống. Họ thực hiện điều này bằng cách sử dụng [Robot Operating System 2 (ROS2)](https://docs.ros.org/en/foxy/index.html), một bộ thư viện và công cụ mã nguồn mở dùng để xây dựng ứng dụng robot. ROS2 giúp Brisa phát triển và triển khai ứng dụng robot nhanh hơn nhờ sử dụng các node và công cụ được cộng đồng xây dựng, như mô phỏng và build tooling. Là thành viên sáng lập của ủy ban chỉ đạo kỹ thuật ROS2, AWS đóng vai trò quan trọng trong cộng đồng, cung cấp nhiều tùy chọn phong phú để vận hành các công cụ ROS2 trên AWS. AWS mang đến cho Brisa nền tảng đám mây có khả năng mở rộng cao nhất cùng các tích hợp sâu nhất với ROS2.

## Streaming the Data

Brisa đăng ký (subscribe) vào topic ROS2 và chuyển tiếp các sự kiện đó vào Kinesis data stream bằng cách sử dụng [AWS IoT Greengrass V2 stream manager](https://docs.aws.amazon.com/greengrass/v2/developerguide/stream-manager-component.html). AWS IoT Greengrass là một edge runtime và dịch vụ đám mây mã nguồn mở dành cho Internet of Things (IoT), giúp bạn xây dựng, triển khai và quản lý các ứng dụng IoT trên thiết bị. Bạn có thể sử dụng AWS IoT Greengrass để xây dựng ứng dụng edge bằng các module phần mềm dựng sẵn hoặc tùy chỉnh, gọi là components, có thể kết nối thiết bị edge của bạn với các dịch vụ AWS hoặc dịch vụ của bên thứ ba. Thành phần [stream manager](https://docs.aws.amazon.com/greengrass/v2/developerguide/stream-manager-component.html) cho phép bạn xử lý các luồng dữ liệu để truyền về AWS Cloud từ các thiết bị Greengrass core.

Brisa chọn Greengrass stream manager vì nó có thể hoạt động offline khi mạng không ổn định, mà không cần phải lo lắng về việc buffer hay gửi dữ liệu lên AWS. Dữ liệu được lưu trữ cục bộ và nén lại cho đến khi có kết nối internet. Thay vì phải tự quản lý workflow này, Brisa chỉ cần gửi dữ liệu vào stream và tập trung vào các quy trình robot của riêng họ. Cách triển khai này linh hoạt cho nhiều nhu cầu khách hàng, vì Greengrass stream manager có thể chạy trực tiếp trên các robot hoặc trên máy tính cục bộ mà robot gửi dữ liệu đến.

Brisa có một ứng dụng client đang chạy và một server để khởi động stream manager. Tùy vào khách hàng, server này có thể nằm trên robot hoặc trên một máy khác trong mạng của robot. Để biết thêm thông tin về việc thiết lập [stream manager](https://docs.aws.amazon.com/greengrass/v2/developerguide/stream-manager-component.html), hãy xem tài liệu stream manager và bài viết [Deploy and Manage ROS Robots with AWS IoT Greengrass V2](https://aws.amazon.com/blogs/robotics/deploy-and-manage-ros-robots-with-aws-iot-greengrass-2-0-and-docker/).

Sau đó, Brisa sử dụng một [ROS2 node](https://docs.ros.org/en/foxy/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.html) để thu thập dữ liệu từ các cảm biến. Họ thực hiện điều này bằng cách tạo một ROS2 package.

Sample commands to create a new ROS2 package:.

### Sample commands to create a new ROS2 package:

```bash
cd ~
mkdir -p ws/src
pip install stream_manager
cd src
ros2 pkg create \
  --package-format 3 \
  --build-type ament_python \
  sm_upload
```

Brisa đưa dữ liệu lên Kinesis data stream và Amazon S3 bucket thông qua Greengrass stream manager. Họ thực hiện điều này bằng cách sử dụng Python SDK của stream manager trong các ROS node của mình. Dưới đây là một ví dụ ROS node tương tự cách Brisa triển khai, dùng để publish dữ liệu từ ROS lên stream manager:


```python
import json
import rclpy
from rclpy.node import Node
from stream_manager import (
      ExportDefinition,
      KinesisConfig,
      MessageStreamDefinition,
      StrategyOnFull,
      StreamManagerClient,
)

STREAM_NAME = "SomeStream"
KINESIS_STREAM_NAME = "MyKinesisStream"

class StreamManagerPublisher(Node):
      def __init__(self):
            super().__init__("aws_iot_core_publisher")
            timer_period = 3  # seconds
            self.client = StreamManagerClient()

            exports = ExportDefinition(
                  kinesis=[
                        KinesisConfig(
                        identifier="KinesisExport" + STREAM_NAME,
                        kinesis_stream_name=KINESIS_STREAM_NAME,
                        )
                  ]
            )

            # Create the Status Stream if it does not exist already
            try:
                  self.client.create_message_stream(
                        MessageStreamDefinition(
                        name=STREAM_NAME,
                        strategy_on_full=StrategyOnFull.OverwriteOldestData,
                        export_definition=exports,
                        )
                  )
            except ConnectionRefusedError as e:
                  self.get_logger().error(f"Could not connect to the stream manager: {str(e)}")
                  raise
            except Exception:
                  pass

            # Create the message stream with the S3 Export definition.
            self.client.create_message_stream(
                  MessageStreamDefinition(
                        name=STREAM_NAME,
                        strategy_on_full=StrategyOnFull.OverwriteOldestData,
                        export_definition=exports,
                  )
            )

            self.timer = self.create_timer(timer_period, self.timer_callback)

      def timer_callback(self):
            self.client.append_message(STREAM_NAME, json.dumps({"robot_id": "C3PO","timestamp": datetime.datetime.utcnow().isoformat(),"x": 1.0, "y": 1.1, "z": 3.0}).encode("utf-8"))
            self.get_logger().info("Successfully appended S3 Task Definition to stream")

def main(args=None):
      rclpy.init(args=args)
      sm_publisher = StreamManagerPublisher()
      rclpy.spin(sm_publisher)
      sm_publisher.destroy_node()
      rclpy.shutdown()

if __name__ == "__main__":
      main()
```

Sau đó, họ tiến hành build ROS2 package:

```bash
colcon build --packages-up-to sm_upload
source install/setup.bash
ros2 run sm_upload sm_upload
```

## Storing the Data

Brisa sử dụng stream manager để truyền một phần dữ liệu, như hình ảnh và video, vào Amazon S3 để lưu trữ đối tượng. Phần dữ liệu còn lại, chẳng hạn dữ liệu telemetry, được truyền vào Amazon Kinesis Data Streams, xử lý bằng AWS Lambda, và sau đó lưu vào Amazon Timestream — một cơ sở dữ liệu được xây dựng chuyên biệt cho dữ liệu chuỗi thời gian (time-series).

Amazon Kinesis Data Streams giúp thu nhận và gom dữ liệu từ ứng dụng và log dịch vụ, đồng thời đưa dữ liệu vào data lake. Xem [Create a data stream](https://docs.aws.amazon.com/streams/latest/dev/tutorial-stock-data-kplkcl-create-stream.html) để tìm hiểu thêm về cách tạo một stream.

Brisa sử dụng một hàm [AWS Lambda](http://aws.amazon.com/lambda) để điều phối các bước extract, process và load (ETL) từ Amazon Kinesis vào Amazon Timestream. Họ chọn Lambda vì đây là dịch vụ serverless, nghĩa là chi phí phụ thuộc vào số lượng request và thời gian chạy của hàm (thời gian thực thi code). Họ đã xem xét các tùy chọn ETL được quản lý khác trong AWS nhưng nhận thấy sử dụng Lambda đơn giản là phù hợp nhất cho yêu cầu ETL hiện tại.

## Querying Newly Stored Data for Their Dashboards

Khi dữ liệu đã nằm trong Timestream, Brisa có thể sử dụng các [time series functions](https://docs.aws.amazon.com/timestream/latest/developerguide/timeseries-specific-constructs.functions.html) để thực hiện những truy vấn đơn giản nhưng hiệu quả nhằm hiển thị các chỉ số dựa trên thời gian. Từ giao diện Amazon Timestream console, Brisa có thể kiểm thử các truy vấn dạng SQL. Để tìm hiểu chi tiết hơn, hãy xem [Timestream Concepts](https://docs.aws.amazon.com/timestream/latest/developerguide/concepts.html) và [Using the console](https://docs.aws.amazon.com/timestream/latest/developerguide/console_timestream.html).

Dưới đây là một ví dụ truy vấn mà Brisa có thể chạy để trích xuất dữ liệu kinh doanh hữu ích. Trong truy vấn này, vị trí (positions) được nhóm theo từng khoảng 5 giây để lấy giá trị trung bình của tọa độ x và y. Điều này giúp tối ưu chi phí truy vấn bằng cách không phải lấy toàn bộ dữ liệu mọi lúc.

```sql
SELECT ROUND(AVG(x), 2) AS avg_x,
	ROUND(AVG(y), 2) AS avg_y,
	BIN(time, 5s) AS binned_timestamp
	FROM database.table
WHERE x IS NOT NULL
	AND y IS NOT NULL
	AND robot_name="C3PO"
GROUP BY BIN(time, 5s)
ORDER BY binned_timestamp ASC
```

Brisa cũng có thể truy vấn dữ liệu đã thu thập bằng nhiều SDK khác nhau như [boto3](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/timestream-query.html#TimestreamQuery.Client.query) cho Python hoặc [AWSJavascriptSDK](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-timestream-query/index.html) cho JavaScript. Danh sách đầy đủ các ngôn ngữ lập trình được hỗ trợ có thể xem tại [Tools to Build on AWS](https://aws.amazon.com/developer/tools/).

Brisa sau đó sử dụng các truy vấn này để lấy dữ liệu liên quan đưa vào dashboard dành cho khách hàng.

Brisa thực hiện truy vấn Timestream từ backend bằng [AWS SDK for pandas (trước đây là AWS Data Wrangler)](https://github.com/aws/aws-sdk-pandas) để lấy và xử lý dữ liệu phục vụ dashboard và API. Phần frontend sẽ hiển thị dữ liệu này lên dashboard.

## Results

Khi dữ liệu đã nằm trong AWS, Brisa có thể dễ dàng truy vấn thông tin đã thu thập, xử lý và cung cấp chúng dưới dạng dashboard trực tiếp và báo cáo KPI cho khách hàng. Thư viện chỉ số và trực quan hóa của Brisa mang tính mô-đun và động, liên tục được cập nhật để đáp ứng tích hợp và trường hợp sử dụng mới theo nhu cầu khách hàng. Khả năng này giúp khách hàng cải thiện mức độ an toàn tổng thể và hiệu quả trong kho của họ.

Trước khi có giải pháp của Brisa, công ty sản xuất bia hầu như xử lý việc theo dõi hoàn toàn thủ công. Nhờ vào dashboard và báo cáo, Brisa hiện có thể mang đến cho họ:

- **Độ chính xác cao hơn**: Các thùng chai được lưu trữ cách mặt đất khoảng 5 mét, rất khó để một người quan sát và nhận diện chính xác. Giải pháp của Brisa thu thập hình ảnh từ camera giúp xác định chính xác các thùng chai.
- **Tần suất dữ liệu cao hơn**: Robot tự động có thể quét dữ liệu với tần suất gấp đôi so với con người làm thủ công.
- **Cải thiện an toàn**: Xe nâng thường di chuyển trong cùng lối đi với người kiểm kho, gây nguy hiểm. Việc giao cho robot đảm nhận nhiệm vụ này giúp loại bỏ nguy cơ tai nạn giữa xe nâng và nhân viên.
- **Thông tin vận hành tốt hơn:** Các heatmap trong dashboard thời gian thực giúp khách hàng của Brisa phát hiện các điểm nghẽn trong hoạt động và tối ưu lịch làm việc. Loại insight này không thể có được chỉ bằng quan sát thủ công.

Dưới đây là một số ảnh chụp màn hình từ dashboard của Brisa:

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/3-BlogsTranslated/Blog-3/2.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>

Heatmap vị trí trong khu vực kiểm thử nội bộ của Brisa. Lý tưởng nhất là không nên có khu vực nào robot phải dừng lại, nhưng khi quan sát heatmap này, bạn có thể thấy một số khu vực nơi robot dừng lại thường xuyên hơn những nơi khác. Điều này có thể xuất phát từ nhiều nguyên nhân: tuyến đường robot không tối ưu, biển báo kém, kế hoạch di chuyển chưa tốt, hoặc các vấn đề nghiêm trọng hơn như có người hoặc robot khác chặn lối. Để giải quyết điều này, Brisa cũng thu thập hình ảnh từ camera để khách hàng có thể xem lại chuyện gì đã xảy ra. Những hình ảnh này giúp công ty sản xuất bia hiểu cách cải thiện bố cục kho hoặc điều chỉnh kế hoạch sao cho robot di chuyển mượt mà, không bị gián đoạn trong quá trình vận hành.

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/3-BlogsTranslated/Blog-3/3.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>

Dashboard với hình ảnh camera và góc nhìn của robot. Dashboard này lấy các tệp này từ Amazon S3, nơi dữ liệu được stream manager lưu trữ.

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/3-BlogsTranslated/Blog-3/4.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>

Dashboard hiển thị hình ảnh từ khu vực vận hành của khách hàng.

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/3-BlogsTranslated/Blog-3/5.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>
Dashboard hiển thị một số chỉ số mà Brisa cung cấp cho khách hàng. Tất cả các chỉ số này đều được truy vấn từ Timestream.

Bên cạnh các chỉ số tiêu chuẩn (như thời gian hoạt động và quãng đường di chuyển), thiết kế mô-đun sử dụng AWS giúp Brisa dễ dàng cung cấp các tích hợp tùy chỉnh cho khách hàng, chẳng hạn như:

- **Số lần phương tiện dừng lại**: Đây là chỉ số được công ty sản xuất bia sử dụng nội bộ. Dashboard cho biết mỗi lần dừng diễn ra trong bao lâu. Khách hàng có thể nhấp vào kết quả để xem các sự kiện liên quan xảy ra vào thời điểm đó. Ví dụ, với một lần dừng, họ có thể thấy một hình ảnh phát hiện có người xuất hiện. Người dùng có thể nhấp vào ảnh để xem vị trí trong kho nơi sự kiện này xảy ra và thời điểm cụ thể. Điều này giúp khách hàng hiểu rõ xu hướng dừng của robot theo thời gian để cải thiện hiệu quả vận hành.

- **Tổng quan tải trọng (Load summary)**: Robot nên di chuyển phần lớn thời gian, ngoại trừ thời gian sạc hoặc một số thời điểm dừng ngắn.  
Brisa tùy chỉnh dashboard dựa trên từng đội robot, loại robot được sử dụng và các KPI quan trọng đối với từng khách hàng.

Với pipeline streaming dữ liệu này, Brisa có thể cung cấp cho khách hàng công cụ để theo dõi hàng hóa và hiểu sâu hơn về các vấn đề vận hành mà không cần thay đổi workflow của họ. Khả năng này giúp các khách hàng, chẳng hạn như công ty sản xuất bia, cải thiện hiệu quả vận hành và an toàn trong kho.

Brisa đang tích hợp với nhiều khách hàng và nhiều loại robot hơn, liên tục bổ sung các chỉ số chuyên sâu và tùy chỉnh vào dashboard của họ.

Brisa có thể giúp bạn kiểm soát hàng hóa thường xuyên và chính xác hơn, xác định và giải quyết các điểm nghẽn trong vận hành (thủ công và/hoặc tự động), và đưa ra quyết định dựa trên dữ liệu thực tế. Để tìm hiểu thêm, hãy truy cập website www.brisa.tech.

---

**TAGS**: [autonomous robots](https://aws.amazon.com/blogs/robotics/tag/autonomous-robots/), [AWS Robotics](https://aws.amazon.com/blogs/robotics/tag/aws-robotics/), [Cloud Robotics](https://aws.amazon.com/blogs/robotics/tag/cloud-robotics/)

---

## About the Authors 

<div style="display: flex; align-items: center; border: 1px solid #e1e4e8; padding: 20px; margin-bottom: 20px; background-color: #fff; text-align: left;">
    <img src="/images/3-BlogsTranslated/Blog-3/author1.jpg" style="width: 100px; height: 100px; object-fit: cover; margin: 0 20px 0 0 !important; flex-shrink: 0; display: block;">
    <div style="flex: 1;">
        <h3 style="margin: 0 0 5px 0; font-size: 18px; color: #232f3e; font-weight: bold; font-family: sans-serif;">Aditya Shahani</h3>
        <p style="margin: 0; color: #555; line-height: 1.5; font-size: 14px; font-family: sans-serif;">
            Aditya Shahani là Startup Solutions Architect tập trung hỗ trợ các startup giai đoạn đầu tăng tốc trong hành trình xây dựng trên AWS. Anh đam mê việc tận dụng các công nghệ mới nhất để tối ưu hóa và đơn giản hóa các bài toán kinh doanh ở quy mô lớn.
        </p>
    </div>
</div>

<div style="display: flex; align-items: center; border: 1px solid #e1e4e8; padding: 20px; margin-bottom: 20px; background-color: #fff; text-align: left;">
    <img src="/images/3-BlogsTranslated/Blog-3/author2.jpg" style="width: 100px; height: 100px; object-fit: cover; margin: 0 20px 0 0 !important; flex-shrink: 0; display: block;">
    <div style="flex: 1;">
        <h3 style="margin: 0 0 5px 0; font-size: 18px; color: #232f3e; font-weight: bold; font-family: sans-serif;">Bonnie McClure</h3>
        <p style="margin: 0; color: #555; line-height: 1.5; font-size: 14px; font-family: sans-serif;">
            Bonnie là một biên tập viên chuyên tạo nội dung dễ tiếp cận và hấp dẫn cho mọi đối tượng và nền tảng. Cô luôn tận tâm mang đến định hướng biên tập toàn diện nhằm đem lại trải nghiệm người dùng liền mạch.
        </p>
    </div>
</div>
