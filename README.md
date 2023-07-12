# CFN warpper For Boto3 Scripts 
### For Script overview refer to [ https://github.com/Sid-arthh/Boto3 ]


## 1.CheckS3LifeCycleEnable

### AWS Lambda Function for Checking S3 Bucket Lifecycle Policies

This CloudFormation template creates an AWS Lambda function that checks if any S3 bucket has lifecycle policies enabled. The Lambda function is written in Python using the Boto3 library to interact with AWS services.

### Overview

The Lambda function defined in this template performs the following steps:

    Imports the necessary libraries: boto3 and botocore.
    Defines a lambda_handler function that is the entry point of the Lambda function.
    Defines a check_s3_lifecycle_policies function that checks if any S3 bucket has lifecycle policies enabled.
    Creates an S3 client using the boto3.client('s3') method.
    Calls the list_buckets method to retrieve the list of all S3 buckets.
    Iterates through each bucket and retrieves its name.
    Calls the get_bucket_lifecycle_configuration method for each bucket to retrieve its lifecycle configuration.
    Checks if the lifecycle configuration exists and prints the appropriate message.
    Handles exceptions such as NoCredentialsError and ClientError for buckets without lifecycle configurations.

### CloudFormation Template

The CloudFormation template lambda-check-s3-lifecycle.yaml creates the necessary AWS resources for deploying the Lambda function:

    CheckS3LifeCyclePolicy:
        Type: AWS::Lambda::Function
        Description: AWS Lambda function for checking S3 bucket lifecycle policies.
        Properties:
            Code: The code for the Lambda function, provided as a ZIP file inline.
            Handler: The name of the function handler (entry point) within the code.
            Role: The IAM role to execute the Lambda function, defined in the LambdaExecutionRole resource.
            Runtime: The runtime environment for the Lambda function (Python 3.10 in this case).

    LambdaExecutionRole:
        Type: AWS::IAM::Role
        Description: IAM role for executing the Lambda function.
        Properties:
            AssumeRolePolicyDocument: The policy document that specifies the trusted entity for the role (Lambda service in this case).
            Policies: The list of policies attached to the role. In this case, a policy named S3Access is attached.

    S3Access Policy:
        Description: IAM policy allowing access to S3 for listing buckets and retrieving lifecycle configurations.
        Statement:
            Effect: Allow
            Action:
                s3:ListAllMyBuckets: Allows listing all S3 buckets.
                s3:GetLifecycleConfiguration: Allows retrieving the lifecycle configuration of an S3 bucket.
            Resource: "*"

Usage

    Deploy the CloudFormation stack using the provided template lambda-check-s3-lifecycle.yaml.
    Once the stack is created, the Lambda function will be deployed.
    The Lambda function will automatically check each S3 bucket for lifecycle policies and print the results to the Lambda function's logs.

Make sure that the appropriate permissions are granted to the IAM role associated with the Lambda function for accessing S3 buckets and retrieving their lifecycle configurations.

Feel free to modify the Lambda function code or the CloudFormation template as needed for your specific requirements.

Please note that the template provided assumes you have the necessary AWS credentials and permissions to create the resources defined in the template.
