---
title: "Configure AWS Secrets Manager"
date: "2025-11-14"
weight: 17
chapter: false
pre: " <b> 5.17. </b> "
---

## Objective

Use **AWS Secrets Manager** to store configuration/secrets for the **TaskHub** system with the following requirements:
- Secrets are stored in **JSON format (Key/value pairs – Plaintext)**
- Data is **encrypted using KMS CMK: `taskhub_kms`**
- (Optional) Enable **automatic secret rotation** using **AWS Lambda**

---

## Step 1 – Access AWS Secrets Manager

1. In the **AWS Console**, type `Secrets Manager` in the search box.
2. Select **Secrets Manager** from the **Services** list.

![1](/images/5-Workshop/5.17-SecretManager/17.1.png)

---

## Step 2 – Create a New Secret (Key/value pairs – JSON)

1. On the Secrets Manager main page, click **Store a new secret**.

![1](/images/5-Workshop/5.17-SecretManager/17.2.png)

2. In **Secret type**, select: **Other type of secret**.
3. In the **Key/value pairs** section:
   - Switch from the **Key/value** tab to the **Plaintext** tab.
   - Paste the following JSON content (demo for DynamoDB + KMS):

   ~~~json
   {
     "Service": "Amazon DynamoDB",
     "Table": "TaskHub Tables",
     "Encryption": "SSE-KMS",
     "KMSKeyAlias": "taskhub_kms",
     "Purpose": "Store users, projects, tasks",
     "DataProtection": "Encrypted at rest"
   }
   ~~~

4. In the **Encryption key** field, select the KMS key:
   - **`taskhub_kms`**
5. Click **Next**.

![1](/images/5-Workshop/5.17-SecretManager/17.3.png)

---

## Step 3 – Configure Secret Name and Basic Information

1. At the **Configure secret** step:

   - **Secret name**:  
     Example:  
     `prod/taskhub/secretmanager`

   - **Description (optional)**:  
     `Metadata for TaskHub DynamoDB encryption demo`

2. **Tags (optional)**: Skip for the workshop.
3. **Resource permissions (optional)**: Keep default (IAM-based access control).
4. **Replicate secret (optional)**: Do not enable for this workshop.
5. Click **Next**.

![1](/images/5-Workshop/5.17-SecretManager/17.4.png)

---

## Step 4 – Configure Automatic Rotation (Optional)

> In a production environment, secrets are usually rotated every 30 days.  
> In this workshop, a shorter interval is configured for demonstration purposes.

1. At **Configure rotation – optional**, enable:
   - **Automatic rotation**
2. In **Rotation schedule**:
   - Select **Schedule expression builder**
   - **Time unit**: `Hours`
   - **Hours**: `23`
   - (Optional) **Window duration**: `4h`
   - Keep **Rotate immediately when the secret is stored** checked
3. In **Rotation function**:
   - Select the Lambda function: **`taskhub-backend`**
4. Click **Next**.

![1](/images/5-Workshop/5.17-SecretManager/17.5.png)

---

## Step 5 – Review & Store the Secret

1. On the **Review** step, verify the following information:

   - Secret type: **Other type of secret**
   - Encryption key: **`taskhub_kms`**
   - Secret name: `prod/taskhub/metadata`
   - Automatic rotation: **Enabled**
   - Lambda rotation function: **`taskhub-backend`**

2. Scroll down to the **Sample code** section:
   - AWS provides built-in sample `getSecret()` functions for:
     - Java
     - JavaScript
     - Python
     - C#
     - Go
   - The TaskHub backend will use the corresponding SDK to retrieve secrets from AWS Secrets Manager instead of hard-coding them in source code.

3. Click **Store** to complete the process.

![1](/images/5-Workshop/5.17-SecretManager/17.6.png)

---

## Result

- A new secret has been successfully created in **AWS Secrets Manager**.
- The secret content is stored in **JSON format**.
- The secret is:
  -  **Encrypted at rest using KMS (`taskhub_kms`)**
  -  **Automatically rotatable using AWS Lambda**
- The **TaskHub backend** can retrieve secrets via:
  - **AWS SDK**
  - **IAM Role / Policy**
