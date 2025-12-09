---
title: "Set up KMS"
date: "2025-11-14"
weight: 9
chapter: false
pre: " <b> 5.9. </b> "
---

## Objective

Create a **Customer Managed Key (CMK)** on AWS KMS to:
- Encrypt data in **DynamoDB**
- Encrypt **Secrets Manager**
- Ensure **DevSecOps standard – data is encrypted using KMS**

---

## Step 1 – Access AWS Key Management Service (KMS)

1. Log in to **AWS Console**  
2. Search for service: **KMS**  
3. Select **Key Management Service**

![Search KMS Service](/images/5-Workshop/5.9-KeyManagementService/9.1.png)

4. Go to menu **Customer managed keys**  
5. Click **Create a key**

![Create Customer managed key](/images/5-Workshop/5.9-KeyManagementService/9.2.png)

---

## Step 2 – Configure Key

1. At **Key type**, select **Symmetric**  
2. At **Key usage**, select **Encrypt and decrypt**

![Configure Key type and Key usage](/images/5-Workshop/5.9-KeyManagementService/9.3.png)

3. Keep other options as default  
4. Click **Next**

---

## Step 3 – Add Labels (Alias & Description)

### Alias

Enter:

~~~text
taskhub_kms
~~~

![Enter alias taskhub_kms](/images/5-Workshop/5.9-KeyManagementService/9.4.png)

Click **Next**

---

## Step 4 – Define Key Administrative Permissions

1. In the **Key administrators** list, select **QuocBao**

![Select Key administrators](/images/5-Workshop/5.9-KeyManagementService/9.5.png)

2. Keep the option **Allow key administrators to delete this key** enabled  
3. Click **Next**

The selected user at this step has full administrative permissions for the KMS Key:
- Edit key policy  
- Enable / disable the key  
- Delete the key  

---

## Step 5 – Define Key Usage Permissions

1. In the **Key users** list, select **QuocBao**

![Select Key users](/images/5-Workshop/5.9-KeyManagementService/9.6.png)

2. No need to add **Other AWS accounts**  
3. Click **Next**

---

## Step 6 – Edit Key Policy

1. At the **Edit key policy** step, click **Edit**  
2. Verify that the policy includes the following permission groups:
   - Root account: `kms:*`  
   - User **QuocBao** is allowed to manage and use the key  

~~~json
{
  "Version": "2012-10-17",
  "Id": "key-consolepolicy-3",
  "Statement": [
    {
      "Sid": "Enable IAM User Permissions",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::166557634525:root"
      },
      "Action": "kms:*",
      "Resource": "*"
    },
    {
      "Sid": "Allow access for Key Administrators",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::166557634525:user/QuocBao"
      },
      "Action": [
        "kms:Create*",
        "kms:Describe*",
        "kms:Enable*",
        "kms:List*",
        "kms:Put*",
        "kms:Update*",
        "kms:Revoke*",
        "kms:Disable*",
        "kms:Get*",
        "kms:Delete*",
        "kms:TagResource",
        "kms:UntagResource",
        "kms:ScheduleKeyDeletion",
        "kms:CancelKeyDeletion",
        "kms:RotateKeyOnDemand"
      ],
      "Resource": "*"
    },
    {
      "Sid": "Allow use of the key",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::166557634525:user/QuocBao"
      },
      "Action": [
        "kms:Encrypt",
        "kms:Decrypt",
        "kms:ReEncrypt*",
        "kms:GenerateDataKey*",
        "kms:DescribeKey"
      ],
      "Resource": "*"
    },
    {
      "Sid": "Allow attachment of persistent resources",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::166557634525:user/QuocBao"
      },
      "Action": [
        "kms:CreateGrant",
        "kms:ListGrants",
        "kms:RevokeGrant"
      ],
      "Resource": "*",
      "Condition": {
        "Bool": {
          "kms:GrantIsForAWSResource": "true"
        }
      }
    }
  ]
}
~~~

3. Modify the policy if necessary to match your actual account ARN  

![Edit key policy screen](/images/5-Workshop/5.9-KeyManagementService/9.7.png)

4. Click **Next**

AWS will automatically attach a valid policy to the key.

---

## Step 7 – Review & Finish

1. Review all configurations:

| Item       | Value                |
|------------|----------------------|
| Key type   | Symmetric            |
| Key usage  | Encrypt and decrypt |
| Alias      | taskhub_kms          |
| Key Admin  | QuocBao             |
| Key User   | QuocBao             |

2. Click **Finish**

AWS will start creating the KMS Key.

---

## Step 8 – Verify KMS Key Created Successfully

![Customer managed keys list](/images/5-Workshop/5.9-KeyManagementService/9.8.png)

1. After creation, go back to **KMS → Customer managed keys**  
2. Verify the following information:
   - Alias: `taskhub_kms`  
   - Status: `Enabled`  
   - Key type: `Symmetric`  
   - Key spec: `SYMMETRIC_DEFAULT`  
   - Key usage: `Encrypt and decrypt`  

![KMS key details](/images/5-Workshop/5.9-KeyManagementService/9.9.png)

The KMS Key has now been successfully created.
