Task 1: Connect to the Red Hat EC2 instance by using SSH
=>	1. Open putty.exe
	2. Choose Connection. For Seconds between keepalives, enter 30
	3. Configure your PuTTY session:
		Choose Session.
		For the Host Name (or IP address), enter the PublicIP address that you copied from the previous steps.
		>In PuTTY in the Connection list, choose SSH to expand it.
  		>Choose Auth, but don't expand it.
		>Choose Browse.
		>Browse to and select the labuser.ppk file that you downloaded.
		>To choose the file, choose Open.
		>Choose Open again.
	4. In the PuTTY Security Alert window, choose Accept to trust and connect to the host.
	5. When prompted with login as, enter ec2-user and press Enter




Task 2: Install the AWS CLI on a Red Hat Linux instance
=>	1. To write the downloaded file to the current directory, run the following curl command with the -o option:
		curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
	2. To unzip the installer, run the following unzip command with the -u option.
		unzip -u awscliv2.zip
	3. To run the install program, run the following command.
		sudo ./aws/install
	4. To confirm the installation, run the following command:
		aws --version
		aws help



Task 3: Observe IAM configuration details in the AWS Management Console
=>	In this task, you observe the IAM configuration details for the EC2 instance in the AWS Management Console. 
	1. In the AWS Management Console, in the Search box, enter IAM and choose IAM
	2. choose Users, and then choose awsstudent. 
	3. You are now in the Permissions tab. Next to lab_policy, choose the arrow icon, and then choose the {} JSON button.
	4. Choose the Security credentials tab. In the Access keys section, locate the awsstudent user's access key ID. 


Task 4: Configure the AWS CLI to connect to your AWS Account
=>	In the SSH session terminal window, run the configure command for the AWS CLI:
	>aws configure
	>	AWS Access Key ID: Choose the Details dropdown list, and choose Show. Copy and paste the AccessKey value into the terminal window.
		AWS Secret Access Key: Copy and paste the SecretKey value into the terminal window.
		Default region name: Enter us-west-2
		Default output format: Enter json


Task 5: Observe IAM configuration details by using the AWS CLI
=>	In the terminal window, test the IAM configuration by running the following command:
	>	aws iam list-users






Activity 1 challenge : Use the AWS CLI Command Reference documentation and AWS CLI to download the lab_policy document in a JSON-formatted IAM policy document. This is the same document that is in the AWS Management Console.

=>	aws iam list-policies --scope Local 
	aws iam get-policy-version --policy-arn arn:aws:iam::038946776283:policy/lab_policy --version-id v1 > lab_policy.json 