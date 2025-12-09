---
title: "Blog 2"
date: "2025-07-09"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Overcoming the Challenges of Kafka Connect with Amazon Data Firehose

Author: Swapna Bandla and Austin Groeneveld — July 7, 2025  
Categories: [Amazon Data Firehose](https://aws.amazon.com/vi/data-firehose/), [Amazon Kinesis](https://aws.amazon.com/vi/kinesis/), [Amazon Managed Streaming for Apache Kafka (Amazon MSK)](https://aws.amazon.com/vi/msk/), [Data Analytics](https://aws.amazon.com/vi/analytics/), Intermediate(200)  

---

Apache Kafka is a popular open source distributed streaming platform that is widely used in the AWS ecosystem. It’s designed to handle real-time, high-throughput data streams, making it well-suited for building real-time data pipelines to meet the streaming needs of modern cloud-based applications.

For AWS customers looking to run Apache Kafka, but don’t want to worry about the undifferentiated heavy lifting involved with self-managing their Kafka clusters, [Amazon Managed Streaming for Apache Kafka](https://aws.amazon.com/msk/) (Amazon MSK) offers fully managed Apache Kafka. This means Amazon MSK provisions your servers, configures your Kafka clusters, replaces servers when they fail, orchestrates server patches and upgrades, makes sure clusters are architected for high availability, makes sure data is durably stored and secured, sets up monitoring and alarms, and runs scaling to support load changes. With a managed service, you can spend your time developing and running streaming event applications.

For applications to use data sent to Kafka, you need to write, deploy, and manage application code that consumes data from Kafka.

Kafka Connect is an open-source component of the Kafka project that provides a framework for connecting with external systems such as databases, key-value stores, search indexes, and file systems from your Kafka clusters. On AWS, our customers commonly write and manage connectors using the Kafka Connect framework to move data out of their Kafka clusters into persistent storage, like Amazon Simple Storage Service (Amazon S3), for long-term storage and historical analysis.

At scale, customers need to programmatically manage their Kafka Connect infrastructure for consistent deployments when updates are required, as well as the code for error handling, retries, compression, or data transformation as it is delivered from your Kafka cluster. However, this introduces a need for investment into the software development lifecycle (SDLC) of this management software. Although the SDLC is a cost-effective and time-efficient process to help development teams build high-quality software, for many customers, this process is not desirable for their data delivery use case, particularly when they could dedicate more resources towards innovating for other key business differentiators. Beyond SDLC challenges, many customers face fluctuating data streaming throughput. For instance:

- Online gaming businesses experience throughput variations based on game usage
- Video streaming applications see changes in throughput depending on viewership
- Traditional businesses have throughput fluctuations tied to consumer activity

Striking the right balance between resources and workload can be challenging. Under-provisioning can lead to consumer lag, processing delays, and potential data loss during peak loads, hampering real-time data flows and business operations. On the other hand, over-provisioning results in underutilized resources and unnecessary high costs, making the setup economically inefficient for customers. Even the action of scaling up your infrastructure introduces additional delays because resources need to be provisioned and acquired for your Kafka Connect cluster.

Even when you can estimate aggregated throughput, predicting throughput per individual stream remains difficult. As a result, to achieve smooth operations, you might resort to over-provisioning your Kafka Connect resources (CPU) for your streams. This approach, though functional, might not be the most efficient or cost-effective solution.

Customers have been asking for a fully serverless solution that will not only handle managing resource allocation, but transition the cost model to only pay for the data they are delivering from the Kafka topic, instead of underlying resources that require constant monitoring and management.

In September 2023, we announced a new integration between Amazon MSK and Amazon Data Firehose, allowing builders to deliver data from their MSK topics to their destination sinks with a fully managed, serverless solution. With this new integration, you no longer needed to develop and manage your own code to read, transform, and write your data to your sink using Kafka Connect. Firehose abstracts away the retry logic required when reading data from your MSK cluster and delivering it to the desired sink, as well as infrastructure provisioning, because it can scale out and scale in automatically to adjust to the volume of data to transfer. There are no provisioning or maintenance operations required on your side.

At release, the checkpoint time to start consuming data from the MSK topic was the creation time of the Firehose stream. Firehose couldn’t start reading from other points on the data stream. This caused challenges for several different use cases.

For customers that are setting up a mechanism to sink data from their cluster for the first time, all data in the topic older than the timestamp of Firehose stream creation would need another way to be persisted. For example, customers using Kafka Connect connectors, like These users were limited in using Firehose because they wanted to sink all the data from the topic to their sink, but Firehose couldn’t read data from earlier than the timestamp of Firehose stream creation.

For other customers that were running Kafka Connect and needed to migrate from their Kafka Connect infrastructure to Firehose, this required some extra coordination. The release functionality of Firehose means you can’t point your Firehose stream to a specific point on the source topic, so a migration requires stopping data ingest to the source MSK topic and waiting for Kafka Connect to sink all the data to the destination. Then you can create the Firehose stream and restart the producers such that the Firehose stream can then consume new messages from the topic. This adds additional, and non-trivial, overhead to the migration effort when attempting to cut over from an existing Kafka Connect infrastructure to a new Firehose stream.

To address these challenges, we’re happy to announce a new feature in the Firehose integration with Amazon MSK. You can now specify the Firehose stream to either read from the earliest position on the Kafka topic or from a custom timestamp to begin reading from your MSK topic.

In the first post of this series, we focused on managed data delivery from Kafka to your data lake. In this post, we extend the solution to choose a custom timestamp for your MSK topic to be synced to Amazon S3.

## Overview of Firehose integration with Amazon MSK

Firehose integrates with Amazon MSK to offer a fully managed solution that simplifies the processing and delivery of streaming data from Kafka clusters into data lakes stored on Amazon S3. With just a few clicks, you can continuously load data from your desired Kafka clusters to an S3 bucket in the same account, eliminating the need to develop or run your own connector applications. The following are some of the key benefits to this approach:

- Fully managed service – Firehose is a fully managed service that handles the provisioning, scaling, and operational tasks, allowing you to focus on configuring the data delivery pipeline.
- Simplified configuration – With Firehose, you can set up the data delivery pipeline from Amazon MSK to your sink with just a few clicks on the AWS Management Console.
- Automatic scaling – Firehose automatically scales to match the throughput of your Amazon MSK data, without the need for ongoing administration.
- Data transformation and optimization – Firehose offers features like JSON to Parquet/ORC conversion and batch aggregation to optimize the delivered file size, simplifying data analytical processing workflows.
- Error handling and retries – Firehose automatically retries data delivery in case of failures, with configurable retry durations and backup options.
Offset select option – With Firehose, you can select the starting position for the MSK delivery stream to be delivered within a topic from three options:
  - Firehose stream creation time – This allows you to deliver data starting from Firehose stream creation time. When migrating from to Firehose, if you have an option to pause the producer, you can consider this option.
  - Earliest – This allows you to deliver data starting from MSK topic creation time. You can choose this option if you’re setting a new delivery pipeline with Firehose from Amazon MSK to Amazon S3.
  - At timestamp – This option allows you to provide a specific start date and time in the topic from where you want the Firehose stream to read data. The time is in your local time zone. You can choose this option if you prefer not to stop your producer applications while migrating from Kafka Connect to Firehose. You can refer to the Python script and steps provided later in this post to derive the timestamp for the latest events in your topic that were consumed by Kafka Connect.

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/3-BlogsTranslated/Blog-2/1.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>

The following are benefits of the new timestamp selection feature with Firehose:

- You can select the starting position of the MSK topic, not just from the point that the Firehose stream is created, but from any point from the earliest timestamp of the topic.
- You can replay the MSK stream delivery if required, for example in the case of testing scenarios to select from different timestamps with the option to select from a specific timestamp.
- When migrating from Kafka Connect to Firehose, gaps or duplicates can be managed by selecting the starting timestamp for Firehose delivery from the point where Kafka Connect delivery ended. Because the new custom timestamp feature isn’t monitoring Kafka consumer offsets per partition, the timestamp you select for your Kafka topic should be a few minutes before the timestamp at which you stopped Kafka Connect. The earlier the timestamp you select, the more duplicate records you will have downstream. The closer the timestamp to the time of Kafka Connect stopping, the higher the likelihood of data loss if certain partitions have fallen behind. Be sure to select a timestamp appropriate to your requirements.

## Overview of the Solution

We will examine two data streaming scenarios as follows:

We discuss two scenarios to stream data.

#### In Scenario 1, we migrate to Firehose from Kafka Connect with the following steps:
- Derive the latest timestamp from MSK events that Kafka Connect delivered to Amazon S3.
Create a Firehose delivery stream with Amazon MSK as the source and Amazon S3 as the destination with the topic starting position as Earliest.
- Query Amazon S3 to validate the data loaded.
#### In Scenario 2, we create a new data pipeline from Amazon MSK to Amazon S3 with Firehose:
- Create a Firehose delivery stream with Amazon MSK as the source and Amazon S3 as the destination with the topic starting position as At timestamp.
- Query Amazon S3 to validate the data loaded.
The solution architecture is depicted in the following diagram.

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/3-BlogsTranslated/Blog-2/2.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>

## Prerequisites

You should have the following prerequisites:

- An [AWS account](https://portal.aws.amazon.com/billing/signup/iam?nc2=h_ct&redirect_url=https%3A%2F%2Faws.amazon.com%2Fregistration-confirmation&src=header_signup#/support) and access to the following AWS services:
  - [Amazon Elastic Compute Cloud](https://aws.amazon.com/ec2/) (Amazon EC2)
  - Amazon Data Firehose
  - [AWS Identity and Access Management](https://aws.amazon.com/iam/) (IAM)
  - Amazon MSK
  - Amazon S3
- An MSK provisioned or MSK serverless cluster with topics created and data streaming to it. The sample topic used in this is order.
- An EC2 instance configured to use as a Kafka admin client. Refer to [Create an IAM role](https://docs.aws.amazon.com/msk/latest/developerguide/create-iam-role.html) for instructions to create the client machine and IAM role that you will need to run commands against your MSK cluster.
- An S3 bucket for delivering data from Amazon MSK using Firehose.
- Kafka Connect to deliver data from Amazon MSK to Amazon S3 if you want to migrate from Kafka Connect (Scenario 1).

## Migrate to Firehose from Kafka Connect

To reduce duplicates and minimize data loss, you need to configure your custom timestamp for Firehose to read events as close to the timestamp of the oldest committed offset that Kafka Connect reported. You can follow the steps in this section to visualize how the timestamps of each committed offset will vary by partition across the topic you want to read from. This is for demonstration purposes and doesn’t scale as a solution for workloads with a large number of partitions.

Sample data was generated for demonstration purposes by following the instructions referenced in the following [GitHub repo](https://github.com/aws-samples/clickstream-producer-for-apache-kafka). We set up a sample producer application that generates clickstream events to simulate users browsing and performing actions on an imaginary ecommerce website.

To derive the latest timestamp from MSK events that Kafka Connect delivered to Amazon S3, complete the following steps:

1. From your Kafka client, query Amazon MSK to retrieve the Kafka Connect consumer group ID: 

```bash
./kafka-consumer-groups.sh --bootstrap-server $bs --list --command-config client.properties
```

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/3-BlogsTranslated/Blog-2/3.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>

2. Stop Kafka Connect.
3. Query Amazon MSK for the latest offset and associated timestamp for the consumer group belonging to Kafka Connect.

You can use the `get_latest_offsets.py` Python script from the following [GitHub repo](https://github.com/aws-samples/How-to-Overcome-your-Kafka-Connect-Challenges-with-Amazon-Data-Firehose) as a reference to get the timestamp associated with the latest offsets for your Kafka Connect consumer group. To enable authentication and authorization for a non-Java client with an IAM authenticated MSK cluster, refer to the following [GitHub repo](https://github.com/aws/aws-msk-iam-sasl-signer-python/blob/main/README.rst) for instructions on installing the `aws-msk-iam-sasl-signer-python` package for your client.

```
python3 get_latest_offsets.py --broker-list $bs --topic-name “order” --consumer-group-id “connect-msk-serverless-connector-090224” --aws-region “eu-west-1”
```

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/3-BlogsTranslated/Blog-2/4.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>

Note the earliest timestamp across all the partitions.

## Create a data pipeline from Amazon MSK to Amazon S3 with Firehose

The steps in this section are applicable to both scenarios. Complete the following steps to create your data pipeline:

1. On the Firehose console, choose Firehose streams in the navigation pane.
2. Choose Create Firehose stream.

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/3-BlogsTranslated/Blog-2/7.jpg"
         width="1024"
         style="display:block; margin-left:0;">
</div>

3. For Source, choose Amazon MSK.
4. For Destination, choose Amazon S3.

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/3-BlogsTranslated/Blog-2/8.jpg"
         width="1024"
         style="display:block; margin-left:0;">
</div>

5. For Source settings, browse to the MSK cluster and enter the topic name you created as part of the prerequisites.
6. Configure the Firehose stream starting position based on your scenario: 
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

7. For Firehose stream name, leave the default generated name or enter a name of your preference.
8. For Destination settings, browse to the S3 bucket created as part of the prerequisites to stream data.
  
Within this S3 bucket, by default, a folder structure with YYYY/MM/dd/HH will be automatically created. Data will be delivered to subfolders pertaining to the HH subfolder according to the Firehose to Amazon S3 ingestion timestamp.

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/3-BlogsTranslated/Blog-2/11.jpg"
         width="1024"
         style="display:block; margin-left:0;">
</div>

9. Under Advanced settings, you can choose to create the default IAM role for all the permissions that Firehose needs or choose existing an IAM role that has the policies that Firehose needs.

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/3-BlogsTranslated/Blog-2/12.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>

10. Choose Create Firehose stream.

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/3-BlogsTranslated/Blog-2/13.jpg"
         width="1024"
         style="display:block; margin-left:0;">
</div>

On the Amazon S3 console, you can verify the data streamed to the S3 folder according to your chosen offset settings.

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/3-BlogsTranslated/Blog-2/14.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>

## Clean up
To avoid incurring future charges, delete the resources you created as part of this exercise if you’re not planning to use them further.

## Conclusion
Firehose provides a straightforward way to deliver data from Amazon MSK to Amazon S3, enabling you to save costs and reduce latency to seconds. To try Firehose with Amazon S3, refer to the [Delivery to Amazon S3 using Amazon Data Firehose](https://catalog.workshops.aws/msk-labs/en-US/amazon-data-firehose-integration) lab.

## About the Authors 

<div style="display: flex; align-items: center; border: 1px solid #e1e4e8; padding: 20px; margin-bottom: 20px; background-color: #fff; text-align: left;">
    <img src="/images/3-BlogsTranslated/Blog-2/author1.png" alt = "Aditya Shahani" style="width: 100px; height: 100px; object-fit: cover; margin: 0 20px 0 0 !important; flex-shrink: 0; display: block;">
    <div style="flex: 1;"> <h3 style="margin: 0 0 5px 0; font-size: 18px; color: #232f3e; font-weight: bold; font-family: sans-serif;">Aditya Shahani</h3>
        <p style="margin: 0; color: #555; line-height: 1.5; font-size: 14px; font-family: sans-serif;">
            Aditya Shahani is a Startup Solutions Architect focused on accelerating early stage startups throughout their journey building on AWS. He is passionate about leveraging the latest technologies to streamline business problems at scale.
        </p>
    </div>
</div>

<div style="display: flex; align-items: center; border: 1px solid #e1e4e8; padding: 20px; margin-bottom: 20px; background-color: #fff; text-align: left;">
    <img src="/images/3-BlogsTranslated/Blog-2/author2.png" alt="Bonnie McClure" style="width: 100px; height: 100px; object-fit: cover; margin: 0 20px 0 0 !important; flex-shrink: 0; display: block;">
    <div style="flex: 1;">
        <h3 style="margin: 0 0 5px 0; font-size: 18px; color: #232f3e; font-weight: bold; font-family: sans-serif;">Bonnie McClure</h3>
        <p style="margin: 0; color: #555; line-height: 1.5; font-size: 14px; font-family: sans-serif;">
            Bonnie is an editor specializing in creating accessible, engaging content for all audiences and platforms. She is dedicated to delivering comprehensive editorial guidance to provide a seamless user experience.
        </p>
    </div>
</div>