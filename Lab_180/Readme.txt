Task 1: Creating a VPC
	1.Choose Create VPC and configure the following options:
	2.Resources to create: Choose VPC only.
	3.Name tag: Enter Lab VPC.
	4.IPv4 CIDR block: Choose IPv4 CIDR manual input.
	5.IPv4 CIDR: Enter 10.0.0.0/16.
	6.IPv6 CIDR block: Choose No IPv6 CIDR block.
	7.Tenancy: Choose Default.
	8.Tags: Leave the suggested tags as is.
	9.Choose Create VPC. 
	10.Choose Actions, and choose Edit VPC settings.
	11.In the DNS settings section, select Enable DNS hostnames.
	12.Choose Save.


Task 2: Creating subnets
	Task 2.1: Creating a public subnet
		1.In the left navigation pane, for Virtual private cloud, choose Subnets.
		2.Choose Create subnet and configure the following options:
			VPC ID: Choose Lab VPC.
			Subnet name: Enter Public Subnet.
			Availability Zone: Choose the first Availability Zone in the list. Do not choose No preference.
			IPv4 CIDR block: Enter 10.0.0.0/24.
		3.Choose Create subnet
		4.Select Public Subnet.
		5.Choose Actions, and then choose Edit subnet settings.
		6.In the Auto-assign IP settings section, select Enable auto-assign public IPv4 address.
		7.Choose Save.

	Task 2.2: Creating a private subnet
		1.Task 2.2: Creating a private subnet
			VPC ID: Choose Lab VPC.
			Subnet name: Enter Private Subnet.
			Availability Zone: Choose the first Availability Zone on the list. Do not choose No preference.
			IPv4 CIDR block: Enter 10.0.2.0/23.
		2.Choose Create subnet.


Task 3: Creating an internet gateway
	1.In the left navigation pane, for Virtual private cloud, choose Internet gateways.
	2.Choose Create internet gateway, and then for Name tag, enter Lab IGW.
	3.Choose Create internet gateway.
	4.Choose Actions, then choose Attach to a VPC.


Task 4: Configuring route tables
	1.In the left navigation pane, for Virtual private cloud, choose Route tables.
	2.Several route tables are listed.
	3.Select the route table that includes Lab VPC in the VPC column.
	4.In the Name column, choose the edit icon, enter Private Route Table for Edit Name, and then choose Save.
	5.Choose the Routes tab.
	6.Choose Create route table and configure the following options:
		Name - optional: Enter Public Route Table.
		VPC: Choose Lab VPC.
	7.Choose Create route table.
	8.After the route table is created, in the Routes tab, choose Edit routes.
	9.Choose Add route and then configure the following options:
		Destination: Enter 0.0.0.0/0.
		Target: Choose Internet Gateway, and then choose Lab IGW from the list.
	10.Choose Save changes.
	11.Choose the Subnet associations tab.
	12.Choose Edit subnet associations.
	13.Select Public Subnet.
	14.Choose Save associations.


Task 5: Launching a bastion server in the public subnet
	1.Task 5: Launching a bastion server in the public subnet
	2.In the left navigation pane, choose Instances.
	3.Choose Launch instances and configure the following options:
		In the Name and tags section, enter Bastion Server.
		In the Application and OS Images (Amazon Machine Image) section, configure the following options:
			Quick Start: Choose Amazon Linux.
			Amazon Machine Image (AMI): Choose Amazon Linux 2023 AMI.
		In the Instance type section, choose t3.micro.
		In the Key pair (login) section, choose Proceed without a key pair (Not recommended).
	4.In the Network settings section, choose Edit and configure the following options:
		VPC - required: Choose Lab VPC.
		Subnet: Choose Public Subnet.
		Auto-assign public IP: Choose Enable.
		Firewall (security groups): Choose Create security group.
			Security group name - required: Enter Bastion Security Group.
			Description - required: Enter Allow SSH.
		Inbound security groups rules:
			Type: Choose ssh.
			Source type: Choose Anywhere.
		Choose Launch instance.

Task 6: Creating a NAT gateway
	1.On the AWS Management Console, in the Search bar, enter NAT gateways, choose the Features list, and choose NAT gateways.
	2.Choose Create NAT gateway and configure the following options:
		Name: Enter Lab NAT gateway. 
		Subnet: From the dropdown list, choose Public Subnet.
	3.Choose Allocate Elastic IP.
	4.Choose Create a NAT gateway.
	5.In the left navigation pane, choose Route tables, and then select Private Route Table.
	6.Choose the Routes tab.
	7.Choose Edit routes.
	8.Choose Add route and configure the following options:
		Destination: Enter 0.0.0.0/0.
		Target: Choose NAT Gateway, and then choose nat- from the list.
	9.Choose Save changes.



Optional challenge: Testing the private subnet
(Launching an instance in the private subnet)
	1.Follow the instructions that you used to launch the bastion server, and configure the following options:
		In the Name and tags section, enter Private Instance.
		In the Application and OS Images (Amazon Machine Image) section, configure the following options:
			Quick Start: Choose Amazon Linux.
			Amazon Machine Image (AMI): Choose Amazon Linux 2023 AMI.
		In the Instance type section, choose t3.micro.
		In the Key pair (login) section, choose Proceed without a key pair (Not recommended).
		In the Network settings section, choose Edit and configure the following options:
			VPC - required: Choose Lab VPC.
			Subnet: Choose Private Subnet (not the public subnet).
			Firewall (security groups): Choose Create security group.
				Security group name - required: Enter Private Instance SG.
				Description - required: Enter Allow SSH from Bastion.
		Inbound security groups rules:
			Type: Choose ssh.
			Source type: Choose Custom.
			Source: Choose 10.0.0.0/16.
	2.Expand the Advanced Details section, and for User data - optional, paste the following script:
			#!/bin/bash
			# Turn on password authentication for lab challenge
			echo 'lab-password' | passwd ec2-user --stdin
			sed -i 's|[#]*PasswordAuthentication no|PasswordAuthentication yes|g' /etc/ssh/sshd_config
			systemctl restart sshd.service
	3.Choose Launch instance.
	4.To display the launched instance, choose View all instances.


Logging in to the bastion server
	1.On the AWS Management Console, in the Search bar, enter and choose EC2 to open the EC2 Management Console.
	2.In the navigation pane, choose Instances.
	3.From the list of instances, select the Bastion Server instance.
	4.Choose Connect.
	5.On the EC2 Instance Connect tab, choose Connect.

Logging in to the private instance
	1.In the Amazon EC2 console, choose Instances, and select Private Instance (and clear any other instances).
	2.Copy the Private IPv4 addresses (shown in the lower half of the page) to your clipboard.
	3.Return to the terminal window, and run the following command. In the command, replace PRIVATE-IP with the IP address that you just copied to your clipboard:
		ssh PRIVATE-IP
	4.If you are prompted with the message "Are you sure you want to continue connecting", enter yes
	5.When prompted for a password, enter lab-password.



Testing the NAT gateway
	1.Run the following command:
		ping -c 3 amazon.com
	