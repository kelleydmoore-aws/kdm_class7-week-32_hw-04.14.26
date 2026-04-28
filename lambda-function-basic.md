## How to create a basic Lambda function:

-	Log into the AWS console.
-	In the Search field enter Lambda and press Enter.
-	Click Create function.
-	Name the function.
-	For Runtime select Python.
-	Click Create Function.
-	This creates the function but does not set it up.
-	In the Code Source section, add the following:

```
import json

def lambda_handler(event, context):
	# TODO implement
	print('Hello World from Lambda!')
	return {
		'statusCode': 200,
		'body': json.dumps('Hello from the Lambda function!!')
	}
```

-	Next click Deploy.  It only gives minimal message for success.
-	Once created Click the Configuration tab and then Function URL.
-	Click Create Function URL.
-	For Auth type select None then click Save.
-	For the new Function URL that is created, click on the link that was created to open it in a new tab.
-	The function should have ran and your message printed in the new tab. 
-	Click on the Monitor tab then click on View CloudWatch logs
-	This should send you to the CloudWatch console under Log Management.
-	A Log stream should have been generated for the Lambda function you just invoked.
-	Click this Log stream and it will take you to Log events which will display the logs for the time, date, and action perform after the invoke.  
-	In this example, it will be the message you set the Lambda up to display.



