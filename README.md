# Dynamic Website Hosting on AWS
This project outlines the process of deploying and hosting a dynamic website on AWS, utilizing a range of AWS services to ensure high availability, scalability, security, and fault tolerance.

## Project Overview

I hosted a Dynamic Website on AWS for a DevOps project and utilized the following resources. I have created a GitHub repository with the reference diagram and deployment scripts. Please use this information to understand the project setup and deployment process.

## Architecture Overview

The website is hosted on Amazon EC2 instances within a Virtual Private Cloud (VPC) that is configured with both public and private subnets spanning two Availability Zones. The infrastructure leverages the following AWS resources:

1. **Virtual Private Cloud (VPC)**: Configured with both public and private subnets that spanned two availability zones.
2. **Internet Gateway**: Deployed to enable connectivity between the VPC instances and the wider internet.
3. **Security Groups**: Established as network firewall mechanisms to control inbound and outbound traffic.
4. **Availability Zones**: Leveraged two Availability Zones to increase system reliability and fault tolerance.
5. **Public Subnets**: Used for hosting infrastructure components like the NAT Gateway and Application Load Balancer.
6. **EC2 Instance Connect Endpoint**: Implemented for secure connections to assets within both public and private subnets.
7. **Private Subnets**: Web servers (EC2 instances) were placed within Private Subnets for enhanced security.
8. **NAT Gateway**: Allowed instances in both the private Application and Data subnets to access the Internet securely.
9. **EC2 Instances**: The website was hosted on these instances.
10. **Application Load Balancer**: Used to evenly distribute web traffic to an Auto Scaling Group of EC2 instances across multiple Availability Zones.
11. **Auto Scaling Group**: Automatically managed EC2 instances to ensure website availability, scalability, fault tolerance, and elasticity.
12. **AWS Certificate Manager**: Secured application communications using SSL/TLS certificates.
13. **Simple Notification Service (SNS)**: Configured to send alerts about activities within the Auto Scaling Group.
14. **Route 53**: Registered the domain name and set up a DNS record for the website.
15. **S3 Bucket**: Used to store application code and assets.

## Deployment

The infrastructure deployment is automated through scripts and configuration files available in a GitHub repository. The repository includes the following components:

- **Reference Diagram**: A visual representation of the AWS infrastructure and its various components.
- **Deployment Scripts**: Scripts that automate the provisioning and configuration of AWS resources.

## Usage

1. Clone the GitHub repository to your local machine.
2. Follow the instructions provided in the repository documentation to set up the required AWS resources.
3. Deploy the website code and assets to the designated S3 bucket.
4. Access the website using the registered domain name through Route 53.



