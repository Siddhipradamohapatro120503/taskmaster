# TaskMaster Deployment Guide

This guide explains how to deploy the TaskMaster Spring Boot application using Docker, AWS ECS, and GitHub Actions.

## Prerequisites

1. AWS Account with appropriate permissions
2. GitHub account
3. Docker installed locally
4. AWS CLI installed and configured
5. Terraform installed

## Infrastructure Setup

### 1. AWS Credentials

Create an IAM user with the following permissions:
- AmazonECS_FullAccess
- AmazonECR_FullAccess
- AmazonVPCFullAccess
- CloudWatchLogsFullAccess

Add the credentials to GitHub repository secrets:
- AWS_ACCESS_KEY_ID
- AWS_SECRET_ACCESS_KEY

### 2. Terraform Infrastructure

The infrastructure is defined in the `terraform` directory:

```bash
cd terraform
terraform init
terraform plan
terraform apply
```

This will create:
- VPC with public and private subnets
- ECR repository
- ECS cluster and service
- Security groups
- CloudWatch log groups

### 3. Application Deployment

The application is automatically deployed through GitHub Actions when code is pushed to the main branch. The pipeline:

1. Builds the Spring Boot application
2. Runs tests
3. Builds Docker image
4. Pushes to ECR
5. Updates ECS service
6. Monitors deployment

## Infrastructure Components

### VPC and Networking
- VPC with public and private subnets
- Internet Gateway for public subnets
- NAT Gateway for private subnets
- Route tables for subnet routing

### Container Infrastructure
- ECR repository for Docker images
- ECS Fargate cluster
- ECS task definition and service
- Application load balancer

### Monitoring and Logging
- CloudWatch log groups
- Container insights enabled
- ECS service metrics

## GitHub Actions Pipeline

The CI/CD pipeline (`/.github/workflows/main.yml`) includes:

1. Build and Test:
   - Builds Spring Boot application
   - Runs unit tests

2. Deploy:
   - Builds Docker image
   - Pushes to ECR
   - Updates ECS service

3. Monitoring:
   - Checks deployment status
   - Monitors CloudWatch logs

## Security Considerations

1. Network Security:
   - Private subnets for ECS tasks
   - Security groups limiting access
   - HTTPS for load balancer

2. Access Control:
   - IAM roles for ECS tasks
   - Least privilege principle
   - Secrets management

## Scaling and Maintenance

### Scaling
- Adjust `desired_count` in Terraform for horizontal scaling
- Modify container CPU/memory in variables.tf
- Auto-scaling can be added to ECS service

### Monitoring
- CloudWatch metrics for ECS services
- Container logs in CloudWatch
- ECS service health checks

## Troubleshooting

1. Deployment Issues:
   - Check GitHub Actions logs
   - Verify ECS service status
   - Review CloudWatch logs

2. Application Issues:
   - Check container logs
   - Verify security group rules
   - Check task definition

## Cost Optimization

- Use Fargate Spot for non-critical workloads
- Scale down during off-hours
- Monitor CloudWatch metrics for right-sizing

## Future Improvements

1. Add auto-scaling based on metrics
2. Implement blue-green deployments
3. Add application performance monitoring
4. Set up disaster recovery procedures
5. Implement cost optimization strategies

## Support

For issues or questions:
1. Check CloudWatch logs
2. Review GitHub Actions logs
3. Verify Terraform state
4. Contact the development team

## Maintenance

Regular maintenance tasks:
1. Update dependencies
2. Review security patches
3. Monitor resource usage
4. Backup Terraform state
5. Review access permissions
