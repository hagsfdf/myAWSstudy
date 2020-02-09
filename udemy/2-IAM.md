# Identity Access Management (IAM)

It allows you to manage users and their level of access to the AWS console.

## Key features

- Centralised control of your AWS account
- Shared Access to your AWS account
- Granular Permissions (access to this server is OK, but other server is not)
- Identity Federation (including Active Directory, Facebook, Linkedin)
    - Active directory means that end users can access to your server just by using the same credentials as they typed in when logging in to their own AWS account
    - using facebook or other SNS accounts to grant access is allowed too.
- Multi-factor authentication (requires not only user name and password but some other special codes)
- Provides temporary access for users/devices and services... (After log in...)
- set up your own password rotation policy (change your password every 3 months... etc.)
- supports PCI DSS compliance (this is required to take credit card info. More related to legal section)

## Terminology

- Users
- Groups (Collection of users). Each user in the group will inherit the permissions of the group.
- Policies. This is made up of policy documents which is in a format called JSON.
- Roles. You can assign it to AWS application. e.g.  Assign role to EC2 in order to write to a bucket in S3.

## Hands-on Experience

There is something called root account. It has complete Administrator access.  This is usually used for logging in to your **own** AWS account. It grants you with god mode. We don't want anyone to log in using root account. So let's use MFA (Multi Factor Authentication)

It can be done by assistance of Google Authenticator application.

**Managing IAM is not confined in a single region. It has its authority to global**. IAM is universal.

We can also attach role to EC2 instance. This is already done, (like S3FullAccess)

How can we make the billing alarm? Go into CloudWatch, and make a new billing alarm setting a threshold.
