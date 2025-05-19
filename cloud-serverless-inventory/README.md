# Multi-cloud Serverless Inventory
This project inventories serverless services across AWS, GCP, Azure, and Alibaba Cloud, and generates JSON, CSV, and HTML reports sent via email.

# roles/aws_serverless/tasks/main.yml
AWS Serverless Services Covered:
AWS Lambda

AWS Step Functions

Amazon EventBridge

AWS App Runner

AWS Fargate (for ECS Tasks)

Amazon S3 (event-based serverless trigger)

# roles/gcp_serverless/tasks/main.yml
GCP Serverless Services Covered:
Cloud Functions (1st & 2nd gen)

Cloud Run

Workflows

# roles/azure_serverless/tasks/main.yml
Azure Serverless Services Covered:
Azure Functions

Azure Logic Apps

Azure Container Apps

# roles/alibaba_serverless/tasks/main.yml
Alibaba Cloud Serverless Services Covered:
Function Compute (FC)

Serverless App Engine (SAE)
