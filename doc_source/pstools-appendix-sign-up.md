# AWS Account and Access Keys<a name="pstools-appendix-sign-up"></a>

To access AWS, you will need to sign up for an AWS account\.

Access keys consist of an *access key ID* and *secret access key*, which are used to sign programmatic requests that you make to AWS\. If you don't have access keys, you can create them by using the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\. We recommend that you use IAM access keys instead of AWS root account access keys\. IAM lets you securely control access to AWS services and resources in your AWS account\.

**Note**  
To create access keys, you must have permissions to perform the required IAM actions\. For more information, see [Granting IAM User Permission to Manage Password Policy and Credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_delegate-permissions.html) in the *IAM User Guide*\.

## To get your access key ID and secret access key<a name="get-access-keys"></a>

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. On the navigation menu, choose **Users**\.

1. Choose your IAM user name \(not the check box\)\.

1. Open the **Security credentials** tab, and then choose **Create access key**\.

1. To see the new access key, choose **Show**\. Your credentials resemble the following:
   + Access key ID: `AKIAIOSFODNN7EXAMPLE` 
   + Secret access key: `wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY` 

1. To download the key pair, choose **Download \.csv file**\. Store the \.csv file with keys in a secure location\.

**Important**  
Keep the keys confidential to protect your AWS account, and never email them\. Do not share them outside your organization, even if an inquiry appears to come from AWS or Amazon\.com\. *No one who legitimately represents Amazon will ever ask you for your secret key\.* 
You can retrieve the secret access key ***only*** when you initially create the key pair\. Like a password, you can't retrieve it later\. If you lose it, you must create a new key pair\.

 **Related topics** 
+  [What Is IAM?](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) in the *IAM User Guide*\.
+  [AWS Security Credentials](https://docs.aws.amazon.com/general/latest/gr/aws-security-credentials.html) in the *Amazon Web Services General Reference*\.