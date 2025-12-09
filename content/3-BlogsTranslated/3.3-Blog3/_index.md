---
title: "Blog 3"  
date: "2023-02-24"  
weight: 1  
chapter: false  
pre: " <b> 3.3. </b> "  
---

# How Brisa Robotics Uses AWS to Improve Robotics Operations

by Erica Goldberger and Sophie Pagalday on 24 FEB 2023  
in [Amazon Kinesis](https://aws.amazon.com/blogs/robotics/category/analytics/amazon-kinesis/), [Amazon Simple Storage Service (S3)](https://aws.amazon.com/blogs/robotics/category/storage/amazon-simple-storage-services-s3/), [Amazon Timestream](https://aws.amazon.com/blogs/robotics/category/database/amazon-timestream/), [Analytics](https://aws.amazon.com/blogs/robotics/category/analytics/), [AWS IoT Greengrass](https://aws.amazon.com/blogs/robotics/category/internet-of-things/aws-greengrass/), [AWS Lambda](https://aws.amazon.com/blogs/robotics/category/compute/aws-lambda/), [Customer Solutions](https://aws.amazon.com/blogs/robotics/category/post-types/customer-solutions/), [Database](https://aws.amazon.com/blogs/robotics/category/database/), [Internet of Things](https://aws.amazon.com/blogs/robotics/category/internet-of-things/), [Kinesis Data Analytics](https://aws.amazon.com/blogs/robotics/category/analytics/amazon-kinesis/kinesis-data-analytics/), [Robotics](https://aws.amazon.com/blogs/robotics/category/robotics/), [Storage](https://aws.amazon.com/blogs/robotics/category/storage/), [Technical How-to](https://aws.amazon.com/blogs/robotics/category/post-types/technical-how-to/) | [Permalink](https://aws.amazon.com/blogs/robotics/how-brisa-robotics-uses-aws-to-improve-robotics-operations/) | [Share](https://aws.amazon.com/vi/blogs/robotics/how-brisa-robotics-uses-aws-to-improve-robotics-operations/#)  

---

In this post, you’ll learn how [Brisa Robotics](https://www.brisa.tech/en/home) leverages Amazon Web Services (AWS) to collect, store, and process data from mixed fleets of vehicles to improve customer operations.

Brisa transforms non-autonomous machines into fleets of autonomous vehicles that collect data to help customers track key performance metrics and improve operations. Their mission is to enhance its customers’ efficiencies by leveraging existing infrastructure and reusing old machines instead of selling scraps and buying new ones. Brisa provides unique modular robotic kits to enhance material handling equipment (MHE) such as forklifts, palletizers, and telehandlers. These robotic kits are retrofitted for the MHE to include Brisa’s proprietary data collection platform. The kits support different use cases: stock-keeping unit (SKU) tracking, inspection (defects, objects, barcodes), and material movement. Getting better visibility into these use cases with data and metrics allows Brisa’s customers to optimize their warehouse layout and plan better.

## Challenge: Build a Flexible Data Collection Solution

The largest brewing company in the world needed more visibility into their warehouse operations and stock to increase productivity and improve safety. So Brisa set out to create a solution to collect and stream data their customer could use to make better decisions.

Brisa is committed to avoiding infrastructure changes for customers so as not to increase customer overhead and maintenance costs. They wanted to help their customer without requiring them to change any of their existing infrastructures. Furthermore, Brisa needed to provide a single solution that would work for their customers with different workflows and requirements.

Depending on the customer, Brisa has different requirements to consider. For example, some customers want their data dashboard only available in their network, whereas others want it online. Additionally, some customers want a local computer that collects the data before sending it to the cloud, whereas others want the data transmitted directly from the robots. Brisa needed a flexible tool to run on different platforms for different scenarios.

The goal was to develop a flexible solution to collect and expose data and metrics for varied customers without customers needing to change any infrastructure.

## Solution Overview

Brisa developed a solution that collects data from the MHE robots; streams, processes, and stores it in AWS; and then pulls it into live custom dashboards for customers. This outcome occurs without changing the customer infrastructure, and the workflow can function with or without a stable internet connection.

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/3-BlogsTranslated/Blog-3/1.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>

1. [AWS IoT Greengrass V2](https://aws.amazon.com/greengrass/) components are deployed to the robots, including pre-built components such as the [stream manager](https://docs.aws.amazon.com/greengrass/v2/developerguide/stream-manager-component.html).
2. Data is collected from the client application running on the server (either the robot or an external machine on the robot network). This application can be run as a custom Greengrass component or outside of Greengrass.
3. The stream manager component streams data directly to [Amazon Kinesis Data Streams (Amazon KDS)](https://aws.amazon.com/kinesis/data-streams/) and [Amazon Simple Storage Service (Amazon S3)](https://aws.amazon.com/s3/).
4. A Python based [AWS Lambda](https://aws.amazon.com/lambda/) function processes the raw data from the Kinesis data stream and stores it in an [Amazon Timestream](https://aws.amazon.com/timestream/) database.

Once data is in AWS, Brisa’s web application can query Amazon Timestream to collect data for their dashboards. You will learn more about this workflow below.

## Collecting the Data

Brisa collects data on the robots, such as object detection, robot position, robot speed, fork movements, and system monitoring. They do this using [Robot Operating System 2 (ROS2)](https://docs.ros.org/en/foxy/index.html), an open-source set of libraries and tools for building robot applications. ROS2 enables Brisa to construct and develop robot applications faster by using community-built nodes and devices such as simulation and build tooling. As a founding technical steering committee member for ROS2, AWS is a vital participant in the community, which results in a wide array of options for running ROS2 tooling in AWS. AWS gives Brisa the most scalable cloud platform with the deepest integrations with ROS2.

## Streaming the Data

Brisa subscribes to the ROS2 topic and forwards those events into the Kinesis data stream using the [AWS IoT Greengrass V2 stream manager](https://docs.aws.amazon.com/greengrass/v2/developerguide/stream-manager-component.html). AWS IoT Greengrass is an open-source Internet of Things (IoT) edge runtime and cloud service that helps you build, deploy and manage IoT applications on your devices. You can use AWS IoT Greengrass to build edge applications using pre-built or custom software modules, called components, that can connect your edge devices to AWS services or third-party services. The [stream manager component](https://docs.aws.amazon.com/greengrass/v2/developerguide/stream-manager-component.html) enables you to process data streams to transfer to the AWS Cloud from Greengrass core devices.

Brisa chose this Greengrass stream manager because it can run offline, under intermittent network conditions, without you having to worry about buffering and publishing data into AWS. The data is stored locally and compressed until an internet connection is active. Instead of managing this workflow, Brisa can send the data to the stream and focus on its unique robotic workflows. This setup is flexible to different customer needs as the Greengrass stream manager runs on the robots themselves or the local computer where the robots send data.

Brisa has a client application running and a server to start the stream manager. Depending on the customer, this server can be on the robot or an external machine on the robot network. For more information on setting up the [stream manager](https://docs.aws.amazon.com/greengrass/v2/developerguide/stream-manager-component.html), see the stream manager documentation and [Deploy and Manage ROS Robots with AWS IoT Greengrass V2](https://aws.amazon.com/blogs/robotics/deploy-and-manage-ros-robots-with-aws-iot-greengrass-2-0-and-docker/).

Brisa then used a [ROS2 node](https://docs.ros.org/en/foxy/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.html) to collect data from the sensors. They did this by creating a ROS2 package.

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

Brisa publishes data to the Kinesis data stream and an Amazon S3 bucket through the Greengrass stream manager. They accomplish this by leveraging the stream manager Python SDK in their ROS nodes. Below is a sample ROS node similar to Brisa’s implementation that publishes data from ROS to the stream manager:

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

Then they built the ROS2 package:

```bash
colcon build --packages-up-to sm_upload
source install/setup.bash
ros2 run sm_upload sm_upload
```

## Storing the Data

Brisa uses the stream manager to stream some of the data, such as images and video, to Amazon S3 for object storage. The rest of the data, such as telemetry data, is streamed to Amazon Kinesis Data Streams, processed with an AWS Lambda function, and then stored in Amazon Timestream, a purpose-built time-series database.

Amazon Kinesis Data Streams helps ingest and collect data from application and service logs and deliver data into data lakes. See the [Create a data stream](https://docs.aws.amazon.com/streams/latest/dev/tutorial-stock-data-kplkcl-create-stream.html) to learn more about creating a stream.

Brisa uses an [AWS Lambda](http://aws.amazon.com/lambda) function to orchestrate the extract, process, and load (ETL) operations from Amazon Kinesis into Amazon Timestream. They chose Lambda because it is serverless, so the cost is based on the number of requests and their duration (the time it takes for your code to run). They explored other options with managed ETL features in AWS but found using a simple Lambda function was the most suitable approach for their current ETL requirements.

## Querying Newly Stored Data for Their Dashboards

Once the data is in Timestream, Brisa can leverage [time series functions](https://docs.aws.amazon.com/timestream/latest/developerguide/timeseries-specific-constructs.functions.html) to make simple yet efficient queries to show the custom time based metrics. From the Amazon Timestream console, Brisa can test SQL-like requests. For a more detailed explanation of these concepts, see [Timestream Concepts](https://docs.aws.amazon.com/timestream/latest/developerguide/concepts.html) and [Using the console](https://docs.aws.amazon.com/timestream/latest/developerguide/console_timestream.html).

Here is an example query Brisa can run to extract interesting business data. In this query, positions are grouped by 5 second frames to get an average of x and y coordinates. This is useful to optimize query costs by not fetching all data points, all the time.

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

Brisa is also able to query the collected data from different SDKs such as [boto3](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/timestream-query.html#TimestreamQuery.Client.query) for Python or [AWSJavascriptSDK](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-timestream-query/index.html) for JavaScript. A full list of available programming languages can be found in [Tools to Build on AWS](https://aws.amazon.com/developer/tools/).

Brisa then uses these queries to pull the relevant data into their customer dashboards.

Brisa makes Timestream queries from their backend using [AWS SDK for panda (previously AWS Data Wrangler)](https://github.com/aws/aws-sdk-pandas) to get and arrange data for the dashboard and API. The frontend then displays this data on the dashboard.

## Results

Once the data is in AWS, Brisa can easily query the collected information, adapt it, and expose it as a live dashboard and KPI reports for customers. Brisa’s library of metrics and visualizations is modular and dynamic and is continuously updated to account for new integrations and use cases depending on customer needs. This capability allows customers to improve overall safety conditions and efficiency in their warehouse.

Before Brisa’s solution, the brewing company was primarily handling their tracking manually. Thanks to the dashboard and report, Brisa can now provide them with:

- **Higher accuracy**: The stored bottles are not easily identifiable by a person since they are 5 meters high. This makes tracking difficult. Brisa’s solution collects camera images for their customer, allowing stored bottles to be identified accurately.
- **Increased data frequency**: The autonomous robot is able to scan twice as often than any person manually can.
- **Improved safety**: Forklifts are often on the same alleys as a human inventory counter. Having a robot handle counting, improves safety by removing the risk of accidents between dangerous forklifts and human counters.
- **Better operational insights:** Heatmaps in the live dashboards allow Brisa’s customers to identify bottlenecks in their operations and improve scheduling. This type of operational insight would not be possible by human observation.

Here are some screenshots taken from Brisa’s dashboard:

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/3-BlogsTranslated/Blog-3/2.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>

Position heatmap in Brisa’s in-house testing area. Ideally, there shouldn’t be any area where the robot stopped, but looking at this heatmap, you can observe some areas where the robot stopped more often than others. There can be multiple reasons for this: non-optimal robot routes, lousy signage, bad planning, or other more severe problems, such as a person or other robot blocking the area. To solve this, Brisa also collects camera images so their customer can look at what happened. The images allow the brewing company to understand how to improve the layout or plan differently, so the robot moves steadily without pausing its operations.

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/3-BlogsTranslated/Blog-3/3.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>

Dashboard with the camera image and robot view. The dashboard grabs these files from Amazon S3 where the data was stored by the stream manager.

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/3-BlogsTranslated/Blog-3/4.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>

Dashboard showing images from the customer site.

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/3-BlogsTranslated/Blog-3/5.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>
Dashboard showing some metrics Brisa shows customers. These metrics are all queried from Timestream.

Apart from standard metrics (such as duration and distance), the modular design using AWS enabled Brisa to easily provide custom integrations for their customer such as:

- **Number of vehicle stops**: Vehicle stops is a metric used internally at the brewing company. The dashboard indicates how long each stop lasts. The customer can click on the dashboard results to see related events that happened at this time. For example, for a vehicle stop, they may see a picture where a person was detected. The user can then click on the image to see where in the warehouse this occurred and at what time. This helps the customer understand stopping patterns over time to improve efficiency in their warehouse.
- **Load summary**: The robot should move most of the time except when charging and for limited stops.
Brisa customizes the dashboard depending on the fleet, the robots running, and the KPIs that matter to each customer.

With this data streaming pipeline, Brisa could provide their customer with tools to track their stock and get deeper insights into operational issues without modifying their workflow. This capability helps customers such as the brewing company improve their warehouse operational efficiency and safety.

Brisa is integrating with more clients and types of robots, adding insightful and customized metrics to its dashboard constantly.

Brisa can help you control your stock more often and precisely, identify and solve bottlenecks in your operations (manual and/or autonomous), and make decisions based on actual data. To learn more, check out the website at www.brisa.tech.

---

**TAGS**: [autonomous robots](https://aws.amazon.com/blogs/robotics/tag/autonomous-robots/), [AWS Robotics](https://aws.amazon.com/blogs/robotics/tag/aws-robotics/), [Cloud Robotics](https://aws.amazon.com/blogs/robotics/tag/cloud-robotics/)

---

## About the Authors 

<div style="display: flex; align-items: center; border: 1px solid #e1e4e8; padding: 20px; margin-bottom: 20px; background-color: #fff; text-align: left;">
    <img src="/images/3-BlogsTranslated/Blog-3/author1.jpg" alt="Aditya Shahani" style="width: 100px; height: 100px; object-fit: cover; margin: 0 20px 0 0 !important; flex-shrink: 0; display: block;">
    <div style="flex: 1;"> <h3 style="margin: 0 0 5px 0; font-size: 18px; color: #232f3e; font-weight: bold; font-family: sans-serif;">Aditya Shahani</h3>
        <p style="margin: 0; color: #555; line-height: 1.5; font-size: 14px; font-family: sans-serif;">
            Aditya Shahani is a Startup Solutions Architect focused on accelerating early stage startups throughout their journey building on AWS. He is passionate about leveraging the latest technologies to streamline business problems at scale.
        </p>
    </div>
</div>

<div style="display: flex; align-items: center; border: 1px solid #e1e4e8; padding: 20px; margin-bottom: 20px; background-color: #fff; text-align: left;">
    <img src="/images/3-BlogsTranslated/Blog-3/author2.jpg" alt="Bonnie McClure" style="width: 100px; height: 100px; object-fit: cover; margin: 0 20px 0 0 !important; flex-shrink: 0; display: block;">
    <div style="flex: 1;">
        <h3 style="margin: 0 0 5px 0; font-size: 18px; color: #232f3e; font-weight: bold; font-family: sans-serif;">Bonnie McClure</h3>
        <p style="margin: 0; color: #555; line-height: 1.5; font-size: 14px; font-family: sans-serif;">
            Bonnie is an editor specializing in creating accessible, engaging content for all audiences and platforms. She is dedicated to delivering comprehensive editorial guidance to provide a seamless user experience.
        </p>
    </div>
</div>