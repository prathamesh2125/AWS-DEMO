Task 1: Generate inventory lists for managed instances
=>	1.In the AWS Management Console, in the  search box, enter Systems Manager
	2.In the left navigation pane, for Node Management, choose Fleet Manager
	3.Choose the Account management dropdown list, and choose Set up inventory.
	4.To create an association that collects information about software and settings for your managed instance, choose the following options:
		>In the Provide inventory details section, for Name, enter Inventory-Association
		>In the Targets section, choose the following options:
			>For Specify targets by, choose Manually selecting instances.
			>Select the row for Managed Instance.
	5.Choose Setup Inventory
	6.Choose the Node ID link, which directs you to the Node overview.
	7.Choose the Inventory tab
		>This tab lists all of the applications in the instance. Take a moment to review the installed applications and other options in the Inventory type dropdown list.





Task 2: Install a Custom Application using Run Command
=>	1.In the upper-left corner, expand the menu icon. For Node Management, choose Run Command.
	2.Choose Run command
	3.Choose the search icon  in the box, and a dropdown box appears
		>Owner
		>Owned by me
	4.If the document is not already selected, select the button for the document.
	5.The following information appears for this document:
		>Description Install Dashboard App
		>Document version: 1 (Default) 
		Leave the Document version option set to this default. 
	6.For Target selection, select Choose instances manually.
	7.In the Instances section, select Managed Instance.
	8.In the Output options section, clear Enable an S3 bucket.
	9.Expand the AWS command line interface command section.
	10.Choose Run
	11.After 1–2 minutes, the Overall status should change to Success. If it doesn't, choose the  refresh icon to update the status.
	12.In the console, choose the following options: 
		>Choose the Details dropdown list at the top of these instructions.
		>Choose Show
		>Copy the ServerIP value. (This value is the public IP address.)
	13.pen a new web browser tab, paste the IP address that you copied, and press Enter.



Task 3: Use Parameter Store to manage application settings
	1.Keep the Widget Manufacturing Dashboard browser tab open, and return to the AWS Systems Manager tab.
	2.In the left navigation pane, for Application Management, choose Parameter Store.
	3.Choose Create parameter
	4.Choose the following options:
		For Name:, enter /dashboard/show-beta-features
		For Description: , enter Display beta features
		For Tier:, leave the default option. 
		For Type:, leave the default option. 
		For Value:, enter True
	5.Choose Create parameter
		A banner with the message "Create parameter request succeeded" appears at the top of the page.  	
	6.Return to the web browser tab that displays the application, and refresh the web page.
	




Task 4: Use Session Manager to access instances
	1.In the left navigation pane, for Node Management, choose Session Manager.
	2.Choose Start session
	3.Select Managed Instance.
	4.Choose Start session
	5.To activate the cursor, choose anywhere in the session window.
	6.Run the following command in the session window:
		ls /var/www/html
	7.Run the following command in the session window:
		# Get region
		AZ=`curl -s http://169.254.169.254/latest/meta-data/placement/availability-zone`
		export AWS_DEFAULT_REGION=${AZ::-1}

		# List information about EC2 instances
		aws ec2 describe-instances

