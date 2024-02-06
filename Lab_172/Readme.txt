*********************************Here are the steps to achieve the challenge:*********************************


Part 1: Launch EC2 Instance
	1.Login to AWS Management Console:
		>Go to the AWS Management Console.
		>Log in with your AWS account.
	2.Launch Instance:
		>Navigate to the EC2 service.
		>Click on "Launch Instance."
		>Choose "Amazon Linux 2 AMI" as the Amazon Machine Image (AMI).
		>Select a T3 instance type with a size that is smaller than medium.
		>Configure the instance details (VPC, subnet, auto-assign public IP).
		>Add storage (use General Purpose SSD (gp2) volume type for the root volume).
		>Configure security group settings to allow SSH and HTTP traffic.
	3.User Data:
		>In the "Configure Instance Details" step, scroll down to the "Advanced Details" section.
		>In the "User data" field, enter the following script:
			#!/bin/bash
			yum update -y
			yum install -y httpd
			systemctl start httpd
			systemctl enable httpd
			chmod -R 755 /var/www/html
		>This script installs and starts the httpd service and gives write permissions to users in the web server's document root directory.

	4.Review and Launch:
		>Review your configurations, then click "Launch."
		>Choose an existing key pair or create a new key pair to connect to the instance.





Part 2: Connect to the Instance
	1.Connect via EC2 Instance Connect:
		>In the EC2 console, select your instance.
		>Click "Connect" and choose "Connect using EC2 Instance Connect."
		>Follow the on-screen instructions to connect via SSH.
	2.Verify httpd Installation:
		>Once connected, run the following command to check the system log:
			sudo cat /var/log/cloud-init-output.log
		>Ensure that the httpd service was successfully installed.





Part 3: Test the Web Server
	1.Create HTML File:
		<!DOCTYPE html>
		<html>
			<body>
				<h1>Prathameh's re/Start Project Work</h1>
				<p>EC2 Instance Challenge Lab</p>
			</body>
		</html>
	
	2.Copy HTML File to EC2 Instance:
		>Use the scp command to copy the file to the /var/www/html directory on the EC2 instance. Execute the following command from your local machine:
		scp -i /path/to/your/keypair.pem projects.html ec2-user@your-instance-public-ip:/var/www/html/
	3.Access the Web Page:
		>Open a web browser and navigate to your instance's public IP address.
		>You should see the sample webpage.
