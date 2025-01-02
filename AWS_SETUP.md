# Flowise AWS Infrastructure Setup
This repository provides the necessary CloudFormation templates and a GitHub Actions workflow to set up the AWS infrastructure for the Flowise application. The setup includes deploying resources such as VPC, ECS, EFS, RDS, ALB, and more to host the application in a scalable and secure environment.

## Prerequisites
### 1. **AWS Account**: 
You need an AWS account to deploy the resources.

### 2. **AWS CLI**: 
Ensure you have the AWS CLI installed and configured on your local machine.

### 3. **CloudFormation Knowledge**: 
Basic knowledge of AWS CloudFormation is helpful but not required.

### 4. **GitHub Repository**: 
Fork or clone this repository into your GitHub account.

### 5. **GitHub Secrets Setup**: 
Add the following secrets in your GitHub repository for the CI/CD pipeline to work:
- `AWS_ACCESS_KEY_ID`: Your AWS Access Key ID.

- `AWS_SECRET_ACCESS_KEY`: Your AWS Secret Access Key.

- `AWS_REGION`: The desired AWS region (e.g., us-east-1).

- `KEY_NAME`: The name of the EC2 key pair to use.

- `ROUTE_HZ_ID`: The Route53 Hosted Zone ID.

- `ACM_CERTIFICATE_ARN`: The ARN of the ACM certificate for HTTPS.

- `USERNAME`: The username for the PostgreSQL database.

- `PASSWORD`: The password for the PostgreSQL database.

- `SSH_KEY`: The SSH key for accessing EC2 instances.

## Deployment Workflow
The project uses GitHub Actions for CI/CD. The workflow automatically deploys the infrastructure when code is pushed to the develop or main branch.

### Steps to Deploy
1. Clone the Repository:
```bash
git clone https://github.com/SUMO-Scheduler/flowise.git
cd flowise
```

2. Configure GitHub Secrets:
<br>Go to your GitHub repository settings and configure the secrets as mentioned in the prerequisites.

3. Review CloudFormation Templates:
<br>Review the CloudFormation templates located in the aws-cloudformation directory to understand the resources being created.

4. Push Code to GitHub:
<br>Push the code to the develop or main branch. The GitHub Actions workflow will automatically trigger and deploy the infrastructure.

5. Monitor Deployment:
<br>Monitor the GitHub Actions logs to ensure the deployment is successful.

6. Access Application:
<br>Once the deployment is complete, the application will be accessible at the domain specified in the Route53 configuration (e.g., `flowise.sumoscheduler.com` for production or `dev-flowise.sumoscheduler.com` for development).

## CloudFormation Templates
The AWS infrastructure is defined using the following CloudFormation templates:

1. `ecr.yml`
<br>Creates an Amazon Elastic Container Registry (ECR) to store Docker images.

2. `vpc.yml`
<br>Sets up the VPC with public and private subnets, NAT gateways, and an Internet Gateway.

3. `alb.yml`
<br>Configures the Application Load Balancer (ALB) to route traffic to ECS tasks.

4. `postgre.yml`
<br>Deploys an Amazon RDS instance for PostgreSQL with daily backups enabled.

5. `ecs.yml`
<br>Creates an Amazon ECS cluster to run the Flowise application as Fargate tasks.

6. `efs.yml`
<br>Sets up Amazon Elastic File System (EFS) for persistent storage.

7. `iam.yml`
<br>Defines IAM roles and policies for ECS tasks and Auto Scaling.

8. `rotate_lambda.yml`
<br>Deploys an AWS Lambda function to handle automatic secret rotation.

9. `secret.yml`
<br>Creates a Secrets Manager secret to store database credentials, with daily rotation enabled.

10. `flowise.yml`
<br>Defines the ECS service and task definition for running the Flowise application.

11. `route.yml`
<br>Configures Route53 DNS records for domain name resolution.

12. `lambda.yml`
<br>Creates Lambda functions and API Gateway endpoints for retrieving and rotating credentials.

## GitHub Actions Workflow
The GitHub Actions workflow (`.github/workflows/deploy-flowise.yml`) automates the deployment process by:

- Validating CloudFormation templates.

- Creating or updating AWS resources using CloudFormation.

- Building and pushing Docker images to ECR.

- Deploying the Flowise application on ECS.

- Configuring Route53 DNS records.

- The workflow is triggered on every push to the `develop` or `main` branch.

## Outputs
After a successful deployment, the following outputs will be available:

- **Application URL**: The URL of the Flowise application (e.g., https://flowise.sumoscheduler.com or https://dev-flowise.sumoscheduler.com).

- **API Gateway Endpoints**:
  - **Get Credentials**: API to retrieve application credentials.

  - **Rotate Password**: API to rotate application credentials.

## User Roles and Permissions
The following AWS IAM roles and permissions are configured for different user groups:

- **Admins**: Full access to all AWS resources.

- **Support**: Read-only access to CloudWatch logs.

- **Developers**:
  - Read access to production resources.

  - Full access to development resources.

  - Full access to all logs.

- **Contractors**:
  - Read-only access to development resources.

  - No access to production resources or logs.

## Contributing
Contributions are welcome! Please fork the repository and create a pull request for any enhancements or bug fixes.

## License
This project is licensed under the MIT License. See the LICENSE file for more details.

By following these instructions, you should be able to deploy the Flowise application on AWS with the provided infrastructure setup.