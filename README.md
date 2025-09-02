# AWS VPC and EC2 Deployment Automation

## Overview
This Python project demonstrates automated deployment of an AWS Virtual Private Cloud (VPC) with public and private subnets, an Internet Gateway, route table, security group, and a free-tier EC2 instance. The script leverages **Boto3**, the AWS SDK for Python, to create and configure the cloud environment programmatically.  

---

## Features
- Automatically checks for an existing **key pair** (`VPCKey`) for secure SSH access.  
- Creates a **VPC** with DNS support and hostnames enabled.  
- Sets up **public and private subnets** within the VPC.  
- Attaches an **Internet Gateway** and configures routing for internet access.  
- Creates a **security group** allowing SSH (port 22) and HTTP (port 80) access.  
- Launches a **free-tier eligible EC2 instance** inside the public subnet.  

---

## Output Example
Key pair 'VPCKey' already exists.
VPC Created: vpc-06173f5c29acdc58f
Public Subnet: subnet-0cb1aafb358cc8f27, Private Subnet: subnet-01ac74d1039fc7ae4
Internet Gateway Attached: igw-087359a4c5fc23c30
Public Route Table Created: rtb-04384885dd747ef9f
Security Group Created: sg-0928a048a93f58403
Launching EC2 instance: i-0eeeac6b6880bb399
EC2 Instance Running. Public IP: None

✅ Deployment Complete!
SSH into EC2 using:
ssh -i path/to/VPCKey.pem ec2-user@None

---

## How It Works (Code Explanation)
1. **Initialize Clients:**  
   The script first creates Boto3 clients for EC2 (`ec2_client`) and the EC2 resource (`ec2_resource`) to interact with AWS services.  

2. **VPC Creation:**  
   It creates a VPC with the CIDR block `10.0.0.0/16` and enables DNS support and hostnames, allowing instances to resolve domain names and be reachable via DNS.  

3. **Subnets:**  
   The code creates a **public subnet** (`10.0.1.0/24`) and a **private subnet** (`10.0.2.0/24`) in the chosen availability zone, organizing the network into segments for different purposes.  

4. **Internet Gateway:**  
   An Internet Gateway (IGW) is created and attached to the VPC, enabling internet access for instances in the public subnet.  

5. **Route Table:**  
   A public route table is created and associated with the public subnet. A route (`0.0.0.0/0`) is added pointing to the IGW, allowing outbound internet traffic.  

6. **Security Group:**  
   A security group is created to allow inbound SSH (port 22) and HTTP (port 80) traffic. This ensures the EC2 instance can be accessed securely and serve web requests if needed.  

7. **EC2 Instance Launch:**  
   The script launches an EC2 instance using the specified AMI (`Amazon Linux 2`) and instance type (`t3.micro`). It assigns the instance to the public subnet and attaches the security group for SSH and HTTP access.  

8. **Instance Monitoring:**  
   The code waits until the instance is running, reloads its attributes, and prints its public IP for SSH access.  

---

## Visual Demonstration

```
VPC (10.0.0.0/16)
├── Internet Gateway
│   └── Connects VPC to Internet
├── Public Subnet (10.0.1.0/24)
│   └── EC2 Instance (t3.micro)
│       └── Security Group: SSH (22), HTTP (80)
└── Private Subnet (10.0.2.0/24)
    └── No EC2 instances launched
```
