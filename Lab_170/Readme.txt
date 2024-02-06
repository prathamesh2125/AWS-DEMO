*********************************Creating a Website on S3*********************************

Task 1: Use SSH to connect to an Amazon Linux EC2 instance
=>	1.Open putty.exe.
	2.Configure the PuTTY timeout to keep the PuTTY session open for a longer period of time:
		>Choose Connection.
		>For Seconds between keepalives, enter 30
	3.Configure your PuTTY session:
		>Choose Session.
		>For the Host Name (or IP address), enter the PublicIP address that you copied from the previous steps.
		>In PuTTY in the Connection list, choose SSH to expand it.
		>Choose Auth, but don't expand it.
		>Choose Browse.
		>Browse to and select the labuser.ppk file that you downloaded.
		>To choose the file, choose Open.
		>Choose Open again.
	4.In the PuTTY Security Alert window, choose Accept to trust and connect to the host.
	5.When prompted with login as, enter ec2-user and press Enter.
	\\This step connects you to the EC2 instance.



Task 2: Configure the AWS CLI
=>	1.In the SSH session terminal window, run the configure command to update the AWS CLI software with credentials.
		>aws configure
	2.At the prompt, configure the following:
		>AWS Access Key ID: Choose the Details dropdown list, and choose Show. Copy and paste the AccessKey value into the terminal window.
		>AWS Secret Access Key: Copy and paste the SecretKey value into the terminal window.
		>Default region name: Enter us-west-2
		>Default output format: Enter json




Task 3: Create an S3 bucket using the AWS CLI
=>	The s3api command creates a new S3 bucket with the AWS credentials in this lab. By default, the S3 bucket is created in the us-east-1 Region. 
	**Tip:** In this lab, you might use the s3api command or the s3 command. s3 commands are built on top of the operations that are found in the s3api commands.

	1.To create a bucket in Amazon S3, you use the aws s3api create-bucket command. When you use this command to create an S3 bucket, you also include the following:
		>Specify --region us-west-2
		>Add --create-bucket-configuration LocationConstraint=us-west-2 to the end of the command.
			aws s3api create-bucket --bucket <twhitlock256> --region us-west-2 --create-bucket-configuration LocationConstraint=us-west-2
	2.If the command is successful, you will get a JSON-formatted response with a Location name-value pair, where the value reflects the bucket name. The following is an example:
			{
			    "Location": "http://twhitlock256.s3.amazonaws.com/"
			}




Task 4: Create a new IAM user that has full access to Amazon S3
=>	1.Using the AWS CLI, create a new IAM user with the command aws iam create-user and username awsS3user: 
		>aws iam create-user --user-name awsS3user
	2.Create a login profile for the new user by using the following command:
		>aws iam create-login-profile --user-name awsS3user --password Training123!
	3.Copy the AWS account number:
		>n the AWS Management Console, choose the account voclabs/user... drop down menu located at the top right of the screen.
		>Copy the 12 digit Account ID number.
		>In the current drop down menu, choose Sign Out.
	4.Log in to the AWS Management Console as the new awsS3user user:
		>In the browser tab where you just signed out of the AWS Management Console, choose Log back in or Sign in to the Console. 
		>In the sign-in screen, choose the radio button IAM user.
		>In the text field, paste or enter the account ID with no dashes.
		>Choose Next.
		>A new login screen with Sign in as IAM user field will show. The account ID will be filled in from the previous screen.
		>For IAM user name, enter awsS3user
		>For Password, enter Training123!
		>Choose Sign In
	5.On the AWS Management Console, in the Search box, enter S3 and choose S3. This option takes you to the Amazon S3 console page.
	6.In the terminal window, to find the AWS managed policy that grants full access to Amazon S3, run the following command:
		>aws iam list-policies --query "Policies[?contains(PolicyName,'S3')]"
	7.To grant the awsS3user user full access to the S3 bucket, replace <policyYouFound> in following command with the appropriate PolicyName from the results, and run the adjusted command:
		>aws iam attach-user-policy --policy-arn arn:aws:iam::aws:policy/<policyYouFound> --user-name awsS3user




Task 5: Extract the files that you need for this lab
=>	1.Back in the SSH terminal, extract the files that you need for this lab by running the following commands:
		>cd ~/sysops-activity-files
		>tar xvzf static-website-v2.tar.gz
		>cd static-website
	2.To confirm that the files were extracted correctly, run the ls command. 
		//You should see a file named index.html and directories named css and images.






Task 6: Upload files to Amazon S3 by using the AWS CLI
=>	1.So that the bucket can function as a website, replace <my-bucket> in the following command with your bucket name, and run the adjusted command. 
		>aws s3 website s3://<my-bucket>/ --index-document index.html
	2.To upload the files to the bucket, replace <my-bucket> in the following command with your bucket name, and run the adjusted command:
		>aws s3 cp /home/ec2-user/sysops-activity-files/static-website/ s3://<my-bucket>/ --recursive --acl public-read
	3.To verify that the files were uploaded, replace <my-bucket> in the following command with your bucket name, and run the adjusted command:
		>aws s3 ls <my-bucket>
	4.On the AWS Management Console, on the Amazon S3 console, choose your bucket name.
	5.Choose the Properties tab. At the bottom of the this tab, note that Static website hosting is Enabled.
	6.To open the URL on a new page, choose the Bucket website endpoint URL that displays.



Task 7: Create a batch file to make updating the website repeatable
=>	1.n the terminal window, to pull up the history of recent commands, run the following command:
		>history
	2.Locate the line where you ran the aws s3 cp command. You will put this line in your new batch file.
	3.To change directories and create an empty file, run the following command in the SSH terminal session:
		>cd ~
		>touch update-website.sh
	4.To open the empty file in the VI editor, run the following command.
		>vi update-website.sh
	5.To enter edit mode in the VI editor, press i
	6.Next, you add the standard first line of a bash file and then add the s3 cp line from your history. To do so, replace <my-bucket> in the following command with your bucket name, and copy and paste the adjusted command into your file:
		>#!/bin/bash
		>aws s3 cp /home/ec2-user/sysops-activity-files/static-website/ s3://<my-bucket>/ --recursive --acl public-read
	7.To write the changes and quit the file, press Esc, enter :wq and then press Enter.
	8.To make the file an executable batch file, run the following command:
		>chmod +x update-website.sh
	9.To open the local copy of the index.html file in a text editor, run the following command:
		>vi sysops-activity-files/static-website/index.html
	10.To enter edit mode in the VI editor, press i and modify the file as follows:
		>Locate the first line that has the HTML code bgcolor="aquamarine" and change this code to bgcolor="gainsboro"
		>Locate the line that has the HTML code bgcolor="orange" and change this code to bgcolor="cornsilk"
		>Locate the second line that has the HTML code bgcolor="aquamarine" and change this code to bgcolor="gainsboro"
		>To write the changes and quit the file, press Esc, enter :wq and then press Enter.
	11.o update the website, run your batch file.
		>./update-website.sh