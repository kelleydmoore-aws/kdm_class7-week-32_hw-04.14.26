How to create a Lambda function with SNS topic:

Log into AWS
In the Search bar, search for IAM
Click on Roles and then Create Role.
In the Trusted entity type select AWS Service
In the Use case dropdown select Lambda
In Permissions policies, search for AWSLambdaBasicExecutionRole and AmazonSNSFullAccess and select both.
Give your Role a name (eg. lambda-sns-notifications).
Add a Description (eg. Allows Lambda functions to call SNS services on my behalf.)
Click Create role

In the Search bar, search for SNS.
In the topic name section, write the topic's name (eg. demo-topic).
Click Next step.
Leave all of the settings in this next section as default and the click Create topic.
Make note of the ARN of the Topic you just created.  It is needed for later configuration.
Click Create Subscription.
In the Topic ARN dropdown field, select the topic you just created.
In the Protocol section, select Email.
In the Endpoint section, add the email that will receive the SNS notification.
Click Create Subscription.
After this step, make note that in the Status section, you will see a Pending Confirmation message. This means the Subscription needs to be confirmed.
Check your email for the confirmation from AWS and click the link.  Be mindful, the email sometimes is filtered to Junk Mail. 
Refresh the AWS SNS page and the Pending Confirmation status message should change to a green check that says Confirmed. 
Go back to the Lambda page and follow the instructions to create a Lambda function. 
Once created, click on the Configuration tab and then Environment variables.
Click Edit and then add SNS_TOPIC_ARN as the Key and the SNS Topic ARN from earlier as the value.
Click Save.
Next click the Permissions tab and then Edit.
In the Execution role section dropdown, select the IAM role you created (eg. lambda-sns-notifications).
In the Code Source section, enter the following being mindful to enter the information for the resources you created previously: 

import json
import boto3
import os
from datetime import datetime, timezone

sns = boto3.client('sns')
TOPIC_ARN = os.environ['SNS_TOPIC_ARN'] 

def lambda_handler(event, context):
	now = datetime.now(timezone.utc)
	timestamp = now.strftime('%Y-%m-%d %H:%M:%S UTC')

	message = f'Lambda invoked at {timestamp}'
	subject = f'Timestamp Notification - {timestamp}'
	
	sns.publish(
		TopicArn=TOPIC_ARN,
		Message=message,
		Subject=subject
	)

	return {
		'statusCode': 200,
		'body': json.dumps({'message': message})
	}
	
Click Deploy to call the Lambda function. 
If you open your Function URL, you should see a message saying the function was successful.
Finally, go into CloudWatch logs and verify that the Lambda function ran successfully.

Alternative code source:

import json
import boto3
import os
from datetime import datetime, timezone

sns = boto3.client('sns')
TOPIC_ARN = os.environ['SNS_TOPIC_ARN'] 

def lambda_handler(event, context):
	now = datetime.now(timezone.utc)
	timestamp = now.strftime('%Y-%m-%d %H:%M:%S UTC')

	message = f'Lambda invoked at {timestamp}' # Edit this line to change the email body. (eg. message = f'The custom Lambda lab ran successfully at {timestamp}.')
	subject = f'Timestamp Notification - {timestamp}' # Edit this line to change the email subject line. (eg. subject = f'Custom SNS Notification - {timestamp}')

	print(message) # Prints the message from this code section. Used for troubleshooting.
	
	publishResult = sns.publish(
		TopicArn=TOPIC_ARN,
		Message=message,
		Subject=subject
	)

	print(f"publishResult {publishResult}") # More robust messaging.

	return {
		'statusCode': 200,
		'body': json.dumps({'message': message}) # Edit this line to change the message printed in the browser. (eg. 'body': json.dumps('Success! The web page has triggered the Lambda function.'))
	}
	
	
Teardown
-	Delete Lambda function
-	Delete API Gateway
-	Delete Cloudwatch log groups
-	Delete IAM policy role for Lambda

arn:aws:sns:us-east-2:321528232261:kdm-lambda-sns-topic

