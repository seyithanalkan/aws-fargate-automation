# AWS Fargate Deployment Automation with GitHub Actions and Terraform

## Introduction

This project automates the deployment of applications to AWS Fargate, utilizing Terraform for creating network infrastructure (like VPCs and subnets) and GitHub Actions with CloudFormation for deploying Fargate components. It provides a dynamic and customizable CI/CD pipeline that's easily adjustable through environment variables, making it suitable for a wide range of projects.

## Features

- **Terraform for Network Infrastructure**: Use Terraform to reliably provision and manage AWS network resources such as VPCs, subnets, and IAM role.
- **GitHub Actions and CloudFormation for Fargate**: Automate the deployment of AWS Fargate services and related components using GitHub Actions in conjunction with AWS CloudFormation templates.
- **Environment Variable Configuration**: Customize your deployment settings via environment variables to adapt to various project needs.
- **Nginx Configuration Support**: Includes a ready-to-deploy setup for Nginx, facilitating web service deployment.

## Getting Started

### Prerequisites

- An AWS account with administrative permissions to ensure sufficient access rights for creating network resources via Terraform and deploying a wide range of services to AWS Fargate.
- Docker, AWS CLI, and Terraform installed on your local machine.
- Basic familiarity with GitHub Actions and AWS CloudFormation.

### Setup

1. **Fork or clone this repository** to start with your own version of the project.
2. **Configure GitHub Secrets**:
   - Go to your GitHub repository's Settings > Secrets.
   - Add AWS credentials and any other necessary environment variables as secrets for GitHub Actions to use in deploying your project to AWS Fargate.
3. **Customize the CI/CD Pipeline**:
   - Modify the `.github/workflows/fargate-deploy.yml` to align with your deployment strategy and environment variables.
   - Use Terraform to set up your AWS network infrastructure by adjusting configurations in the `/terraform` directory as needed.

### Deployment

#### 2. **GitHub Actions Workflow**:

The workflow consists of two main jobs: `build` and `deploy`, each responsible for different parts of the CI/CD process.

- **Build Job**:
  - **Checkout**: Checks out the source code for the push event.
  - **Configure AWS Credentials**: Configures AWS credentials using secrets stored in GitHub.
  - **Create or Verify ECR Repository**: Ensures the ECR repository exists for storing Docker images.
  - **Login to Amazon ECR**: Authenticates to Amazon ECR to push Docker images.
  - **Build, Tag, and Push Image**: Builds the Docker image, tags it with the GitHub SHA, and pushes it to the ECR repository.

- **Deploy Job**:
  - **Checkout** and **Configure AWS Credentials**: Similar to the build job, but prepares the environment for deployment.
  - **Fill in the new image ID in the Amazon ECS task definition**: Updates the ECS task definition with the new image ID using `amazon-ecs-render-task-definition` action.
  - **Register Task Definition**: Registers the new task definition with ECS.
  - **Deploy Shared Resources to AWS CloudFormation**: Deploys common infrastructure resources using CloudFormation.
  - **Ensure Target Group Exists**: Checks and creates a target group if necessary.
  - **Modify Target Group Attributes**: Adjusts attributes of the target group.
  - **Find Listener ARNs**: Identifies ARNs for HTTP and HTTPS listeners.
  - **Determine Listener Rule Priority**: Calculates priorities for new listener rules.
  - **Add Rule to HTTP/HTTPS Listener**: Creates routing rules for the listeners.
  - **Deploy ECS Service to AWS CloudFormation**: Deploys or updates the ECS service, linking it to the updated task definition.
  - **Deploy Amazon ECS task definition**: Updates the ECS service with the new task definition to initiate the deployment.

## Screenshots
![Github-Secret](https://drive.google.com/thumbnail?id=1FEAr0y0H2nd3A4Hh1nTXfr02Qft6Ul5J&sz=w1000)  
![Github-Action](https://drive.google.com/thumbnail?id=1FscD56NgZ1ad26-UnebtkiNzH_f1vuLf&sz=w1000)  
![Cloudformation](https://drive.google.com/thumbnail?id=1gLqH_2FWLv6UWcOV9SUIXC_FNtE0mJuY&sz=w1000)  
![Loadbalancer](https://drive.google.com/thumbnail?id=1iYPKC3qZvmFbrKwkpJhCIiY427yMBwZ5&sz=w1000)  
![LB-Rules](https://drive.google.com/thumbnail?id=1IXp5HD0bzsfra0EwK9JtQ353YwWuYu-F&sz=w1000)   
![Target-Group](https://drive.google.com/thumbnail?id=1dqiR4E7N12aXwpbB085ByO03ke848wgl&sz=w1000)  
![Fargate-Service](https://drive.google.com/thumbnail?id=1cnpPUEhBsvm3sqGTJyCNOTQcLZ91THjy&sz=w1000)
![Endpoints](https://drive.google.com/thumbnail?id=1-mVmTkMBtXTJc5vH-Hb7cfgqGohj2lvf&sz=w1000) 


## Project Structure

- `/terraform`: Terraform configurations for VPC, subnets, and other network resources.
- `/nginx`: Configuration files for Nginx.
- `/aws`: CloudFormation templates for deploying Fargate services and related AWS resources.
- `/.github`: Contains the GitHub Actions workflows that automate the CI/CD pipeline.

## Contributing

Contributions to enhance the project or add new features are highly encouraged. Feel free to fork the repository, implement your changes, and submit a pull request.


