# CFN warpper For Boto3 Scripts 
### For Script overview refer to [ https://github.com/Sid-arthh/Boto3 ]


## 1.CheckS3LifeCycleEnable

### AWS Lambda Function for Checking S3 Bucket Lifecycle Policies

This CloudFormation template creates an AWS Lambda function that checks if any S3 bucket has lifecycle policies enabled. The Lambda function is written in Python using the Boto3 library to interact with AWS services.

### Overview

#### The Lambda function defined in this template performs the following steps:

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

#### The CloudFormation template lambda-check-s3-lifecycle.yaml creates the necessary AWS resources for deploying the Lambda function:

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

#### Usage

    Deploy the CloudFormation stack using the provided template lambda-check-s3-lifecycle.yaml.
    Once the stack is created, the Lambda function will be deployed.
    The Lambda function will automatically check each S3 bucket for lifecycle policies and print the results to the Lambda function's logs.

## 2.CreateAmiForEc2ViaTags.yml
### AWS Lambda Function for Creating AMI of EC2 Instance

This CloudFormation template creates an AWS Lambda function that creates an AMI (Amazon Machine Image) of an EC2 instance based on a specified tag. The Lambda function is written in Python using the Boto3 library to interact with AWS services.
Overview

#### The Lambda function defined in this template performs the following steps:

    Imports the necessary libraries: boto3 and datetime.
    Defines a lambda_handler function that is the entry point of the Lambda function.
    Retrieves the EC2 instance based on a specified tag key and value.
    Creates an AMI of the instance with a unique name using the current timestamp.
    Prints the AMI ID if the AMI creation is successful.
    Prints a message if no instances are found based on the specified tag.

### CloudFormation Template

#### The CloudFormation template lambda-create-ec2-ami.yaml creates the necessary AWS resources for deploying the Lambda function:

    CreateAmiForEc2:
        Type: AWS::Lambda::Function
        Description: AWS Lambda function for creating an AMI of an EC2 instance based on a specified tag.
        Properties:
            Timeout: The maximum amount of time (in seconds) that the Lambda function can run.
            Code: The code for the Lambda function, provided as a ZIP file inline.
            Handler: The name of the function handler (entry point) within the code.
            Role: The IAM role to execute the Lambda function, defined in the LambdaExecutionRole resource.
            Runtime: The runtime environment for the Lambda function (Python 3.10 in this case).

    LambdaExecutionRole:
        Type: AWS::IAM::Role
        Description: IAM role for executing the Lambda function.
        Properties:
            AssumeRolePolicyDocument: The policy document that specifies the trusted entity for the role (Lambda service in this case).
            Policies: The list of policies attached to the role. In this case, a policy named EC2Access is attached.

    EC2Access Policy:
        Description: IAM policy allowing access to EC2 resources for creating AMIs, describing instances, and creating tags.
        Statement:
            Sid: EC2Access
            Effect: Allow
            Action:
                ec2:CreateImage: Allows creating an AMI.
                ec2:DescribeInstances: Allows describing EC2 instances.
                ec2:CreateTags: Allows creating tags for EC2 resources.
            Resource: "*"

#### Usage

    Deploy the CloudFormation stack using the provided template lambda-create-ec2-ami.yaml.
    Once the stack is created, the Lambda function will be deployed.
    The Lambda function will automatically create an AMI of the EC2 instance(s) based on the specified tag.
    The AMI ID will be printed to the Lambda function's logs if the AMI creation is successful.
    If no instances are found based on the specified tag, a corresponding message will be printed.

## 3.PrintEc2Services
### AWS Lambda Function to Execute SSM Run Command on EC2 Instance

#### This CloudFormation template creates an AWS Lambda function that executes an SSM Run Command on an EC2 instance. The Lambda function is written in Python using the Boto3 library to interact with AWS services.
Overview

#### The Lambda function defined in this template performs the following steps:

    Imports the necessary library: boto3.
    Defines a lambda_handler function that is the entry point of the Lambda function.
    Specifies the EC2 instance ID and the command to be executed on the instance.
    Creates an SSM client using the boto3.client method.
    Sends the SSM Run Command using the send_command method of the SSM client.
    Retrieves the command ID from the response.
    Waits for the command to be executed on the instance using the get_waiter method of the SSM client.
    Retrieves the command invocation details using the get_command_invocation method of the SSM client.
    Checks the status of the command execution and prints the output if successful, or the error message if failed.

### CloudFormation Template

#### The CloudFormation template PrintEc2Services.yml creates the necessary AWS resources for deploying the Lambda function:

    CreateAmiForEc2:
        Type: AWS::Lambda::Function
        Description: AWS Lambda function to execute SSM Run Command on an EC2 instance.
        Properties:
            Timeout: 10
            Code: The code for the Lambda function written in Python.
            Handler: The name of the Python handler function.
            Role: The IAM role required for the Lambda function. The role is referenced using the LambdaExecutionRole resource.
            Runtime: The runtime environment for the Lambda function (Python 3.10).

    LambdaExecutionRole:
        Type: AWS::IAM::Role
        Description: IAM role for the Lambda function to execute SSM Run Command.
        Properties:
            AssumeRolePolicyDocument: The IAM policy document that defines the permissions for the role.
            Policies: The list of IAM policies attached to the role. In this case, it grants permissions to use SSM Run Command.

#### Make sure to replace the ec2_instance_id with the actual EC2 instance ID and modify the ssm:SendCommand, ssm:GetCommandInvocation, ssm:GetCommand, ssm:GetCommandInvocation, and ssm:ListCommands permissions in the IAM policy to match your requirements.

This CloudFormation template allows you to deploy the AWS Lambda function to execute SSM Run Command on an EC2 instance with the necessary permissions.

Make sure that the appropriate permissions are granted to the IAM role associated with the Lambda function for accessing S3 buckets and retrieving their lifecycle configurations.

Feel free to modify the Lambda function code or the CloudFormation template as needed for your specific requirements.

Please note that the template provided assumes you have the necessary AWS credentials and permissions to create the resources defined in the template.
