
* Create root account
  * Enable MFA for root accnt.
* Create another Admin account similar to root
  * Enable MFA
* 


**AWS Account**
* It's a container of AWS resources.
* Using multiple AWS accounts is a best practice for scaling environments.
* Provides a natural billing boundary for costs
* Isolates resources for security

**AWS Organization**
* It's a collection of AWS accounts that can be organized into a hierarchy and managed centrally.
* Help to programmatically create new accounts and allocate resources, and simplify billing by setting up a single payment method for all accounts.
* Can integrate with other AWS services so admins can define central configurations, security mechanisms, and resource sharing across accounts.

**AWS user**
* It's an AWS identity created directly in the AWS IAM or AWS IAM Identity Center admin console that consists of a name and credentials.

**Group**
* It's a collection of users. 
* Let admins specify permissions for multiple users, which can make it easier to manage the permissions.

**Role**
* Similar to a user in that it is an AWS identity with permissions and policies that determine what the identity can and cannot do in an AWS account.
* Instead of being uniquely associated with one person, a role is intended to be assumable by anyone who needs it.
* Does not have standard long-term credentials such as a password or access keys associated with it. Instead, when a user assumes a role, it provides them with a set of temporary security credentials for that session.
* Used to provide access to users, applications, or services that don’t normally have access to those AWS resources. 

**AWS IAM Identity Center Permission Set**
* It defines the level of access a user has to AWS resources within an AWS account.

**AWS IAM - AWS Identity and Access Management**
 * Enable admins to manage access to AWS services and resources within an AWS account securely for what it calls “entities" (IAM users created from the AWS IAM admin console, federated users, application code, or another AWS service).
 * Admins can create and manage AWS users and groups directly, and use permissions to allow and deny their access to AWS resources.
 * AWS IAM supports setting up multiple identity providers per account.

**AWS IAM Identity Center**
* Also manages access to AWS services and resources. 
* The difference between AWS IAM and AWS IAM Identity Center is that the latter manages access for all AWS accounts within an AWS Organization, as well as access to other cloud applications, e.g., Salesforce.
* Includes a user portal where end users can find and access their assigned AWS accounts, cloud applications, and custom applications in one place.
* Only supports a single identity provider.
* AWS is pushing their users to switch from AWS IAM to AWS IAM Identity Center.

Source:
https://jumpcloud.com/blog/aws-iam-vs-aws-sso