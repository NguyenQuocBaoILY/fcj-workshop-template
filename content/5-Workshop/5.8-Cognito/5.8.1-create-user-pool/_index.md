---
title: "Create Cognito User Pool"
date: "2025-09-09"
weight: 1
chapter: false
pre: " <b> 5.8.1. </b> "
---

#### Create User Pool

In this step, you will create a Cognito User Pool to manage user authentication for your application.

1. Navigate to **AWS Cognito** service in the AWS Console
2. Click **Create user pool**

![Create User Pool](/images/5-Workshop/5.8-Cognito/create-user-pool-1.png)

3. **Configure options**
   
   **Options for sign-in identifiers:**
   - Select **Email** (allows users to sign in with email)
   - Uncheck **Phone number** and **Username**
   
   **Self-registration:**
   - ✅ **Enable self-registration** (allows users to sign up themselves)
   
   **Required attributes for sign-up:**
   - Select **email** and **name** as required attributes
   
   ![Configure Options](/images/5-Workshop/5.8-Cognito/configure-options.png)
   
   - Click **Create user pool**

4. **Enter User Pool Name**
   - User pool name: **TaskManagementUserPool**
   - Click **Create user pool**

![User Pool Name](/images/5-Workshop/5.8-Cognito/user-pool-name.png)

#### Verify User Pool Creation

After successful creation, you should see your User Pool in the Cognito console with default configurations:

![User Pool Created](/images/5-Workshop/5.8-Cognito/user-pool-created.png)

#### Review Default Configuration

Cognito automatically creates the User Pool with default settings:

**Sign-in experience:**
- Sign-in options: **Username** (can be changed to Email)
- Password policy: **Cognito defaults**
- Multi-factor authentication: **Optional**

![Default Settings](/images/5-Workshop/5.8-Cognito/default-settings.png)

**Sign-up experience:**
- Self-service sign-up: **Enabled**
- Email verification: **Enabled**
- Required attributes: **email**

**Message delivery:**
- Email provider: **Send email with Cognito**

**App integration:**
- Hosted UI: **Not configured** (will be configured in later steps)

#### Important Note

⚠️ **Warning:** Options for sign-in identifiers and required attributes **cannot be changed** after the User Pool is created. If you need to change these, you must create a new User Pool.

Note down the **User Pool ID** from the overview page as you'll need it for the next steps:



In the following steps, we will configure detailed features like password policies, email verification, and create App Clients.