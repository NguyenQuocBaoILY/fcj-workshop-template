---
title: "Setup Email Verification"
date: "2025-09-09"
weight: 3
chapter: false
pre: " <b> 5.8.3. </b> "
---

#### Setup Email Verification

In this step, you will configure email verification to ensure users verify their email addresses during registration.

1. Navigate to your **User Pool** in the Cognito console
2. Go to **Sign-up experience** tab
3. Click **Edit** in the Attribute verification and user account confirmation section

![Email Verification](/images/5-Workshop/5.8-Cognito/email-verification-edit.png)

#### Configure Email Verification

**Attribute verification and user account confirmation:**
- ✅ **Send email message, verify email address**
- Verification message: **Code**
- Email verification subject: `Verify your email for Task Management System`
- Email verification message: 
```
Your verification code for Task Management System is {####}. 
Please enter this code to complete your registration.
```

![Email Settings](/images/5-Workshop/5.8-Cognito/email-settings.png)

#### Configure Email Delivery

**Email delivery method:**
- **Send email with Cognito** (for development)
- FROM email address: **no-reply@verificationemail.com**

![Email Delivery](/images/5-Workshop/5.8-Cognito/email-delivery.png)

**Note:** For production applications, consider using **Amazon SES** for better email deliverability and custom domains.

