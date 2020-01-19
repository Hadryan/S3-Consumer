# S3-Consumer
Copy files from one S3 bucket to another using S3 - SQS - Lambda - SNS


## Objective:
CloudFormation template 1:
  Creates an S3 bucket (to store the zipped lambda deployment package)
	Makes the name of the bucket available as CLoudFormation export

 

CloudFormation template 2
	Creates another two S3 buckets
		To act as a Source
		To act as a Destination
  Creates an SQS queue
  Creates an S3 event notification
    On PUT events in the Source Bucket
      Target is SQS queue above
      Creates an SNS topic
  Creates a Lambda execution role with appropriate access to
    Buckets
		SQS queue
		SNS topic
  Creates a Lambda function
	  Written in any supported language  ( we use python3 with boto3 everywhere, but any other language and AWS SDK combo is acceptable)
		Code is zipped, uploaded to S3, location specified using a CloudFormation import from the other stack above
		Being triggered by SQS above, used for dequeuing and message processing
		Copies file from source to destination bucket upon triggering
		Reports completion of copy to an SNS topic
