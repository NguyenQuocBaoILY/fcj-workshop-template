---
title: "Set up CloudFront"
date: "2025-09-09"
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

# COMPLETE CLOUDFRONT DISTRIBUTION CONFIGURATION

This Worklog outlines the steps for configuring a CloudFront Distribution, focusing on setting up **Origin Access Control (OAC)** security and verifying the prerequisites on the S3 Bucket.

## 1. S3 PREREQUISITES CHECK

Before finalizing CloudFront setup, you must ensure the S3 Bucket **`taskhub-frontend-prod`** is protected and configured correctly.

### 1.1. Static Website Hosting Status
* **Check:** The **Properties** tab of the S3 Bucket.
* **Status:** **S3 static website hosting** must be in the **Disabled** state.
![anh1](/images/5-workshop/5.7-CloudFront/image1.png)
* **Explanation:** Since CloudFront acts as the CDN, **Static Website Hosting is not required** on S3. S3 merely serves as the content repository (Origin).

### 1.2. Block Public Access & Bucket Policy
* **Check:** The **Permissions** tab of the S3 Bucket.
* **Block public access (bucket settings):** Must be in the **On** state (`Block all public access`).
![anh2](/images/5-workshop/5.7-CloudFront/image2.png)
* **Bucket policy (Final Confirmation):** Must contain the **OAC Policy** that is **automatically updated by CloudFront**.

---

## 2. CLOUDFRONT DISTRIBUTION CONFIGURATION

### 2.1. Step 1 & 2: Get started

![anh4](/images/5-workshop/5.7-CloudFront/image4.png)
![anh5](/images/5-workshop/5.7-CloudFront/image5.png)
![anh6](/images/5-workshop/5.7-CloudFront/image6.png)

| Configuration | Value | Explanation |
| :--- | :--- | :--- |
| **Action** | Click **"Create distribution"**. | Start the CDN creation process. |
| **Plan** | Select **Free Plan** ($0/month). | The free plan for the project. |
| **Distribution name** | `taskhub-frontend-cdn` | A memorable name for the resource. |
| **Distribution type** | **Single website or app**. | The appropriate type for a Frontend application. |

### 2.2. Step 3: Specify origin (OAC Setup)

#### 1. Specify Origin
* **Origin type:** Select **Amazon S3**.
![anh7](/images/5-workshop/5.7-CloudFront/image7.png)
* **S3 origin:** Select the S3 Bucket **`taskhub-frontend-prod`**.
![anh8](/images/5-workshop/5.7-CloudFront/image8.png)

#### 2. Configure OAC (Automatic Security)
1.  Tick **"Allow private S3 bucket access to CloudFront - Recommended"**.
2.  Select **Use recommended origin settings**.
    * **Explanation:** Upon completing the Distribution creation, CloudFront will **automatically update the S3 Bucket Policy** to grant access via OAC.

#### 3. Cache Configuration
* **Cache settings:** Select **Use recommended cache settings tailored to serving S3 content**.
* **Default Root Object:** Enter **`index.html`** (Standard configuration for SPA).

### 2.3. Step 4 & 5: Security & TLS

![anh9](/images/5-workshop/5.7-CloudFront/image9.png)

| Configuration | Applied Value | Explanation |
| :--- | :--- | :--- |
| **WAF** | Uncheck paid features. | Keeping defaults for the project. |
| **Viewer protocol policy** | **Redirect HTTP to HTTPS** | Mandates the use of the secure protocol. |
| **Custom SSL certificate** | **Default CloudFront Certificate** | Activates free HTTPS. |

### 2.4. Step 6: Review and create

#### 1. Review Origin (Source Confirmation)
* **S3 origin:** **taskhub-frontend-prod**. (Must ensure the correct Frontend Bucket is selected).
* **Grant CloudFront access to origin:** Must be **Yes**. (Confirms OAC functionality).

#### 2. Final Configuration
* **Price Class:** Select **Use all edge locations (best performance)**.
* Click the **"Create distribution"** button to deploy.

![anh10](/images/5-workshop/5.7-CloudFront/image10.png)

---

## 3. POST-DEPLOYMENT CHECK: OAC SECURITY CONFIRMATION

After the Distribution status changes to **`Deploying`**, perform this crucial check to confirm that OAC is functional:

1.  **Access S3 Bucket** `taskhub-frontend-prod`.
2.  Navigate to the **"Permissions"** tab.
3.  Find the **"Bucket policy"** section.
4.  **Confirm:** The Policy must be automatically updated and contain the JSON statement authorizing the CloudFront service. This confirms that OAC has blocked direct access and only allows CloudFront to read the files.

![anh3](/images/5-workshop/5.7-CloudFront/image3.png)