# WideBot-DevOps
## WideBot-DevOps
About
This repository contains the infrastructure as code for WideBot DevOps environment. It includes the infrastructure for VPC, EKS, MongoDB, Redis, SQL Server, and Web application.

## Overview
This Terraform code provisions an infrastructure on AWS that includes a Virtual Private Cloud (VPC) with public and private subnets, an internet gateway, security groups for load balancing, web servers, Redis, MongoDB and SQL Server, an Elastic Kubernetes Service (EKS) cluster, and Kubernetes deployments and services for web app, databases and SQL Server. It also creates a load balancer and target group for the web app.

## Components

### VPC
- The VPC.tf file contains the Terraform code to create the Virtual Private Cloud (VPC) for the DevOps environment. The VPC is the foundation for the entire infrastructure and allows you to isolate your resources from other networks and the internet.

- The VPC code creates a VPC with two public and two private subnets spread across two availability zones. The code also creates an internet gateway and attaches it to the VPC, allowing resources in the public subnet to communicate with the internet.

- The VPC code also creates two private route tables and two public route tables. The private route tables are associated with the private subnets and route traffic to a NAT gateway in the public subnet. The NAT gateway allows resources in the private subnet to communicate with the internet while remaining private. The public route tables are associated with the public subnets and route traffic to the internet gateway.

- The VPC code also creates a security group that allows inbound traffic on port 22 (SSH) from a designated IP address. This security group is used to allow SSH access to the worker nodes in the EKS cluster.

- Overall, the VPC.tf file is an important part of the infrastructure as it provides the foundation for the entire environment. By creating a VPC with public and private subnets, the code isolates resources and provides security. The NAT gateway in the private subnet allows resources to communicate with the internet while remaining private, and the security group provides secure access to the worker nodes.
### Security Groups
- Security Groups are an important component of the WideBot-DevOps infrastructure. A security group acts as a virtual firewall for the instances in your Amazon VPC. It controls inbound and outbound traffic and allows you to define rules to allow or deny traffic to and from the instances.

- In the Security.tf file, the Terraform code defines a security group that allows inbound traffic on port 22 (SSH) from a designated IP address. This security group is used to allow SSH access to the worker nodes in the EKS cluster. The code also defines a security group for the MongoDB instance that allows inbound traffic on port 27017 (default MongoDB port) only from the private subnet.

- By using security groups to control traffic, the infrastructure is more secure as it ensures that only authorized traffic can access resources. The security group for SSH access to the worker nodes ensures that only authorized users can access the nodes, while the security group for MongoDB ensures that only resources in the private subnet can access the database.
### Load Balancer
- Load Balancers are a crucial component of the WideBot-DevOps infrastructure as they help to distribute traffic across the worker nodes in the EKS cluster. Load Balancers increase the availability and fault tolerance of the infrastructure by automatically distributing traffic across healthy nodes.

- In the web_app_service.yaml file, the Kubernetes manifest defines a Load Balancer service for the Web application. The Load Balancer service distributes traffic across the worker nodes in the EKS cluster that are running the Web application. The service also ensures that only healthy nodes receive traffic by monitoring the health of the nodes and automatically routing traffic to healthy nodes.

- By using Load Balancers, the infrastructure is more resilient as it ensures that traffic is distributed across healthy nodes in the EKS cluster. This helps to prevent a single point of failure and increases the availability of the Web application.
### Autoscaling Group
- Autoscaling Groups are an important component of the WideBot-DevOps infrastructure as they allow you to automatically adjust the number of worker nodes in the EKS cluster based on the demand for the Web application. Autoscaling Groups help to ensure that the infrastructure can handle increases in traffic and maintain performance during peak usage periods.

- In the node_groups.tf file, the Terraform code defines an Autoscaling Group for the worker nodes in the EKS cluster. The Autoscaling Group automatically adjusts the number of worker nodes based on the demand for the Web application. The code also defines a Launch Configuration that specifies the instance type, AMI, and other details for the worker nodes.

- By using Autoscaling Groups, the infrastructure is more responsive to changes in demand as it can automatically adjust the number of worker nodes based on traffic. This helps to ensure that the Web application can handle increases in traffic and maintain performance during peak usage periods.
### Databases
- Databases are a critical component of the WideBot-DevOps infrastructure as they store and manage the data used by the Web application. The infrastructure includes three types of databases: MongoDB, Redis, and SQL Server.

- In the mongodb_service.yaml and mongodb_statefulset.yaml files, the Kubernetes manifest defines a MongoDB service and statefulset. The MongoDB service provides access to the MongoDB statefulset, which manages the MongoDB instances. The code also defines a MongoDB Persistent Volume Claim (PVC) that provides persistent storage for the MongoDB instances.

- In the redis_service.yaml and redis_statefulset.yaml files, the Kubernetes manifest defines a Redis service and statefulset. The Redis service provides access to the Redis statefulset, which manages the Redis instances. The code also defines a Redis PVC that provides persistent storage for the Redis instances.

- In the sqlserver_service.yaml and sqlserver_statefulset.yaml files, the Kubernetes manifest defines a SQL Server service and statefulset. The SQL Server service provides access to the SQL Server statefulset, which manages the SQL Server instances. The code also defines a SQL Server PVC that provides persistent storage for the SQL Server instances.

- By using databases, the infrastructure can store and manage the data used by the Web application. The MongoDB database is used to store and manage the chatbot conversations, while the Redis database is used for caching. The SQL Server database is used for storing and managing user data.
### Kubernetes
- Kubernetes is a key component of the WideBot-DevOps infrastructure as it is used to manage the containers that run the Web application and the databases. Kubernetes is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications.

- In the web_app_deployment.yaml file, the Kubernetes manifest defines a Deployment for the Web application. The Deployment manages the containers that run the Web application and ensures that the desired number of replicas are running at all times.

- In the mongodb_service.yaml and mongodb_statefulset.yaml files, the Kubernetes manifest defines a MongoDB service and statefulset. The statefulset manages the MongoDB instances and ensures that the desired number of replicas are running at all times.

- In the redis_service.yaml and redis_statefulset.yaml files, the Kubernetes manifest defines a Redis service and statefulset. The statefulset manages the Redis instances and ensures that the desired number of replicas are running at all times.

- In the sqlserver_service.yaml and sqlserver_statefulset.yaml files, the Kubernetes manifest defines a SQL Server service and statefulset. The statefulset manages the SQL Server instances and ensures that the desired number of replicas are running at all times.

- By using Kubernetes to manage the containers that run the Web application and the databases, the infrastructure can ensure that the containers are highly available and can scale to meet the demands of the Web application. Kubernetes provides features such as automatic scaling, rolling updates, and self-healing that help to ensure the availability and reliability of the containers.
## Security Components
### Network ACLs
- Network ACLs are an important component of the WideBot-DevOps infrastructure as they provide an additional layer of security for the VPC. Network ACLs act as a firewall at the subnet level and control inbound and outbound traffic.

- In the VPC.tf file, the Terraform code defines two Network ACLs: one for the public subnet and one for the private subnet. The Network ACL for the public subnet allows inbound traffic on ports 80 (HTTP) and 443 (HTTPS), while the Network ACL for the private subnet allows inbound traffic on port 27017 (default MongoDB port) from the public subnet.

- By using Network ACLs to control traffic at the subnet level, the infrastructure is more secure as it ensures that only authorized traffic can access resources in the subnets. The Network ACL for the public subnet allows inbound traffic on ports 80 and 443, which are required for accessing the Web application, while the Network ACL for the private subnet allows inbound traffic on port 27017 only from the public subnet, which is required for accessing the MongoDB instance
### IAM Roles
- IAM Roles are an important component of the WideBot-DevOps infrastructure as they allow you to control access to AWS resources. IAM Roles are AWS Identity and Access Management (IAM) entities that define a set of permissions for making AWS service requests.

- In the eks_cluster.tf file, the Terraform code defines an IAM Role for the worker nodes in the EKS cluster. The IAM Role allows the worker nodes to access AWS resources, such as the S3 bucket that stores the Docker images used by the Web application.

- By using IAM Roles, the infrastructure is more secure as it ensures that only authorized resources can access AWS resources. The IAM Role for the worker nodes in the EKS cluster ensures that the nodes can only access the AWS resources that are required for running the Web application.
### SSL Certificate
- Creates an SSL certificate using AWS Certificate Manager to encrypt traffic between the web application and the load balancer.
- Configures the load balancer to use the SSL certificate for HTTPS traffic.

## Installation
To use this repository, follow the steps below:

Clone the repository:
git clone https://github.com/Ahmedfathyy/WideBot-DevOps.git
```
Change into the cloned directory:

cd WideBot-DevOps
Modify the variables.tf file to suit your environment.
Run the following command to initialize your working directory and download the necessary plugins:

terraform init
Run the following command to preview the changes that Terraform will make:

terraform plan
If the preview looks good, apply the changes with the following command:

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

![Blank diagram]
