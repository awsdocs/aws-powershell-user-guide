# Identity and Access Management for the AWS Tools for PowerShell<a name="pstools-security-iam"></a>

The AWS Tools for PowerShell uses the same IAM users and roles that you use to access your AWS resources and their services with the AWS Management Console\. The policies that grant permissions are also the same because the AWS Tools for PowerShell calls the same API operations that are used by the service console\. For more information, see the "Identity and Access Management" section in the "Security" chapter for the AWS service that you want to use\.

The only major difference is how you authenticate when using a standard IAM user and long\-term credentials\. Although an IAM user requires a password to access an AWS service's console, that same IAM user requires an access key instead of a password to perform the same operations using the AWS Tools for PowerShell\. All other short\-term credentials are used in the same way they are used with the console\.

The credentials used by the AWS Tools for PowerShell are typically stored in plaintext files and are ***not*** encrypted\. However, you do have an option to use the encrypted \.NET SDK credential store when you run on Windows\.
+ The `$HOME/.aws/credentials` file stores long\-term credentials required to access your AWS resources\. These include your access key ID and secret access key\. 

**Mitigation of Risk**
+ We strongly recommend that you configure your file system permissions on the `$HOME/.aws` folder and its child folders and files to restrict access to only authorized users\.
+ Use roles with temporary credentials wherever possible to reduce the opportunity for damage if the credentials are compromised\. Use long\-term credentials only to request and refresh short\-term role credentials\.