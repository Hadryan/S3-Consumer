# S3-Consumer
Copy files from one S3 bucket to another using S3 - SQS - Lambda - SNS

## Objective:
The objective is to have a flat file automatically copied from one S3 bucket to another using S3, SQS, Lambda and SNS.
 
## CloudFormation Template 1 (CFTemplate1.yaml):
- Creates an S3 bucket (to store the zipped lambda deployment package).
- Makes the name of the bucket available as CLoudFormation export.

Parameters:
- BucketName: (The name of the S3 bucket which will store the Lambda deployment package.)
 
## CloudFormation Template 2 (CFTemplate1.yaml):
- Creates another two S3 buckets
  - To act as a Source
  - To act as a Destination
- Creates an SQS queue
- Creates an S3 event notification
  - On PUT events in the Source Bucket
  - Target is SQS queue above
- Creates an SNS topic
- Creates a Lambda execution role with appropriate access to
  - Buckets
  - SQS queue
  - SNS topic
- Creates a Lambda function
  - Written in python3 with boto3 
  - Code is zipped, uploaded to S3, location specified using a CloudFormation import from the other stack above
  - Being triggered by SQS above, used for dequeuing and message processing
  - Copies file from source to destination bucket upon triggering
  - Reports completion of copy to an SNS topic

Parameters:

- AppName
  - Name of the application to be created.
  - Type String
  - Default ta-s3-consumer
- Environment
  - Type String
  - AllowedValues
    - dev
    - test
    - prod
  - Default dev
- SourceBucketSuffix
  - Description The unique suffix of the S3 source bucket which files will be copied from.
  - Type String
- TargetBucketSuffix
  - Description The unique suffix of the S3 destination bucket which files will be copied to.
  - Type String
- ParentStackName
  - Description Name of the parent stack which contains the Lambda function.
  - Type String
- LambdaFileName
  - Description The filename of the zipped lambda file.
  - Type String
  - Default lambda_function.zip
- LambdaHandler
  - Description The Lambda Python Handler.
  - Type String
  - Default lambda_function.lambda_handler
