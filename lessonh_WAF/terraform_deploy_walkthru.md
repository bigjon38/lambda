Lab 1B – Terraform Deployment Walkthrough
Objective

In this lab, you will deploy the same AWS infrastructure that you previously created using the AWS Console. The purpose is to demonstrate how Infrastructure as Code (IaC) can provision an entire serverless application consistently and repeatedly.

Note:

        Do not skip verification steps. A successful terraform apply only confirms that AWS accepted the resource definitions. It does not prove the application works. The engineering task is complete only after you verify that API Gateway, WAF, Lambda, CloudWatch, DynamoDB, EventBridge, and Bedrock are all functioning together as an integrated system.


By the end of this lab, you will have deployed:

Amazon API Gateway (REST API)
AWS Lambda
AWS WAF
CloudWatch Log Groups
DynamoDB
IAM Roles and Policies
Amazon EventBridge
Amazon Bedrock configuration
Prerequisites

Before beginning, verify:

Terraform is installed.
AWS CLI is configured (aws configure).
You have an AWS account with permissions to create Lambda, API Gateway, WAF, CloudWatch, DynamoDB, IAM, EventBridge, and Bedrock resources.
Bedrock model access has been enabled for Anthropic Claude 3 Haiku in your AWS Region. Bedrock model access is enabled separately from Terraform.

Step 1 — Change to the Terraform Directory

cd lessonh_WAF/terraform

Expected files:

    provider.tf
    variables.tf
    apigateway.tf
    waf.tf
    cloudwatch.tf
    dynamodb.tf
    iam.tf
    bedrock.tf
    eventbridge.tf
    lambda_waf.tf
    outputs.tf
    terraform.tfvars

Step 2 — Initialize Terraform

    terraform init

    Terraform has been successfully initialized.


Step 3 — Validate the Configuration

    terraform validate

    Success! The configuration is valid.

Step 4 — Review the Deployment Plan

    terraform plan

Review the resources Terraform intends to create.

You should see resources similar to:

        IAM Role
        IAM Policy
        Lambda Function
        API Gateway REST API
        WAF Web ACL
        CloudWatch Log Groups
        DynamoDB Table
        EventBridge Rule
        EventBridge Target

Do not proceed until the plan completes without errors.

Step 5 — Deploy the Infrastructure

    terraform apply

    But did you pray though?

Step 6 — Verify the Deployment

Lambda
    waf-bedrock-analyzer
    unused-token-detector

API Gateway
    REST API
    /analyze resource
    POST method
    
WAF
    Web ACL
    Attached to the API Gateway stage

CloudWatch

Verify both log groups exist:

    /aws/lambda/waf-bedrock-analyzer
    aws-waf-logs-api
    
DynamoDB

Verify the table: waf-events

EventBridge

Verify: unused-token-check

Step 7 — Generate a WAF Event

Call the protected API using a request that should trigger a WAF rule.

For example:

        curl "https://<api-id>.execute-api.<region>.amazonaws.com/prod/analyze?name=<script>alert(1)</script>"

A managed rule set should inspect the request and, depending on your configuration, may block it and write a WAF log entry.

Step 8 — Verify CloudWatch

Confirm:

        WAF logs were written.
        Lambda execution logs exist.
        Bedrock produced an AI incident summary.
        The Lambda successfully stored a record in DynamoDB.

Step 9 — Verify DynamoDB

Open the table: waf-events

Confirm that a new record was inserted containing information such as:

        Source IP
        Country
        HTTP Method
        URI
        WAF Action
        Rule Name
        your N count with Lizzo

Step 10 — Verify EventBridge

Confirm the scheduled rule:

        unused-token-check

is enabled.

Verify:

        Schedule: rate(5 minutes)
        Target: unused-token-detector

Troubleshooting Checklist

If Terraform fails:

        Run terraform validate.
        Verify your AWS credentials.
        Confirm the AWS Region is correct.
        Ensure Bedrock model access is enabled in that Region.
        Read the Terraform error carefully before making changes.

If the Lambda runs but no AI summary appears:

        Confirm the Lambda execution role includes bedrock:InvokeModel.
        Check the Lambda's CloudWatch log group for exceptions.
        Verify the configured model ID matches an enabled Bedrock model in the Region.

