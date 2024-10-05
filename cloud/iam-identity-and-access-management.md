# IAM (Identity & Access management)

## IAM & Global service

IAM is Identity and Access Management, Global service&#x20;

* Root account **created by default**, shouldn’t be used or shared&#x20;
* <mark style="color:red;">**Users**</mark> are people within your organization, and <mark style="color:red;">**can be grouped**</mark>&#x20;
* &#x20;**Groups only contain users**, not other groups&#x20;
* Users don’t have to belong to a group, and <mark style="color:blue;">**user can belong to multiple groups**</mark>

## Policy

* Users or Groups can be **assigned JSON documents** called <mark style="color:red;">**policies**</mark>
* These policies <mark style="color:red;">**define the permissions**</mark> of the users&#x20;
* &#x20;In AWS you apply the least privilege principle: <mark style="color:blue;">don’t give more permissions</mark> than a <mark style="color:orange;">user needs</mark>

{% code lineNumbers="true" %}
```json

 "Version": "2012-10-17",
 "Id": ""
 "Statement": [
    {
    "Effect": "Allow",
    "Action": "ec2:Describe*",
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
    }
  ]
```
{% endcode %}

### Policy Consists of&#x20;

<table><thead><tr><th width="203">Policy Element</th><th>Description</th></tr></thead><tbody><tr><td>Version</td><td>Policy language version, always include "2024-1017"</td></tr><tr><td>Id</td><td>An identifier for the policy (optional) </td></tr><tr><td>Statement</td><td>One or more individual statements (required) </td></tr></tbody></table>

### Statements consists of:

<table><thead><tr><th width="184">Policy Element</th><th>Description</th></tr></thead><tbody><tr><td>Sid</td><td>An identifier for the statement (optional)</td></tr><tr><td>Effect</td><td>Whether the statement allows or denies access (Allow, Deny)</td></tr><tr><td>Principal</td><td>Account/user/role to which this policy applies</td></tr><tr><td>Action</td><td>List of actions this policy allows or denies</td></tr><tr><td>Resource</td><td>List of resources to which the actions apply</td></tr><tr><td>Condition</td><td>Conditions for when this policy is in effect (optional)</td></tr></tbody></table>

```json
{
  "Version": "2012-10-17",
  "Id": "S3-Account-Permissions",
  "Statement": [
    {
      "Sid": "1",
      "Effect": "Allow",
      "Principal": {
        "AWS": ["arn:aws:iam::123456789012:root"]
      },
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": ["arn:aws:s3:::mybucket/*"]
    }
  ]
}
```

## To access AWS, you have three options:&#x20;

1. AWS Management Console (protected by password + MFA)
2. AWS Command Line Interface (CLI): protected by access keys
3. AWS Software Developer Kit (SDK) - for code: protected by access keys

## IAM Roles for Services

* Some <mark style="color:orange;">AWS services</mark> need to <mark style="color:orange;">perform actions on your behalf</mark>.
* To do so, permissions are assigned to AWS services using **IAM Roles**.

#### Common IAM Roles:

* **EC2 Instance Roles**: Grant permissions to EC2 instances to interact with other AWS services.
* **Lambda Function Roles**: Allow Lambda functions to access AWS resources or perform actions.
* **Roles for CloudFormation**: Enable CloudFormation to create or manage resources on your behalf.

## IAM Security Tools

1. **IAM Credentials Report** (account-level):
   * A report that <mark style="color:purple;">**lists all your account's users and the status**</mark> of their various credentials.
2. **IAM Access Advisor** (user-level):
   * Shows the <mark style="color:green;">**service permissions granted to a user**</mark> and when those services were <mark style="color:green;">**last accessed**</mark>.
   * You can use this information to revise your policies and optimize access permissions.







