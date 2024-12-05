### **IAM (Identity and Access Management)** Notes

---

#### **Key Components of IAM**

1. **Users**  
   - Represent individuals or services that need access to AWS.  
   - Assign unique names and define permissions using **IAM policies**.

2. **Groups**  
   - Collection of users with similar access needs.  
   - Attach policies to groups for easier permission management.

3. **Roles**  
   - Provide **temporary access** to AWS resources.  
   - Use cases:  
     - Assign roles to AWS services (e.g., EC2, Lambda).  
     - Enable cross-account access or external federation.  

4. **Policies**  
   - Define permissions in JSON format.  
   - Types:  
     - **Managed Policies**: Reusable policies created by AWS or you.  
     - **Inline Policies**: Specific to a user, group, or role.  
   - Components:  
     - **Actions** (e.g., `s3:ListBucket`)  
     - **Resources** (e.g., specific S3 bucket)  
     - **Conditions** (e.g., IP-based restrictions).

5. **Identity Providers**  
   - Integrate external identity systems (e.g., SAML, Google Workspace) for **federated access**.  
   - Enable **single sign-on (SSO)**.

---

#### **Best Practices**

1. **Grant Least Privilege**  
   - Assign only the permissions necessary for tasks.  
   - Regularly review and refine access policies.

2. **Use Groups for Permissions**  
   - Manage permissions by attaching policies to groups instead of individuals.

3. **Enable Multi-Factor Authentication (MFA)**  
   - Enforce MFA for sensitive operations and console access.

4. **Use Roles for Applications**  
   - Assign IAM roles to EC2, Lambda, or ECS tasks to avoid hardcoding credentials.

5. **Rotate Access Keys Regularly**  
   - Avoid long-term keys for users; use roles wherever possible.

6. **Monitor and Audit IAM Activity**  
   - Use **AWS CloudTrail** to log and review IAM activity.  
   - Analyze **IAM Access Analyzer** for over-permissive policies.

7. **Set Permissions Boundaries**  
   - Define maximum permissions that users or roles can have.

---
## Policy 

Template
```
{
  "Version": "2012-10-17", # Version is date
  "Statement": [
    {
      "Effect": "Allow", // "Allow" or "Deny" the specified actions
      "Action": [        // List of AWS actions this policy permits/denies
        "service:Action1",  // like "s3:GetObject",
        "service:Action2"  // like "s3:ListBucket" 
      ],
      "Resource": [      // Specify the AWS resources this policy applies to
        "arn:aws:service:region:account-id:resource-type/resource-id",
        "arn:aws:service:region:account-id:resource-type/resource-id/*"
      ],
      "Condition": {     // Optional: Add conditions for granular control
        "ConditionKey": {
          "ConditionOperator": "ConditionValue"
        }
      }
    }
  ]
}
```
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::123456789012:user/JohnDoe"
      },
      "Action": [
        "s3:GetObject",
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:s3:::your-bucket-name",
        "arn:aws:s3:::your-bucket-name/*"
      ]
    }
  ]
}
```
## Types of policy

In AWS, whether or not a policy includes a **Principal** depends on **the type of policy** being used. Policies are generally classified into **resource-based policies** and **identity-based policies**, and the role of the **Principal** differs based on the type.

### **1. Identity-Based Policies (No Principal Needed)**

- **What They Are**: These policies are attached directly to IAM **users**, **groups**, or **roles**. 
- **Principal Implicit**: The **Principal** is implicit because the policy is attached to a specific identity (user, group, or role). There's no need to explicitly define the Principal in the policy.

**Examples of Identity-Based Policies**:
- A policy attached to an IAM user will automatically apply to that user, meaning the user is the **Principal** in this case.
- A policy attached to an IAM role is implicitly referring to the role as the **Principal**.

**Example Identity-Based Policy**:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:ListBucket",
      "Resource": "arn:aws:s3:::my-bucket"
    }
  ]
}
```
- This policy applies to the user, group, or role that it's attached to, so there's no need to explicitly mention a **Principal**.

### **2. Resource-Based Policies (Require a Principal)**

- **What They Are**: These policies are directly attached to **AWS resources** like S3 buckets, Lambda functions, or SNS topics.  
- **Principal Defined Explicitly**: In **resource-based policies**, you **must specify a Principal** to define **who** or **what** can access the resource. Since the policy is attached to the resource itself, the **Principal** must be defined to specify the entity that is being granted or denied access.

**Examples of Resource-Based Policies**:
- **S3 Bucket Policies**: You define who (IAM user, role, or even another AWS account) can access the bucket and its contents.
- **Lambda Permissions**: You define which services (like API Gateway or CloudWatch) can invoke the Lambda function.

**Example Resource-Based Policy (S3 Bucket)**:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::123456789012:user/JohnDoe"
      },
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-bucket/*"
    }
  ]
}
```
- Here, the **Principal** is `arn:aws:iam::123456789012:user/JohnDoe`, and it grants this specific user access to the S3 bucket.

### **Key Differences**

| **Policy Type**            | **Principal**        | **Use Case**                                 |
|----------------------------|----------------------|----------------------------------------------|
| **Identity-Based Policy**   | No Principal         | Attached to users, groups, or roles (implicitly refers to the entity it is attached to). |
| **Resource-Based Policy**   | Requires a Principal | Attached to AWS resources (e.g., S3, Lambda) and specifies who (IAM user, role, service, etc.) can access the resource. |

---

### **When to Use a Principal**

- **Identity-Based Policies**: You **don’t need to include a Principal** because the policy is attached directly to an identity (IAM user, group, or role).
- **Resource-Based Policies**: You **must include a Principal** to specify **who** or **what** can access the resource. 

---

### **Why Some Policies Don't Have a Principal**
- **Identity-Based Policies** are designed to manage what the attached user, group, or role can do, so the **Principal** is inherently defined by the user, group, or role the policy is attached to.
- **Resource-Based Policies**, on the other hand, apply to a specific resource and need to define the **Principal** explicitly to identify the entity allowed to access that resource.

---

I hope that clarifies it! Let me know if you need more details.