# How Amazon FSx for Lustre Uses AWS KMS<a name="FSXKMS"></a>

AA\-SERVICENAMElong; integrates with AWS Key Management Service \(AWS KMS\) for key management for encrypting data at rest\. Amazon FSx uses customer master keys \(CMKs\) to encrypt your file system in the following way:
+ **Encrypting data at rest** – Amazon FSx for Lustre uses a single customer master key \(CMK\), either the AWS managed CMK for Amazon FSx or a custom CMK, to encrypt and decrypt file system data\. For scratch file systems, Amazon FSx for Lustre uses the AWS managed CMK for Amazon FSx\. For persistent file systems, you choose the CMK used to encrypt and decrypt data, either the AWS managed CMK for Amazon FSx or a custom CMK\. You specify which key to use when you create a persistent file system\. You can enable, disable, or revoke grants on this CMK\. This CMK can be one of the two following types:
  + **AWS managed CMK for Amazon FSx** – This is the default CMK\. You're not charged to create and store a CMK, but there are usage charges\. For more information, see [AWS Key Management Service pricing](https://aws.amazon.com/kms/pricing/)\.
  + **Customer\-managed CMK** – This is the most flexible master key to use, because you can configure its key policies and grants for multiple users or services\. For more information on creating CMKs, see [Creating Keys](https://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html) in the* AWS Key Management Service Developer Guide\.*

    If you use a customer\-managed CMK as your master key for file data encryption and decryption, you can enable key rotation\. When you enable key rotation, AWS KMS automatically rotates your key once per year\. Additionally, with a customer\-managed CMK, you can choose when to disable, re\-enable, delete, or revoke access to your CMK at any time\. 

**Important**  
Amazon FSx accepts only symmetric CMKs\. You can't use asymmetric CMKs with Amazon FSx\.

## Amazon FSx Key Policies for AWS KMS<a name="FSxKMSPolicy"></a>

Key policies are the primary way to control access to CMKs\. For more information on key policies, see [Using Key Policies in AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html) in the *AWS Key Management Service Developer Guide\. *The following list describes all the AWS KMS–related permissions supported by Amazon FSx for encrypted at rest file systems:
+ **kms:Encrypt** – \(Optional\) Encrypts plaintext into ciphertext\. This permission is included in the default key policy\.
+ **kms:Decrypt** – \(Required\) Decrypts ciphertext\. Ciphertext is plaintext that has been previously encrypted\. This permission is included in the default key policy\.
+ **kms:ReEncrypt** – \(Optional\) Encrypts data on the server side with a new customer master key \(CMK\), without exposing the plaintext of the data on the client side\. The data is first decrypted and then re\-encrypted\. This permission is included in the default key policy\.
+ **kms:GenerateDataKeyWithoutPlaintext** – \(Required\) Returns a data encryption key encrypted under a CMK\. This permission is included in the default key policy under **kms:GenerateDataKey\***\.
+ **kms:CreateGrant** – \(Required\) Adds a grant to a key to specify who can use the key and under what conditions\. Grants are alternate permission mechanisms to key policies\. For more information on grants, see [Using Grants](https://docs.aws.amazon.com/kms/latest/developerguide/grants.html) in the *AWS Key Management Service Developer Guide\.* This permission is included in the default key policy\.
+ **kms:DescribeKey** – \(Required\) Provides detailed information about the specified customer master key\. This permission is included in the default key policy\.
+ **kms:ListAliases** – \(Optional\) Lists all of the key aliases in the account\. When you use the console to create an encrypted file system, this permission populates the **Select KMS master key** list\. We recommend using this permission to provide the best user experience\. This permission is included in the default key policy\.