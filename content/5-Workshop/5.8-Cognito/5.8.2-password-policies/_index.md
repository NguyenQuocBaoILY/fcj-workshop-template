---
title: "Configure Password Policies"
date: "2025-09-09"
weight: 2
chapter: false
pre: " <b> 5.8.2. </b> "
---

#### Configure Password Policies

In this step, you will configure password policies to ensure strong password requirements for your users.

1. Navigate to your **User Pool** in the Cognito console
2. Go to **Authentication methods** tab
3. Click **Edit** in the Password policy section

![Password Policy](/images/5-Workshop/5.8-Cognito/password-policy-edit.png)

#### Password Policy Configuration

Configure the following password requirements:

**Password length:**
- Minimum length: **8 characters**
- Maximum length: **256 characters**

**Password complexity:**
- ✅ Require numbers
- ✅ Require special characters
- ✅ Require uppercase letters
- ✅ Require lowercase letters

![Password Requirements](/images/5-Workshop/5.8-Cognito/password-requirements.png)


#### Save Configuration

1. Review your password policy settings
2. Click **Save changes**



#### Verify Password Policy

The password policy is now active. Users will need to create passwords that meet these requirements:


**Example of valid passwords:**
- `MySecurePass123!`
- `StrongPassword2024#`
- `TaskManager@2025`

**Example of invalid passwords:**
- `password` (no uppercase, numbers, special chars)
- `12345678` (no letters, special chars)
- `Pass1!` (too short)