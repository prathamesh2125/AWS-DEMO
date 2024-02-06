Task 1: Creating an AMI for auto scaling
=>	1.Open the Amazon EC2 Management Console
	2.Choose the  Web Server 1 instance, which should appear in a  Running state.
	3.From the Actions  dropdown list, choose Image and templates > Create image, and then configure the following options:
		>For Image name, enter Web Server AMI
		>For Image description - optional, enter Lab AMI for Web Server
	4.Choose Create image



Task 2: Creating a load balancer
=>	1.In the left navigation pane, locate the Load Balancing section, and choose Load Balancers.
	2.In the Load balancer types section, select Application Load Balancer
	3.Create Application Load Balancer page, in the Basic configuration section, configure the following option:
		>For the Load balancer name, enter LabELB
	4.In the Network mapping section, configure the following options:
		>For VPC, choose Lab VPC.
		>For Mappings, choose both Availability Zones listed.
		>For the first Availability Zone, choose Public Subnet 1.
		>For the second Availability Zone, choose Public Subnet 2.
	5.From the Security groups dropdown list, choose Web Security Group (already created)
	6.In the Listeners and routing section, choose the Create target group link.
		*******Note: This link opens a new browser tab with the Create target group configuration options.*******
	7.On the new Target groups browser tab, in the Basic configuration section, configure the following:
		>For Choose a target type, choose Instances.
		>For Target group name, enter lab-target-group
	8.At the bottom of the page, choose Next.
	9.On the Register targets page, choose Create target group.
	10.Return to the Load balancers browser tab. In the Listeners and routing section, choose  Refresh to the right of the Forward to dropdown list for Default action.
	11.From the Forward to dropdown list, choose lab-target-group.
	12.At the bottom of the page, choose Create load balancer.
	13.To view the LabELB load balancer that you created, choose View load balancer.




Task 3: Creating a launch template
	1.At the top of the AWS Management Console, in the search bar, enter and choose EC2
	2.In the left navigation pane, locate the Instances section, and choose Launch Templates.
	3.Choose Create launch template.
	4.Launch template name and description section, configure the following options:	
		>For Launch template name - required, enter lab-app-launch-template
		>For Template version description, enter A web server for the load test app
		>For Auto Scaling guidance, choose  Provide guidance to help me set up a template that I can use with EC2 Auto Scaling.
	5.In the Application and OS Images (Amazon Machine Image) - required section, choose the My AMIs tab. Notice that Web Server AMI is already chosen.
	6.In the Instance type section, choose the Instance type dropdown list, and choose t3.micro.
	7.In the Key pair (login) section, confirm that the Key pair name dropdown list is set to Don't include in launch template.
	8.In the Network settings section, choose the Security groups dropdown list, and choose Web Security Group.
	9.Choose Create launch template.
	10.Choose View launch templates.



Task 4: Creating an Auto Scaling group
=>	1.Choose  lab-app-launch-template, and then from the Actions  dropdown list, choose Create Auto Scaling group
	2.On the Choose launch template or configuration page, in the Name section, for Auto Scaling group name, enter Lab Auto Scaling Group
	3.On the Choose instance launch options page, in the Network section, configure the following options:
		>From the VPC dropdown list, choose Lab VPC.
		>From the Availability Zones and subnets dropdown list, choose Private Subnet 1 (10.0.1.0/24) and Private Subnet 2 (10.0.3.0/24). 
	4.On the Configure advanced options – optional page, configure the following options: 
		>In the Load balancing – optional section, choose Attach to an existing load balancer.
		>In the Attach to an existing load balancer section, configure the following options:
		>Choose Choose from your load balancer target groups.
		>From the Existing load balancer target groups dropdown list, choose lab-target-group | HTTP.
		>In the Health checks – optional section, for Health check type, choose  ELB.
	5.On the Configure group size and scaling policies – optional page, configure the following options: 
		>In the Group size – optional section, enter the following values: 
			>Desired capacity:2
			>Minimum capacity: 2
			>Maximum capacity: 4
		>In the Scaling policies – optional section, configure the following options:
			>Choose  Target tracking scaling policy.
			>For Metric type, choose Average CPU utilization.
			>Change the Target value to 50
	6.On the Add notifications – optional page, choose Next.
	7.On the Add tags – optional page, choose Add tag and configure the following options:
		>Key: Enter Name
		>Value - optional: Enter Lab Instance
	8.Choose Create Auto Scaling group.



Task 5: Verifying that load balancing is working
=>	1.In the left navigation pane, locate the Instances section, and choose Instances.
	2.In the left navigation pane, in the Load Balancing section, choose Target Groups.
	3.Choose lab-target-group.
		In the Registered targets section, two Lab Instance targets should be listed for this target group.
	4.Wait until the Health status of both instances changes to healthy. To check for updates, choose refresh .
	5.Open a new web browser tab, paste the DNS name that you copied before, and press Enter.




Task 6: Testing auto scaling
=>	1.Return to the AWS Management Console, but keep the Load Test application tab open. You return to this tab soon.
	2.In the AWS Management Console, in the search bar, enter and choose CloudWatch
	3.In the left navigation pane, in the Alarms section, choose All alarms.
		>Two alarms are displayed. The Auto Scaling group automatically created these two alarms. These alarms automatically keep the average CPU load close to 50 percent while also staying within the limitation of having 2–4 instances.
	4.Choose the alarm that has AlarmHigh in its name. This alarm should have a State of OK
	5.Return to the browser tab with the Load Test application.
	6.Next to the AWS logo, choose Load Test.
	7.Return to browser tab with the CloudWatch Management Console.
		>Return to browser tab with the CloudWatch Management Console.
		>In less than 5 minutes, the AlarmLow alarm status should change to OK, and the AlarmHigh alarm status should change to In alarm.
	8.Wait until the AlarmHigh alarm enters the In alarm state.
	9.In the AWS Management Console, in the search bar, enter and choose EC2 
	10.In the left navigation pane, locate the Instances section, and choose Instances.
		>More than two instances named Lab Instance should now be running. Auto scaling created the new instances




Task 7: Terminating the Web Server 1 instance
	1.Choose  Web Server 1, and ensure that it is the only instance selected.
	2.From the Instance state  dropdown menu, choose Terminate instance.
	3.Choose Terminate.



