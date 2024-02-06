Task 1: Creating and configuring resources

Task 1.1: Create an S3 bucket
1. Open AWS Management Console, navigate to S3.
2. Choose Create bucket.
3. Configure:
   - Bucket name: Enter a unique name (s3-bucket-name).
   - Region: Leave as default.
4. Scroll and choose Create bucket.

Task 1.2: Attach instance profile to Processor
1. Open EC2 Management Console, navigate to Instances.
2. Choose Processor.
3. Actions > Security > Modify IAM role.
4. Choose S3BucketAccess role, click Update IAM role.

Task 2: Taking snapshots of your instance

Task 2.1: Connecting to the Command Host EC2 instance
1. Open EC2 Management Console, navigate to Instances.
2. Choose Command Host.
3. Choose Connect on EC2 Instance Connect tab.

Task 2.2: Taking an initial snapshot
- Obtain VolumeId and InstanceId.
- Stop Processor instance.
- Take an initial snapshot using VolumeId.
- Wait for snapshot completion.
- Restart Processor instance.

Task 2.3: Scheduling creation of subsequent snapshots
- Use cron to schedule snapshot creation every minute.

Task 2.4: Retaining the last two snapshots
- Stop cron job.
- Examine snapshotter_v2.py.
- Run the script to maintain only the last two snapshots.

Task 3: Challenge: Synchronize files with Amazon S3

Challenge Description
- Download files.zip.
- Unzip files.
- Activate versioning, sync files, delete locally, sync delete to S3, recover deleted file using versioning.

Solution Summary
- Use AWS CLI commands for versioning, syncing, deleting, and recovering.

Task 3.1: Downloading and unzipping sample files
- Connect to Processor instance.
- Run wget and unzip commands.

Task 3.2: Syncing files
- Activate versioning with put-bucket-versioning.
- Sync files to S3 with s3 sync.
- Delete locally and sync delete to S3.
- Recover deleted file using list-object-versions and get-object.

Shell Scripts:

1. Activate Versioning:
```bash
aws s3api put-bucket-versioning --bucket S3-BUCKET-NAME --versioning-configuration Status=Enabled


Sync Files to S3: aws s3 sync files s3://S3-BUCKET-NAME/files/

Delete Locally and Sync Delete to S3:
rm files/file1.txt
aws s3 sync files s3://S3-BUCKET-NAME/files/ --delete


Recover Deleted File using Versioning:
aws s3api list-object-versions --bucket S3-BUCKET-NAME --prefix files/file1.txt
aws s3api get-object --bucket S3-BUCKET-NAME --key files/file1.txt --version-id VERSION-ID files/file1.txt
