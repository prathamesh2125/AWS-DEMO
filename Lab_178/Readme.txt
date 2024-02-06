AWS Lambda Lab README

Task 1: Observing the IAM role settings

Task 1.1: Observing the salesAnalysisReport IAM role settings

1. Navigate to AWS Management Console.
2. Go to Services > Security, Identity, & Compliance > IAM.
3. In the navigation pane, select Roles.
4. Search for "sales" and choose the salesAnalysisReportRole hyperlink.
5. Select the Trust relationships tab and verify lambda.amazonaws.com is listed.
6. Switch to the Permissions tab to observe the assigned policies.

   AmazonSNSFullAccess: Provides full access to Amazon SNS resources.
   AmazonSSMReadOnlyAccess: Provides read-only access to Systems Manager resources.
   AWSLambdaBasicRunRole: Provides write permissions to CloudWatch logs.
   AWSLambdaRole: Allows invoking another Lambda function.

Task 1.2: Observing the salesAnalysisReportDERole IAM role settings

1. Navigate to Roles again.
2. Search for "sales" and choose the salesAnalysisReportDERole hyperlink.
3. Select the Trust relationships tab and verify lambda.amazonaws.com is listed.
4. Go to the Permissions tab and observe the permissions granted.

   AWSLambdaBasicRunRole: Provides write permissions to CloudWatch logs.
   AWSLambdaVPCAccessRunRole: Provides permissions to manage elastic network interfaces.

Task 2: Creating a Lambda layer and a data extractor Lambda function

Task 2.1: Creating a Lambda Layer

1. Navigate to AWS Management Console.
2. Go to Services > Compute > Lambda.
3. Choose Layers and then Create layer.
4. Configure settings and upload the required .zip file.
5. Choose Create.

Task 2.2: Creating a data extractor Lambda function

1. Navigate to Functions.
2. Choose Create function and configure options.
3. Specify existing role and choose Create function.

Task 2.3: Adding the Lambda layer to the function

1. In the Function overview panel, select Layers.
2. Choose Add a layer and configure options.

Task 2.4: Importing the code for the data extractor Lambda function

1. Go to the Lambda function page.
2. In the Runtime settings panel, choose Edit.
3. Configure the code source and upload the required .zip file.

Task 2.5: Configuring network settings for the function

1. Choose the Configuration tab, and then VPC.
2. Choose Edit, and configure VPC settings.
3. Save the settings.

Task 3: Testing the data extractor Lambda function

Task 3.1: Launching a test of the Lambda function

1. Open AWS Management Console and navigate to Systems Manager.
2. Copy database connection parameters from Parameter Store.
3. Return to Lambda Management Console and test the function.

Task 3.2: Troubleshooting the data extractor Lambda function

1. Analyze the error message to determine the cause.

Task 3.3: Analyzing and correcting the Lambda function

1. Review the database connection settings and security group rules.

Task 3.4: Placing an order and testing again

1. Access the café website and place orders to populate the database.
2. Test the function again to observe changes in the report.

Task 4: Configuring notifications

Task 4.1: Creating an SNS topic

1. Navigate to Simple Notification Service.
2. Choose Topics and create a new topic.
3. Copy the ARN value for future use.

Task 4.2: Subscribing to the SNS topic

1. Create a subscription for the topic using an email address.
2. Confirm the subscription.

Task 5: Creating the salesAnalysisReport Lambda function

Task 5.1: Connecting to the CLI Host instance

1. Use EC2 Instance Connect to log in to the CLI Host instance.

Task 5.2: Configuring the AWS CLI

1. Run "aws configure" and enter necessary credentials.

Task 5.3: Creating the salesAnalysisReport Lambda function using the AWS CLI

1. Retrieve the ARN of the salesAnalysisReportRole IAM role.
2. Use AWS CLI to create the Lambda function.

Task 5.4: Configuring the salesAnalysisReport Lambda function

1. Define environment variables, including the SNS topic ARN.

Task 5.5: Testing the salesAnalysisReport Lambda function

1. Test the function using the Lambda management console.
2. Verify the execution result and check email inbox for notifications.

Task 5.6: Adding a trigger to the salesAnalysisReport Lambda function

1. Configure a CloudWatch Events event as a trigger.
2. Schedule the trigger for daily execution.

