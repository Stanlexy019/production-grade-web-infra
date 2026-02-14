# Production-Grade Two-Tier AWS Infrastructure (Terraform)

This project demonstrates the design and deployment of a production-style two-tier architecture on AWS using Terraform.

It began as a hand-drawn architecture sketch and was translated into a fully functional, multi-AZ, Infrastructure as Code deployment.

---

## Architecture Overview

The infrastructure includes:

- VPC with custom CIDR block
- Public and Private subnets across multiple Availability Zones
- Internet Gateway & NAT Gateway configuration
- Public and Private Route Tables
- Application Load Balancer (ALB)
- Auto Scaling Group (ASG) for application instances
- Launch Template with automated bootstrap configuration
- RDS deployed across multiple AZs
- Layered Security Groups (ALB → App → DB)
- Health checks and traffic routing validation

---

## Traffic Flow

1. Users access the application via the ALB DNS endpoint.
2. The ALB forwards traffic to healthy EC2 instances in private subnets.
3. EC2 instances communicate with the RDS database in isolated private DB subnets.
4. Private instances access the internet via NAT Gateway for updates and package installation.

---

##  Security Design

- ALB allows inbound HTTP traffic from the internet.
- Application instances only accept traffic from the ALB security group.
- Database only accepts traffic from the application security group.
- Compute resources are deployed in private subnets (no public IPs).

This ensures proper network segmentation and least-privilege communication between tiers.

---

## Database Layer

Engine: MySQL (Multi-AZ deployment)

- Deployed across private DB subnets
- Associated with a dedicated DB subnet group
- Not publicly accessible
- Accessible only via the application security group
- The database tier is fully isolated from internet exposure and follows production-level best practices.

## Infrastructure as Code Structure

The project is organized into logical Terraform layers:

## ⚙️ Infrastructure as Code Structure

The Terraform configuration is organized into clearly defined logical layers:

**Network Layer**
- 00-provider.tf
- 01-vpc.tf
- 02-int-gateway.tf
- 03-public-subnets.tf
- 04-private-subnets.tf
- 05-elastic-ip.tf
- 06-nat-gateway.tf
- 07-public-route-table.tf
- 08-private-route-table.tf
- 09-route-table-associations.tf

**Security Layer**
- 10-alb-security-group.tf
- 11-app-security-group.tf
- 12-db-security-group.tf

**Load Balancing Layer**
- 13-target-group.tf
- 14-alb.tf
- 15-listener.tf

**Compute Layer**
- 16-launch-template.tf
- 17-auto-scaling-group.tf

**Database Layer**
- 18-rds-subnet-group.tf
- 19-rds.tf
