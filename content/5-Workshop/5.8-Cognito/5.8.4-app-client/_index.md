---
title: "Configure App Client"
date: "2025-09-09"
weight: 4
chapter: false
pre: " <b> 5.8.4. </b> "
---

#### Configure App Client

When creating the User Pool, AWS Cognito automatically created a default App Client. In this step, we will configure this App Client to suit our application needs.

1. Navigate to your **User Pool** in the Cognito console
2. Go to **App integration** tab
3. You will see the App Client has already been created

![Default App Client](/images/5-Workshop/5.8-Cognito/default-app-client.png)

#### Edit App Client

1. Click on the App Client name to edit it
2. Or click **Edit** if available


**Update app client information:**
- App client name: **TaskManagementWebApp** (if you want to change it)
- App client type: **Public client**
- Authentication flows: 
  - ✅ **ALLOW_USER_PASSWORD_AUTH**
  - ✅ **ALLOW_REFRESH_TOKEN_AUTH**
  - ✅ **ALLOW_USER_SRP_AUTH**

![App Client Config](/images/5-Workshop/5.8-Cognito/app-client-config.png)

#### Configure Hosted UI

**Hosted UI settings:**
- Use the Cognito Hosted UI: **Enabled**
- Domain type: **Use a Cognito domain**
- Cognito domain: **taskmanagement-auth-[your-unique-id]**

![Hosted UI Domain](/images/5-Workshop/5.8-Cognito/hosted-ui-domain.png)

**Initial app client settings:**
- Allowed callback URLs: 
  - `http://localhost:3000/callback`
  - `https://your-app-domain.com/callback`
- Allowed sign-out URLs:
  - `http://localhost:3000/`
  - `https://your-app-domain.com/`

![Callback URLs](/images/5-Workshop/5.8-Cognito/callback-urls.png)

**OAuth 2.0 settings:**
- Allowed OAuth flows:
  - ✅ **Authorization code grant**
  - ✅ **Implicit grant**
- Allowed OAuth scopes:
  - ✅ **email**
  - ✅ **openid**
  - ✅ **profile**

![OAuth Settings](/images/5-Workshop/5.8-Cognito/oauth-settings.png)

#### Save Configuration

1. Review all configurations
2. Click **Save changes**



#### Verify App Client Configuration

After updating, note down the important information:

**App Client Details:**
- Client ID: `[your-client-id]`
- Hosted UI URL: `https://taskmanagement-auth-[your-id].auth.us-east-1.amazoncognito.com`



The App Client is now configured and ready to handle authentication requests from your application. You can use the **Client ID** and **Hosted UI URL** to integrate with your web or mobile application.