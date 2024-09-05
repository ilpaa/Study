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
### Creating a VPC

![image](https://github.com/user-attachments/assets/463274f5-9efe-4123-b103-7975806bac77)

1. **Begin creating a VPC**:
   - In the top-right of the screen, verify that **N. Virginia (us-east-1)** is the region.
   - Choose the **VPC dashboard** link, located toward the top left of the console.
   - Next, choose ![image](https://github.com/user-attachments/assets/6445f67e-8790-42aa-9066-d906c0d9258e)
.
     - _Note: If you do not see a button with that name, choose the **Launch VPC Wizard** button instead._

2. **Configure the VPC details in the VPC settings panel**:
   - Choose **VPC and more**.
   - Under **Name tag auto-generation**, keep **Auto-generate** selected, but change the value from `project` to `lab`.
   - Keep the **IPv4 CIDR block** set to `10.0.0.0/16`.
   - For **Number of Availability Zones**, choose `1`.
   - For **Number of public subnets**, keep the setting as `1`.
   - For **Number of private subnets**, keep the setting as `0`.

3. **Expand the Customize subnets CIDR blocks section**:
   - Change **Public subnet CIDR block** in `us-east-1a` to `10.0.0.0/24`.

4. **Set other options**:
   - Set **NAT gateways** to `None`.
   - Set **VPC endpoints** to `None`.
   - Keep both **DNS hostnames** and **DNS resolution** enabled.

Once the creation is complete, choose ![image](https://github.com/user-attachments/assets/fad0bca4-6870-4334-8e86-582e04b71f37)


The wizard will have provisioned a VPC with a public subnet and a private subnet in one Availability Zone, with route tables for each subnet. It will also have created an Internet Gateway. To view the settings of these resources, browse through the VPC console links, such as the **Subnets** link to view subnet details or the **Route tables** link to view the route table details.

![image](https://github.com/user-attachments/assets/0c62f86e-5126-48da-a1a7-a9218e1095c9)


