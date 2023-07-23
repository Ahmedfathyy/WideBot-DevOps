# WideBot-DevOps
## WideBot-DevOps
About
This repository contains the infrastructure as code for WideBot DevOps environment. It includes the infrastructure for VPC, EKS, MongoDB, Redis, SQL Server, and Web application.

## Overview
This Terraform code provisions an infrastructure on AWS that includes a Virtual Private Cloud (VPC) with public and private subnets, an internet gateway, security groups for load balancing, web servers, Redis, MongoDB and SQL Server, an Elastic Kubernetes Service (EKS) cluster, and Kubernetes deployments and services for web app, databases and SQL Server. It also creates a load balancer and target group for the web app.

## Components

### VPC
- Creates a VPC with a CIDR block of 10.0.0.0/16.
- Creates a public and private subnet with CIDR blocks of 10.0.1.0/24 and 10.0.2.0/24, respectively.
- Associates the public subnet with an internet gateway to allow outbound traffic.
- Associates the private subnet with a NAT gateway to allow inbound traffic.
### Security Groups
- Creates security groups for the web application, Redis database, MongoDB database, and SQL Server database.
- Configures the security groups to allow traffic from specific IP addresses and ports.
- Configures the security groups to deny all traffic that is not explicitly allowed.
### Load Balancer
- Creates an AWS Application Load Balancer to distribute incoming traffic to the web application running in the public subnet.
- Configures the listener rules that define how traffic is distributed to the target groups.
### Autoscaling Group
- Creates an AWS Auto Scaling Group to automatically scale the number of instances running the web application based on CPU utilization.
- Configures the launch settings for the instances, including the AMI ID, user data script, and security group.
### Databases
- Creates a Redis, MongoDB, and SQL Server database running in the private subnet.
- Configures the databases to use persistent storage and authentication.
- Configures the databases to only allow traffic from specific security groups.
### Kubernetes
- Uses Kubernetes manifests to deploy the web application and databases to the Kubernetes cluster.
- Configures Kubernetes Services to expose the internal IP addresses of the resources running in the private subnet.
- Configures the web application to connect to the databases using their internal IP addresses.
## Security Components
### Network ACLs
- Creates network ACLs for the public and private subnets to control inbound and outbound traffic.
- Configures the network ACLs to allow only necessary traffic and deny all other traffic.
### IAM Roles
- Creates an IAM role for the EC2 instances running in the Auto Scaling Group to control access to AWS resources.
- Configures the IAM role to grant only necessary permissions and deny all other permissions.
- Attaches the IAM role to the instances in the Auto Scaling Group.
Key Pair
- Creates an EC2 key pair to securely access the instances in the Auto Scaling Group.
- Configures the key pair to only allow access from specific IP addresses.
Associates the key pair with the instances in the Auto Scaling Group.
### SSL Certificate
- Creates an SSL certificate using AWS Certificate Manager to encrypt traffic between the web application and the load balancer.
- Configures the load balancer to use the SSL certificate for HTTPS traffic.

## Installation
To use this repository, follow the steps below:

Clone the repository:
Copy
git clone https://github.com/Ahmedfathyy/WideBot-DevOps.git
```
Change into the cloned directory:
Copy
cd WideBot-DevOps
Modify the variables.tf file to suit your environment.
Run the following command to initialize your working directory and download the necessary plugins:
Copy
terraform init
Run the following command to preview the changes that Terraform will make:
Copy
terraform plan
If the preview looks good, apply the changes with the following command:
Copy
terraform apply

## Resources
The resources included in this repository are:

VPC.tf: Contains the Terraform code to create the VPC for the environment.
eks.tf: Contains the Terraform code to create the Elastic Kubernetes Service (EKS) cluster.
node_groups.tf: Contains the Terraform code to create the worker nodes for the EKS cluster.
mongodb_service.yaml: Contains the Kubernetes manifest file to create the MongoDB service.
mongodb_statefulset.yaml: Contains the Kubernetes manifest file to create the MongoDB statefulset.
redis_service.yaml: Contains the Kubernetes manifest file to create the Redis service.
redis_statefulset.yaml: Contains the Kubernetes manifest file to create the Redis statefulset.
sqlserver_service.yaml: Contains the Kubernetes manifest file to create the SQL Server service.
sqlserver_statefulset.yaml: Contains the Kubernetes manifest file to create the SQL Server statefulset.
web_app_service.yaml: Contains the Kubernetes manifest file to create the Web application service.
web_app_manifest.yaml: Contains the Kubernetes manifest file to create the Web application deployment.
Variables.tf: Contains the variables used in the Terraform code.
Providers.tf: Contains the provider information used in the Terraform code.
Security.tf: Contains the security group and rule information used in the Terraform code.
SSL.tf: Contains the SSL certificate information used in the Terraform code.
Domain.tf: Contains the domain name information used in the Terraform code.
Local.tf: Contains the local variables used in the Terraform code.
Obtain_SSL_cert.sh: Contains the script to obtain SSL certificates.
Compose.yaml: Contains the Docker Compose file to run the application locally.