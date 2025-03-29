# IAM - Passwords Policy, User Policies and Groups

## Introduction to AWS Identity and Access Management (IAM)

In many business environments, access involves a single login to a computer or a network of computer systems that provides the user access to all resources on the network. This access includes rights to personal and shared folders on a network server, company intranets, printers, and other network resources and devices. Unauthorized users can quickly exploit these same resources if the access control and associated authentication procedures are not set up properly.

In this project, I explored users, user groups, and policies in the AWS Identity and Access Management (IAM) service.

### Diagram of the current environment with the listed IAM users and IAM groups.

![image](https://github.com/user-attachments/assets/17bd97c9-d703-498d-bd67-e60e55500387)

## Objectives

After completing this project, I was able to:

- Create and apply an IAM password policy.
- Explore pre-created IAM users and user groups.
- Inspect IAM policies as applied to the pre-created user groups.
- Add users to user groups with specific capabilities active.
- Locate and use the IAM sign-in URL.
- Experiment with the effects of policies on service access.

## Task 1: Create an Account Password Policy

In this task, I created a custom password policy for my AWS account. This policy affects all the users associated with the account.

2. In the AWS Management Console, I searched for IAM and selected it.
3. In the left navigation pane, I chose **Account settings**.
4. Here, I could see the default password policy that was currently in effect. The company I was working for had much stricter requirements, so I needed to update this policy.
5. I clicked **Edit**.

![image](https://github.com/user-attachments/assets/67d2656d-4a80-4dd6-98c5-3740abf6f1d6)

7. Under **Select your account password policy requirements**, I configured the following options:
   - Changed **Enforce minimum password length** from 8 to **10 characters**.
   - Selected every checkbox except **Password expiration requires administrator reset**.
   - Left **Enable password expiration** at the default **90 days**.
   - Left **Prevent password reuse** at the default **5 passwords**.
8. I clicked **Save changes**.

![image](https://github.com/user-attachments/assets/e8e91583-07de-487d-8c2e-bac02098a5df)

These changes took effect at the AWS account level and applied to every user associated with the account.

---

## Task 2: Explore Users and User Groups

In this task, I explored the users and user groups that had already been created in IAM.

1. In the left navigation pane, I chose **Users**.
2. The following IAM users had been created:
   - **user-1**
   - **user-2**
   - **user-3**

![image](https://github.com/user-attachments/assets/a64c9dc3-409c-4817-9021-284daee489b7)

3. I selected **user-1** to view its **Summary page**.
   - In the **Permissions** tab, user-1 did not have any permissions.

![image](https://github.com/user-attachments/assets/f51d47e8-0f74-424e-add7-b2c7955d95dd)

   - In the **Groups** tab, user-1 was not a member of any user groups.

![image](https://github.com/user-attachments/assets/1a5bccb6-ca65-45ed-8885-7ebcf7f56284)

   - In the **Security credentials** tab, user-1 had a **Console password** assigned.

![image](https://github.com/user-attachments/assets/b099b777-3a8b-4113-b81b-d706cc8460fc)

4. Next, I explored user groups:
   - In the left navigation pane, I chose **User groups**.
   - The following user groups had been created:
     - **EC2-Admin**
     - **EC2-Support**
     - **S3-Support**

![image](https://github.com/user-attachments/assets/a6e9eefc-d7c3-4921-bb58-0fa1333f5495)

5. I explored the **EC2-Support** group:
   - In the **Permissions** tab, I found that this group had a managed policy called `AmazonEC2ReadOnlyAccess`.

![image](https://github.com/user-attachments/assets/e06b047b-65d3-44cd-aaf3-a3e70abfc439)

   - This policy allowed listing and describing EC2, ELB, CloudWatch, and Auto Scaling resources but not modifying them.

### AmazonEC2ReadOnlyAccess
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:Describe*",
                "ec2:GetSecurityGroupsForVpc"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "elasticloadbalancing:Describe*",
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "cloudwatch:ListMetrics",
                "cloudwatch:GetMetricStatistics",
                "cloudwatch:Describe*"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "autoscaling:Describe*",
            "Resource": "*"
        }
    ]
}
```
![image](https://github.com/user-attachments/assets/6c32b576-01d2-433f-84f8-b41330e5e448)

6. I explored the **S3-Support** group:
   - In the **Permissions** tab, this group had the `AmazonS3ReadOnlyAccess` policy attached.
   - This policy allowed users to get and list Amazon S3 resources.

### AmazonS3ReadOnlyAccess
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:Get*",
                "s3:List*",
                "s3:Describe*",
                "s3-object-lambda:Get*",
                "s3-object-lambda:List*"
            ],
            "Resource": "*"
        }
    ]
}
```
![image](https://github.com/user-attachments/assets/19188678-5d51-4966-b951-60003499f60c)

7. I explored the **EC2-Admin** group:
   - This group had a **customer inline policy** instead of a managed policy.
   - The **EC2-Admin-Policy** allowed users to describe, start, and stop EC2 instances.

### EC2-Admin-Policy
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "ec2:Describe*",
                "ec2:StartInstances",
                "ec2:StopInstances"
            ],
            "Resource": [
                "*"
            ],
            "Effect": "Allow"
        }
    ]
}
```
![image](https://github.com/user-attachments/assets/467b5e3d-d017-48ba-a371-80753b033d1c)

---

## Task 3: Add Users to Groups

In this task, I added users to groups with specific capabilities.

1. In the left navigation pane, I chose **Users Groups**.
2. I selected **S3-Support Group**.
3. In the **Users** tab, I clicked **Add users**.

![image](https://github.com/user-attachments/assets/a4693600-8b05-4326-95df-40398166f4ea)

5. I selected **user-1** to the **S3-Support** group.
6. I clicked **Add users**.

![image](https://github.com/user-attachments/assets/69f702a9-c9cb-4915-bd41-120a6a36f8bf)

Now, user-1 had read-only access to S3 resources because it inherited the `AmazonS3ReadOnlyAccess` policy from the group.

![image](https://github.com/user-attachments/assets/2d27af36-1119-4b45-afeb-6bd2c566cc75)

7. Now we repeat the same process with **user-2** to add him to **EC2-Support group** and **user-3** to **EC2-Admin group**

![image](https://github.com/user-attachments/assets/9d7a31f2-86a1-4ddb-8d78-41d9905bb370)
![image](https://github.com/user-attachments/assets/bc8cda75-0a0c-493d-9f26-7e4c3a153418)
![image](https://github.com/user-attachments/assets/4c72e1cc-77af-498f-8ec2-6ee36ed1b00f)

Now, user-2 had read-only access to EC2 resources because it inherited the `AmazonEC2ReadOnlyAccess` policy from the group.

![image](https://github.com/user-attachments/assets/7eb699c7-297f-4102-9478-00f2b4fed6e6)
![image](https://github.com/user-attachments/assets/145deb15-5793-40a8-9e3c-30cd9ea60e3f)
![image](https://github.com/user-attachments/assets/538ae32f-49f2-47ab-9622-87022f40c0fd)

Now, user-3 has Describe, Start, and Stop EC2 instances access to EC2 resources because it inherited the `AmazonEC2AdminPolicy` policy from the group.

---

## Task 4: Locate and Use the IAM Sign-in URL

In this task, I located the IAM sign-in URL, which users need to access AWS resources securely.

1. In the left navigation pane, I chose **Dashboard**.
2. I found the **IAM sign-in URL** at the top of the page.
3. I copied the **IAM sign-in URL** and tested it in a browser.

![image](https://github.com/user-attachments/assets/85c444f3-ffb5-411c-a2d8-d3743f00d1cb)

---

## Task 5: Experiment with the Effects of Policies on Service Access

In this task, I tested the effects of policies by attempting actions assigned and beyond permissions.

1. I logged in as **user-1**.

![image](https://github.com/user-attachments/assets/87e0e871-4b02-4e71-8070-6a59ee801d1b)
![image](https://github.com/user-attachments/assets/e139e205-524c-412a-894f-a169aabb6b8c)

2. Go to S3 and view the buckets

![image](https://github.com/user-attachments/assets/96a72bde-1e57-4ba2-8258-c29149db1f07)

Successfully verified that user-1 has only read access to S3 resources

Now, let's test an action beyond its permission, for example, access to Amazon EC2

![image](https://github.com/user-attachments/assets/71883295-8a7e-458d-a75e-de42af534c77)

As expected, we receive the error and can't view the resources

3. Logging in with user-2

![image](https://github.com/user-attachments/assets/f495e2cd-b4e3-4509-b367-68a163e21b92)
![image](https://github.com/user-attachments/assets/e1145402-155a-42cc-af78-1afdf178681f)

Successfully verified that user-2 has only read access to EC2 resources

Now, let's test actions beyond its permission, for example, stopping the instance and viewing S3 buckets

![image](https://github.com/user-attachments/assets/fab8865c-c9ae-4f71-97e8-255fee5226c7)
![image](https://github.com/user-attachments/assets/193c6c09-5c33-40cd-8a53-ddd8ac6a35a2)
![image](https://github.com/user-attachments/assets/3a650fa2-40de-4d5f-a32f-1efde92242f0)

As expected, we receive the errors that we can't make changes to the instance and view S3 buckets

4. Logging in with user-3

![image](https://github.com/user-attachments/assets/fe9d714e-217e-43d6-ace1-2a277e2c9927)
![image](https://github.com/user-attachments/assets/ae4204fd-5dbc-48da-ab32-17abe024e0ce)

Testing stopping the instance

![image](https://github.com/user-attachments/assets/9d218992-58e2-4581-9522-da93ef56d5d5)
![image](https://github.com/user-attachments/assets/4fbcf56d-8df2-41ba-b707-b294c104cfe4)
![image](https://github.com/user-attachments/assets/af775b52-135b-46b4-a2ad-950db5e69ee7)

Successfully verified that user-3 has access to make changes (stop) to EC2 resources

---

## Conclusion

In this project, I successfully configured IAM password policies, explored user groups, assigned permissions and tested policy effects. This experience reinforced my understanding of AWS IAM and best practices for managing identity and access control in cloud environments.




