# Encrypt-S3-bucket-EBS-Volume-and-AMI-using-AWS-KMS

# Encryption-and-Decryption-Using-KMS

## Purpose of the Hands on

Use (AWS) Key Management Service (KMS) for Encrypting data on S3 bucket, EBS volume and AMI & Snaphot. 
KMS provides a central way to manage cryptographic keys for various AWS services and our own applications. AWS KMS makes it easy to create and manage encryption keys, and it offers a range of features and integrations to enhance the security of our data.


## Architecture

![Architecture for KMS Key](https://github.com/user-attachments/assets/e2b8ceb1-20b7-4e2c-815b-a6d32005d975)






### Step-by-Step Implementation

<b> Note: we work on us-east-1 region. only the  <b>bucket that will replicate data from the source bucket</b> in step 5 use ap-south-1 region.</b>

1. Create an AWS KMS key.  (reference: 1-Create a KMS key.png)
   - Define a key administrative permissions(User already created).
   - Define a key usage permissions(select the same username).
   - Key Policy(JSON policy).
   - Enable Key Rotation for 365 days.
  
     
2. Create Encrypted S3 Bucket with KMS key.  (reference: 2-Create S3 Bucket with KMS key.png)
   - Enable ACL with Object Writer.
   - Uncheck <b>Block all public access</b>
   - Enable Versioning.
   - Encyption Type: SSE-KMS.
  
     
3. Encrypt and Upload an object to S3 bucket.  (reference: 3-Object Image.png)
   - The picture uploaded (Object) and added to Storage Class <b>One Zone-IA</b>
   - Encryption type: SSE-KMS.
  
     
4. Verifying the encryption of the object.  
   - Make it our object public using ACL.
   - <b>Test</b>: By copy Object URL, we are getting error because the file was encrypted using AWS KMS, The access from outside source was restricted.  (reference: 4-Copy the Object image.png & 4-Unauthorization to access.png)
   - The object image was opened locally from the source which can decrypt the file by <b>Open</b> button.  (reference: 5-Open Image.png & 5-Open image locally.png)
  
     
5. Cross-Region Replication and Versioning in S3.
   - Create bucket in <b>ap-south-1</b>that will replicate data from the source bucket us-east-1.
   - Uncheck <b>Block all public access</b>
   - Enable Versioning.  (reference: 6-Bucket to replicat data in.png) 
   - On the source bucket we create <b>Replication rule</b> and we choose the second bucket as destination and <b>aws/s3</b> as AWS KMS key.  (reference: 7-Replication Rules.png) 
   - <b>Test: On the source bucket we upload new Object (image test) with Encryption key type: SSE-S3. (reference: 8-Image test successfully replicated to second bucket.png & 8-test object in source bucket.png)</b>


6. Disabling the KMS Key
   - Disable the KMS key.  (reference: 9-Key disabled.png)
   - <b>Test: Access to Image on the bucket source (attached to KMS key not the last one attached to SSE-S3) was denied as the KMS key was disabled.  (reference: 10-Object image.png & 8-test object in source bucket.png)</b>


7. Encryption EBS Volume
   - Create a Volume:
     -- name AdminEBS
     -- GP2 type
     -- 1GiB Size
     -- Encryption choose our AWS KMS key (Administrator)  (reference: 11-EBS volume with KMS key encryption.png)

8. Encrypting AMI and Snapshot.
   -  Create an Image with name <b>AdminUnencrypted</b> on the EC2 instance already created.  (reference:  12-Create an Image.png)
   -  Copy AMI with name <b>AdminEncrypted</b> and check <b>Encypt EBS snapshots of AMI copy</b> and add our KMS key.  (reference:  13-Copy AMI from AdminUnencrypted AMI.png)
   -  search the ID of <b>AdminEncrypted</b> AMI on Snapshot  (reference:  14-AdminEncrypted on snapshot.png)
   -  <b>Test: AMI <b>AdminEncrypted</b> that was copied from <b>AdminUnencrypted</b> has the same AWS KMS key.  (reference:  15-AdminUnencrypted AMI with AWS KMS key.png)</b>
     





   


> *Lab originally from: [Encrypt S3 bucket, EBS Volume and AMI using AWS KMS](https://www.whizlabs.com/labs/encrypt-s3-bucket-ebs-volume-and-ami-using-aws-kms/))*



