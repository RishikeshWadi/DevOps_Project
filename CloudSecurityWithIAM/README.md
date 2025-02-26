# Cloud Security with AWS IAM

In AWS, a user can be a person or a system entity that interacts with the AWS cloud. AWS Identity and Access Management (IAM) helps manage authentication (sign-in) and authorization (permissions) for accessing AWS resources.

## Get Ready to Create:
1. 💻 EC2 Instances
2. 📏 IAM Policies
3. 👩‍👩‍👧‍👧 IAM Users and User Groups
4. 🔖 AWS Account Alias

---

## Activity Diagram

![AD](https://github.com/RishikeshWadi/DevOps_Project/blob/main/CloudSecurityWithIAM/projectImages/CloudSecurity_AWS_IAM_Activity_Diagram.png)


---

## Step #1: Launch EC2 Instance 💡

### What is EC2?
Amazon EC2 (Elastic Compute Cloud) provides virtual computing resources in the cloud, allowing you to create, customize, and use instances for applications, hosting, and more.

- **Elastic** = Flexible, adjusts in size and power as needed.
- **Compute** = Processing power.
- **Cloud** = Available via the internet.

### Steps to Launch EC2 Instance
1. Log in to your **AWS Management Console**.
2. Navigate to **EC2**.
3. Set the **Region** closest to you.
4. Click **Launch instances**.
5. In **Name**, enter `production-yourname` (replace with your name).
6. Click **Add additional tags** → **Add new tag**:
   - **Key:** Env
   - **Value:** production
![ ](https://github.com/RishikeshWadi/DevOps_Project/blob/main/CloudSecurityWithIAM/projectImages/EC2InstanceNameandTags.png)

💡 **Why create tags?**
- Tags help organize AWS resources.
- The **Env** tag labels instances as `production` or `development`.
- Helps filter resources, manage costs, and enforce policies.

7. Verify **Amazon Machine Image (AMI)** settings.
8. Choose a **Free tier eligible** instance type.
9. **Key Pair (Login):** Use an existing key pair or create a new one.

💡 **What is a Key Pair?**
A key pair enables secure SSH access to an EC2 instance. Proceeding without one is **not recommended** as it restricts secure access outside the AWS Console.

10. Configure **network and storage settings**.
    - **Network settings** determine how instances interact with other resources.
    - **Storage settings** define storage volume type and size.

11. Click **Launch instance**.

### Create a Development EC2 Instance
Repeat the above steps with the following details:
- **Name:** development-yourname
- **Env:** development

![ ](https://github.com/RishikeshWadi/DevOps_Project/blob/main/CloudSecurityWithIAM/projectImages/development-rishikesh-tag.png)

✅ You've successfully deployed **two EC2 instances** for production and development environments!

---

## Step #2: Create an IAM Policy 📏

### What is IAM?
IAM (Identity and Access Management) is used to control user and service access to AWS resources.

### What is an IAM Policy?
An IAM policy defines **who can do what** with AWS resources.

### Steps to Create IAM Policy
1. Navigate to the **IAM Console**.
2. Click **Policies** → **Create policy**.
3. Switch to the **JSON** editor. (. (we can create and edit AWS policies in the visual editor or JSON)
4. Use the following policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "ec2:*",
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "ec2:ResourceTag/Env": "development"
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": "ec2:Describe*",
      "Resource": "*"
    },
    {
      "Effect": "Deny",
      "Action": [
        "ec2:DeleteTags",
        "ec2:CreateTags"
      ],
      "Resource": "*"
    }
  ]
}
```

💡 **Structure of JSON Policies:**
- **Version:** Identifies the policy language version (e.g., "2012-10-17" is the latest supported version).
- **Statement:** The core part of the policy, containing permissions.
- **Effect:** Specifies whether the policy allows (`Allow`) or denies (`Deny`) an action.
- **Action:** Lists actions that the policy permits or denies. (e.g., `ec2:*` allows all EC2 actions).
- **Resource:** Defines the AWS resources affected (`*` applies to all resources).
- **Condition (Optional):** Adds specific conditions for when the policy is applied, such as allowing access only to instances tagged `Env=development`.

![ ](https://github.com/RishikeshWadi/DevOps_Project/blob/main/CloudSecurityWithIAM/projectImages/development-policies-json.png)

5. Click **Next**.
6. Name the policy `DevEnvironmentPolicy`.
7. Add a description: *IAM Policy for development environment*.
8. Click **Create policy**.

---

## Step #3: Create an Account Alias 🔖

### What is an Account Alias?
An **Account Alias** is a user-friendly name for your AWS account, making sign-in easier instead of using a numeric account ID.

### Steps to Create an Account Alias
1. Go to the **IAM dashboard**.
2. Click **Create** under **Account Alias**.
3. Enter `alias-preferredName`.
4. Click **Create alias**.

---

## Step #4: Create IAM Users and User Groups 👩‍👩‍👧‍👧

### What is an IAM User Group?
A **User Group** is a collection of users with shared permissions.

### What is an IAM User?
An **IAM User** is an individual who accesses AWS resources.

### Steps to Create a User Group
1. In **IAM**, select **User Groups** → **Create Group**.
2. **Name:** `dev-group`.
3. Attach **DevEnvironmentPolicy**.
4. Click **Create user group**.

### Steps to Create an IAM User
1. In **IAM**, select **Users** → **Create user**.
2. **User name:** `dev-yourname`.
3. Enable **AWS Management Console access**.
4. Select **I want to create an IAM user**.
5. Uncheck **Require password change at next sign-in**.
6. Add user to `dev-group`.
7. Click **Next** → **Create user**.

✅ IAM user and user group setup complete!

---

## Step #5: Test Your Setup 💡

### Steps to Test IAM Permissions
1. Copy the **AWS Console sign-in URL**.
2. Open **Incognito Mode** in your browser.
3. Access the sign-in URL.
4. Log in as `dev-yourname`.
5. Verify restricted access:
   - Attempt to stop the **production instance**.
   - **Access denied** message should appear.
6. Attempt to stop the **development instance**.
   - **Success!** Instance should stop.

✅ You have successfully implemented **AWS IAM security best practices** with EC2 instances, IAM policies, user groups, and account aliasing! 🚀
