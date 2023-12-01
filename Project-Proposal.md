# Project Proposal: WordPress Hosting Made Easy - Optimised AWS Infrastructure with Minimal Effort

## Overview
This proposal outlines distinct projects, each focusing on key aspects of AWS deployment, security, and automation.

## Goals
1. **WordPress Deployment on AWS:**
   - Gain practical experience with EC2, Linux, MySQL databases, and security groups.
   - Establish proficiency in setting up web applications using AWS resources.

2. **Advanced WordPress Deployment:**
   - Elevate WordPress deployment by creating a VPC with public and private subnets.
   - Set up a MariaDB database and implement secure connections using security groups and an application load balancer.
   - Address real-world scenarios for secure and scalable application deployment.

3. **Infrastructure as Code (IaC) Mastery with Terraform:**
   - Solidify IaC skills by recreating previous projects using Terraform.
   - Showcase expertise in infrastructure automation by replacing manual resource creation with Terraform scripts.

## Specifications
### WordPress Deployment on EC2
- **Duration:** 1 weeks
- **Key Skills:** EC2, Linux, MySQL, Security Groups
- **Deliverables:**
  - Successfully deployed WordPress server on an EC2 instance.
  - Documentation demonstrating proficiency in resource setup.

### Advanced WordPress Deployment
- **Duration:** 1 weeks
- **Prerequisite:** Completion of WordPress Deployment on EC2
- **Key Skills:** VPC, Subnets, MySQL mariadb, Security Groups, Application Load Balancer
- **Deliverables:**
  - Deployed WordPress in an advanced environment with VPC and public subnets.
  - Set up a MySQL mariaddb database and established secure connections.
  - Documentation showcasing real-world application deployment scenarios.

### Infrastructure as Code (IaC) Mastery with Terraform
- **Duration:** 1 weeks
- **Prerequisite:** Completion of Advanced WordPress Deployment
- **Key Skills:** Terraform, Automation, Resource Recreation
- **Deliverables:**
  - Recreated all previous projects using Terraform scripts.
  - Demonstration of infrastructure automation expertise.

## Milestones
1. **Week 1:** Completion of WordPress Deployment on EC2
   - Successful deployment of a WordPress server.
   - Documentation submission.

2. **Week 1:** Completion of Advanced WordPress Deployment
   - Deployment of WordPress in an advanced environment.
   - Documentation of the setup and security measures.

3. **Week 2:** Completion of Infrastructure as Code (IaC) Mastery with Terraform
   - Recreated all projects using Terraform.
   - Final presentation showcasing Terraform scripts and automation benefits.

## Conclusion
These  projects aims to provide participants with a well-rounded understanding of AWS, ranging from foundational deployment to advanced scenarios and infrastructure automation. 

# Project - Solution: Deploy WordPress on EC2

1. Launch EC2 Instance
   - Choose Ubuntu 20.04 AMI: Ubuntu 20.04 LTS provides a stable, secure and updated OS for hosting WordPress
   - Select t2.micro instance: Good starting point to minimize costs for initial deployment
   - Add 8GB EBS volume: Provides adequate disk space for WordPress files and future growth  
   - Use default VPC and subnet: Simplifies networking configuration using default resources
   - Assign public IP: Allows direct internet access to server using public IP address 
   - Add Name tag: Identifies instance clearly for easy management

2. Connect to EC2 Instance
   - Use SSH to connect as ubuntu: Default ssh user for Ubuntu AMI to start configuration
   - Create sudo user: Optionally create less-privileged user for improved security

3. Install LAMP Stack
   - Update apt repository: Ensures latest security patches and versions available
   - Install Apache, MySQL, PHP: Required stack to host and run WordPress site
   - Start services: Initializes Apache web server and MySQL database server

4. Configure MySQL 
   - Run security script: Removes insecure default settings to harden MySQL
   - Create admin user: Admin account allows managing other users and databases
   - Create WordPress database: Separate database provides isolation for WP data
   - Secure installation: Remove root login and test strict security controls

5. Download and Configure WordPress
   - Get latest WordPress: Starts with most updated WP core files
   - Extract into web root: Places WP application code into default Apache folder 
   - Set file permissions: Allows web server to access and modify WP files
   - Create wp-config.php: Database credentials needed for WP installation
   - Complete install: Interactive setup to finalize configs 

6. Configure Security Groups
   - Restrict HTTP/HTTPS access: Limits who can access WordPress admin and site
   - Allow SSH: Permits admin troubleshooting access
   - Add rule for outbound Internet access: Allows WP to communicate externally 

7. Test WordPress Site
   - Verify dashboard access: Checks admin controls and settings work
   - Create test post/page: Confirms content creation and publishing functions
   - Confirm public accessibility: Asserts WordPress site visible over Internet

8. diitinal config -  Deploy Application Load Balancer 
   - Internet-facing in public subnets: Single entry point to access WordPress site
   - Listeners and health checks: Routes traffic only to healthy EC2 instances
   - EC2 target group: Registers backend WordPress application servers to receive requests

9. Configure Security Groups
   - Allow HTTP/HTTPS from ALB: Permits load balancer to forward internet traffic to EC2 fleet  
   - Enable RDS database access: Opens database traffic from EC2 tier over VPC  
   - Restrict direct internet to fleet: Reduces exposure by obscuring WordPress servers

10. Validate Solution
   - Upload media files: Checks performance with increased load
   - Simulate traffic: Confirms auto-scaling works under heavy load
   - Review metrics: Monitoring for infrastructure tuning and optimization


   ## Diagram of the  WordPress Deployment architecture on AWS

  