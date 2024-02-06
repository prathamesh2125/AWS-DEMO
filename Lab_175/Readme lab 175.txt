Task 1: Creating a new AMI for Amazon EC2 Auto Scaling
=>	Task 1.1: Connecting to the Command Host instance
		1.On the AWS Management Console, in the Search bar, enter and choose EC2
		2.In the navigation pane, choose Instance
		3.From the list of instances, select the  Command Host instance.
		4.Choose Connect.
		5.On the EC2 Instance Connect tab, choose Connect.

	Task 1.2: Configuring the AWS CLI
		1.To confirm that the Region in which the Command Host instance is running
			curl http://169.254.169.254/latest/dynamic/instance-identity/document | grep region
		2.To update the AWS CLI software with the correct credentials, run the following command:
			aws configure
				AWS Access Key ID: Press Enter.
				AWS Secret Access Key: Press Enter.
				Default region name: us-west-2
				Default output format: Enter json
		3.To access these scripts, enter the following command to navigate to their directory:
			cd /home/ec2-user/


	Task 1.3: Creating a new EC2 Instance
		1.To inspect the UserData.txt script
			more UserData.txt
		2.his script performs a number of initialization tasks, including updating all installed software on the box and installing a small PHP web application that you can use to simulate a high CPU load on the instance. The following lines appear near the end of the script:
			find -wholename /root/.*history -wholename /home/*/.*history -exec rm -f {} \;
			find / -name 'authorized_keys' -exec rm -f {} \;
			rm -rf /var/lib/cloud/data/scripts/*
		These lines erase any history or security information that might have accidentally been left on the instance when the image was taken.

		3.Copy the KEYNAME, AMIID, HTTPACCESS, and SUBNETID values into a text editor document, and then choose X to close the Credentials panel.
		4.In the following script, replace the corresponding text with the values from the previous step.
			aws ec2 run-instances --key-name KEYNAME --instance-type t3.micro --image-id AMIID --user-data file:///home/ec2-				user/UserData.txt --security-group-ids HTTPACCESS --subnet-id SUBNETID --associate-public-ip-address --tag specifications 'ResourceType=instance,Tags=[{Key=Name,Value=WebServer}]' --output text --query 'Instances[*].InstanceId'
		5.Enter your modified script into the terminal window, and run the script.
		6.Copy and paste the InstanceId value into a text editor to use later. 

		7.To use the aws ec2 wait instance-running command to monitor this instance's status, replace NEW-INSTANCE-ID in the following command with the InstanceID value that you copied in the previous step. Run your modified command. 
		aws ec2 wait instance-running --instance-ids NEW-INSTANCE-ID
		8.To obtain the public DNS name, in the following command, replace NEW-INSTANCE-ID with the value that you copied previously, and run your modified command:
			aws ec2 describe-instances --instance-id NEW-INSTANCE-ID --query 'Reservations[0].Instances[0].NetworkInterfaces[0].Association.PublicDnsName'

		9.Copy the output of this command without the quotation marks. 
			The value of this output is referred to as PUBLIC-DNS-ADDRESS in the next steps. 
		10.In the following command, replace PUBLIC-DNS-ADDRESS with the value that you copied in the previous steps, and then run your modified command.
			http://PUBLIC-DNS-ADDRESS/index.php


	Task 1.4: Creating a Custom AMI
		1.To create a new AMI based on this instance, in the following aws ec2 create-image command, replace NEW-INSTANCE-ID with the value that you copied previously, and run your adjusted command:
			aws ec2 create-image --name WebServerAMI --instance-id NEW-INSTANCE-ID








Task 2: Creating an auto scaling environment
=>	Task 2.1: Creating an Application Load Balancer
		1.On the EC2 Management Console, in the left navigation pane, locate the Load Balancing section, and choose Load Balancers.
		2.Choose Create load balancer.
		3.In the Load balancer types section, for Application Load Balancer, choose Create.
		4.On the Create Application Load Balancer page, in the Basic configuration section, configure the following option:
			For Load balancer name, enter WebServerELB
		5.In the Network mapping section, configure the following options:
			For VPC, choose Lab VPC.
			For Mappings, choose both Availability Zones listed.
				For the first Availability Zone, choose Public Subnet 1.
				For the second Availability Zone, choose Public Subnet 2.
		6.In the Security groups section, choose the X for the default security group to remove it.
		7.From the Security groups dropdown list, choose HTTPAccess.
		8.In the Listeners and routing section, choose the Create target group link.
		9.On the Specify group details page, in the Basic configuration section, configure the following options:
			For Choose a target type, choose Instances.
			For Target group name, enter webserver-app
		10.In the Health checks section, for Health check path, enter /index.php
		11.At the bottom of the page, choose Next.
		12.On the Register targets page, choose Create target group.
		13.Return to the Load balancers browser tab, and locate the Listeners and routing section. For Default action, choose  Refresh to the right of the Forward to dropdown list.
		14.From the Forward to dropdown list, choose webserver-app.
		15.At the bottom of the page, choose Create load balancer.
		16.To view the WebServerELB load balancer that you created, choose View load balancer.
		17.To copy the DNS name of the load balancer, use the copy option , and paste the DNS name into a text editor. 



	Task 2.2: Creating a launch template
		1.On the EC2 Management Console, in the left navigation pane, locate the Instances section, and choose Launch Templates.
		2.Choose Create launch template.
		3.On the Create launch template page, in the Launch template name and description section, configure the following options:
			For Launch template name - required, enter web-app-launch-template
			For Template version description, enter A web server for the load test app
			For Auto Scaling guidance, select  Provide guidance to help me set up a template that I can use with EC2 Auto Scaling.
		4.In the Application and OS Images (Amazon Machine Image) - required section, choose the My AMIs tab. 
		5.In the Instance type section, choose the Instance type dropdown list, and choose t3.micro.
		6.In the Key pair (login) section, confirm that the Key pair name dropdown list is set to Don't include in launch template.
		7.In the Network settings section, choose the Security groups dropdown list, and choose HTTPAccess.
		8.Choose Create launch template.


	Task 2.3: Creating an Auto Scaling group
		1.Choose  web-app-launch-template, and then from the Actions  dropdown list, choose Create Auto Scaling group.
		2.On the Choose launch template or configuration page, in the Name section, for Auto Scaling group name, enter Web App Auto Scaling Group
		3.Choose Next.
		4.On the Choose instance launch options page, in the Network section, configure the following options:
			From the VPC dropdown list, choose Lab VPC.
			From the Availability Zones and subnets dropdown list, choose Private Subnet 1 (10.0.2.0/24) and Private Subnet 2 (10.0.4.0/24). 
		5.Choose Next.
		6.On the Configure advanced options – optional page, configure the following options: 
			In the Load balancing – optional section, choose Attach to an existing load balancer.
			In the Attach to an existing load balancer section, configure the following options:
				Choose Choose from your load balancer target groups.
				From the Existing load balancer target groups dropdown list, choose webserver-app | HTTP.
		7.In the Health checks section, under Additional health check types, select  Turn on Elastic Load Balancing health checks.
		8.Choose Next.
		9.On the Configure group size and scaling policies – optional page, configure the following options: 
			In the Group size – optional section, enter the following values: 
				Desired capacity:2
				Minimum capacity: 2
				Maximum capacity: 4
			In the Scaling policies – optional section, configure the following options:
				Choose  Target tracking scaling policy.
				For Metric type, choose Average CPU utilization.
				For Target value, enter 50
		10.Choose Next.
		11.On the Add notifications – optional page, choose Next.
		12.On the Add tags – optional page, choose Add tag and configure the following options:
			For Key, enter Name
			For Value - optional, enter WebApp
		13.Choose Next.
		14.On the Review page, choose Create Auto Scaling group.




Task 3: Verifying the auto scaling configuration
=>	1.In the left navigation pane, choose Instances.
	2.Once the instances have completed initialization, in the left navigation pane in the Load Balancing section, choose Target Groups, and then select  your target group, webserver-app.

	3.On the Targets tab, verify that two instances are being created. Refresh this list until the Health status of these instances changes to healthy.






Task 4: Testing auto scaling configuration
	1.Open a new web browser tab, and paste the DNS name of the load balancer that you copied earlier into the address bar, and press Enter.
	2.On the web page, choose Start Stress.
	3.On the EC2 Management console, in the left navigation pane in the Auto Scaling section, choose Auto Scaling Groups.
	4.Select  Web App Auto Scaling Group.
	5.Choose the Activity tab. 
		After a few minutes, you should see your Auto Scaling group add a new instance






		
		
