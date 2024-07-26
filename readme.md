1. Create an IAM user and assign **AmazonS3FullAccess** permission to user.
2. Create access key for user by navigating to IAM >> Users >> Click on User >> Security Credentials >> Access keys >> Create Access Key >> Application running outside AWS >> Give a name and create it
3. Note down the `access key` and `secret key`, we have to use it later in `config.yaml` file.
4. Login with IAM user and Create a new bucket by going to S3 service. Create Bucket >> General Purpose >> Give a bucket name >> ACLs disabled >> Untick the "Block all public access" >> click on Create Bucket to create it
5. After creation Go to Bucket >> Permissions >> Bucket policy >> Add following policy to it. In resource field **quaybucket** is the bucket name.
```bash
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:*",
            "Resource": "arn:aws:s3:::quaybucket/*"
        }
    ]
}
```
6. Aws part is configured, now configure quay to use the AWS object storage. Specify the `access_key` and `secret_key` created in Step 3. Replace `quaybucket` with bucket name. Also set the `region` in which you have created the bucket. You can keep the `/datastorage/registry` as it is, quay will be creating the objects in it.

```bash
cat config.yaml
DISTRIBUTED_STORAGE_CONFIG:
  s3Storage::
    - S3Storage
    - host: s3.us-east-1.amazonaws.com
      s3_bucket: quaybucket
      s3_access_key: ******************
      s3_secret_key: *************************
      s3_region: us-east-1
      storage_path: /datastorage/registry
DISTRIBUTED_STORAGE_DEFAULT_LOCATIONS: []
DISTRIBUTED_STORAGE_PREFERENCE:
    - s3Storage

```
