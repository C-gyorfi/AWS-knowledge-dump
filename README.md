# AWS Certified Developer Associate - Knowledge dump

## Contents

- [IAM & AWS CLI](#iam-&-aws-cli)
  - [Users and Groups](#users-and-groups)
  - [Policies](#policies)
  - [Access](#access)
  - [More about IAM](#more-about-iam)
  - [Responsibilities](#responsibilities)

## IAM & AWS CLI
- **IAM** stands for **Identity and Access Management**
- It is a Global service

### Users and groups
- Users (people within the organization)
- Groups -> can contain one or more users(one user can be in multiple group)
- Assigned by policies(JSON format) which gives permissions of the users
- Follows the least privilege principle

### Policies
- Managed policies(created by AWS, e.g IAMReadOnlyAccess-> user can view IAM roles)
- Password Policy - AWS allows setup password policy e.g required chars, allow users to change, expire after a given time or require MFA

### Access
- AWS console
- AWS CLI
- AWS SDK

### More about IAM
- IAM Roles allows AWS services to perform actions
- IAM Credentials Report - report on users and status of their credentials
- IAM Access Advisor - shows the service permissions of a given user and last usage

### Responsibilities
- AWS is responsible for:
> Infrastructure and global network security, configuration/vulnerability analysis and compliance validation

- Organisation/User is responsible for:
>Managing/Monitoring Users, Groups, Roles, Policies, enabling MFA and rotating keys and reviewing permissions
