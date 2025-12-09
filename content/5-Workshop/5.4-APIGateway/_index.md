---
title: "API Gateway"
date: "2025-09-09"
weight: 4
chapter: false
pre: " <b> 5.4 </b> "
---
# Initializing API Gateway

### Create an API Gateway Function

Open **API Gateway**

- Select **Create API**

![lambda](/images/5-Workshop/5.4-APIGateway/api1.png)

Choose API type: **REST API**

Then select: **Build**

![lambda](/images/5-Workshop/5.4-APIGateway/api2.png)

### Configure API Information

Enter API name, for example: **taskhub-backend-api**

Endpoint type: **Regional**

Security policy: **SecurityPolicy_TLS13_1_3_2025_09**

Click **Create API**

![lambda](/images/5-Workshop/5.4-APIGateway/api3.png)

![lambda](/images/5-Workshop/5.4-APIGateway/api4.png)

### Create Resource for API

In the API Gateway menu, select **Resources**

Click **Create resource**

![lambda](/images/5-Workshop/5.4-APIGateway/api5.png)

Enter:

- Resource name: `auth`, `task`, `projects` … depending on the project  
- Resource path: `/auth`, `/task`, …

![lambda](/images/5-Workshop/5.4-APIGateway/api6.png)

Click **Create resource**

### Create Method and Connect to Lambda

![lambda](/images/5-Workshop/5.4-APIGateway/api7.png)

Select a **Resource** → click **Create method**

- Choose **ANY** (or POST, GET depending on your API)
- Integration type: **Lambda Function**
- Tick **Use Lambda Proxy integration**
- Select **Region**
- Enter the name of the Lambda function, e.g., **taskhub-backend_1**

![lambda](/images/5-Workshop/5.4-APIGateway/api8.png)

Click **Save**

![lambda](/images/5-Workshop/5.4-APIGateway/api90.png)

Repeat these steps to create additional API routes.

### Grant API Gateway Permission to Call Lambda

Choose **Deploy API**

![lambda](/images/5-Workshop/5.4-APIGateway/api10.png)

- Create a new stage (if not available): **prod1**
- Click **Deploy**

![lambda](/images/5-Workshop/5.4-APIGateway/api11.png)

Result: You will receive an Invoke URL like:

`https://ne6pw5hqej.execute-api.ap-southeast-1.amazonaws.com/prod1`

![lambda](/images/5-Workshop/5.4-APIGateway/api12.png)