---
title: "Set up Web ACL"
date: "2025-11-14"
weight: 18
chapter: false
pre: " <b> 5.18. </b> "
---

## Step 1 – Access AWS WAF & Shield

1. Sign in to the **AWS Console**  
2. Search for the service: **WAF & Shield**  
<div style="text-align: left; margin-top: 10px;">
    <img src="/images/5-Workshop/5.18-WAF/1.1.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>  

3. Click **AWS WAF**  
4. From the left menu, select:  
   **Protection packs (web ACLs)**  
   <div style="text-align: left; margin-top: 10px;">
    <img src="/images/5-Workshop/5.18-WAF/1.2.png"
         width="400"
         style="display:block; margin-left:0;">
    </div>  

5. Click the button:  
   **Create protection pack (web ACL)**

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/5-Workshop/5.18-WAF/1.3.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>  

---

## Step 2 – Select Application Type (Tell us about your app)

### 2.1 Select App Category  
Choose:  
**API & integration services**

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/5-Workshop/5.18-WAF/1.4.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>  

---

## Step 3 – Select the CloudFront Distribution to Protect

1. Expand **Select resources to protect**  
2. Click **Add resources**  
<div style="text-align: left; margin-top: 10px;">
    <img src="/images/5-Workshop/5.18-WAF/1.5.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>  

3. Select:  
   **Global → Add CloudFront or Amplify resources**  
4. Check the CloudFront distribution of TaskHub (S3 frontend)  
<div style="text-align: left; margin-top: 10px;">
    <img src="/images/5-Workshop/5.18-WAF/1.6.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>

5. Click **Add**

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/5-Workshop/5.18-WAF/1.7.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>  

---

## Step 4 – Select the Rule Pack Type

1. Select:

✅ **Build your own pack from all of the protections AWS WAF offers**  

2. In the right panel, select:

✅ **AWS-managed rule group**

3. Click **Next**

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/5-Workshop/5.18-WAF/1.8.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>  

---

## Step 5 – Add Amazon IP Reputation List

1. Select the rule:

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/5-Workshop/5.18-WAF/1.9.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>  

2. In **Rule overrides**, configure as follows:

| Rule | Action |
|------|--------|
| AWSManagedIPReputationList | ✅ Block |
| AWSManagedReconnaissanceList | ✅ Block |
| AWSManagedIPDDoSList | ✅ Count |

3. Click **Add rule**

✅ After this step, the rule will appear in the **Add rules** list.

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/5-Workshop/5.18-WAF/1.10.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>  

---

## Step 6 – Verify the Added Rule

Verify that the following information appears:

- Rule: `AWSManagedRulesAmazonIpReputationList`  
- Status: **Saved**  
- WCU: **25 WCU**

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/5-Workshop/5.18-WAF/1.11.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>  

---

## Step 7 – Set the Web ACL Name

1. In the **Name and describe** section:
   
   - **Name:** `taskhub-waf`  
   - **Description:** (leave blank or enter any description)

---

## Step 8 – Create the Protection Pack (Web ACL)

1. Click:

**Create protection pack (web ACL)**

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/5-Workshop/5.18-WAF/1.12.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>  

2. Wait for AWS to complete the Web ACL creation

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/5-Workshop/5.18-WAF/1.13.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>  
