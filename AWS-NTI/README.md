# AWS VPC Web Server Hosting

## Overview
This project demonstrates how to create a custom AWS Virtual Private Cloud (VPC) to host a website. The VPC includes public and private subnets across multiple availability zones. An EC2 instance is launched in the public subnet, and a security group is configured to allow HTTP traffic for website hosting.
![image](https://github.com/user-attachments/assets/97d89698-cf7c-409b-b77e-82ba321b150f)


The website hosted in this lab is a project I created. The entire setup follows AWS best practices, ensuring security and high availability.

## Objectives
- Create a custom VPC with subnets in two Availability Zones
- Configure a NAT Gateway for internet access from the private subnet
- Set up security groups to allow HTTP access
- Launch an EC2 instance and deploy a web server
- Host a custom website on the EC2 instance

## Architecture
The architecture consists of:
- A VPC with both public and private subnets
- Route tables for the subnets
- An Internet Gateway and NAT Gateway
- An EC2 instance running in the public subnet with a web server
- A custom web page hosted on the EC2 instance

## Steps
### 1. Create a Custom VPC
- Navigate to the VPC dashboard and create a new VPC.
- Specify a CIDR block for the VPC (e.g., `10.0.0.0/16`).
- Add public and private subnets with CIDR blocks:
  - Public Subnet (AZ 1a): `10.0.0.0/24`

- Attach an Internet Gateway.

### 2. Configure Route Tables
- Public subnets should route `0.0.0.0/0` traffic through the Internet Gateway.


### 3. Create Security Group
- Create a security group allowing inbound HTTP (port 80) traffic from anywhere.

### 4. Launch EC2 Instance
- Launch an EC2 instance in one of the public subnets.
- Choose Amazon Linux as the AMI and a `t2.micro` instance type.
- Attach the previously created security group.

### 5. Deploy Website
- Connect to the EC2 instance via SSH.
- Install Apache Web Server:
  ```bash
  sudo yum update -y
  sudo yum install -y httpd
  sudo systemctl start httpd
  sudo systemctl enable httpd
Upload your website files to /var/www/html/ directory:
bash
Copy code
sudo cp -r /path/to/your/website/* /var/www/html/
Verify the website by entering the EC2 instance's public DNS in the browser.
