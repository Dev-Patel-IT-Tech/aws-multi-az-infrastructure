# AWS Multi-AZ Infrastructure Deployment

AWS infrastructure deployment covering compute, load balancing, networking, storage, Infrastructure as Code, static hosting, and container workloads.

## AWS Services Used

- Amazon EC2
- Application Load Balancer
- Auto Scaling
- Amazon CloudWatch
- Amazon VPC
- Amazon EBS
- Amazon S3
- AWS CloudFormation
- Amazon ECS (Fargate)

## Scope

This repository documents the deployment and validation of multiple AWS services across compute, networking, storage, Infrastructure as Code, and container runtime layers.

The environment included automated EC2 provisioning, multi-AZ traffic distribution through an Application Load Balancer, dynamic Auto Scaling, structured VPC design, EBS storage attachment, S3 static website hosting, CloudFormation-based infrastructure deployment, and ECS Fargate workload execution.

## EC2 Automation and Load Distribution

EC2 instances were initialized using user data to provision an Apache web server at launch. Instance-specific context was rendered using the Instance Metadata Service (IMDS), allowing runtime behavior to be validated directly from the web layer.

The architecture was then extended with an Application Load Balancer and dynamic Auto Scaling. Load generation triggered CloudWatch-based scale-out activity and traffic distribution across multiple Availability Zones.

![Apache IMDS Output](images/apache-imds-output.png)

![ALB Traffic us-east-1a](images/alb-traffic-us-east-1a.png)
![ALB Traffic us-east-1b](images/alb-traffic-us-east-1b.png)
![ALB Traffic us-east-1c](images/alb-traffic-us-east-1c.png)
![ALB Traffic us-east-1d](images/alb-traffic-us-east-1d.png)

![EC2 Instances Multi AZ](images/ec2-instances-multi-az.png)

## EBS Volume Integration

Additional block storage was provisioned using a GP3 EBS volume and attached to a running EC2 instance. Device visibility was confirmed at the operating system level, followed by filesystem creation, mount validation, and file write verification.

![EBS Volumes GP3](images/ebs-volumes-gp3.png)
![lsblk xvdb Attached](images/lsblk-xvdb-attached.png)
![Filesystem Write Test](images/filesystem-write-test.png)

## S3 Static Website Hosting

Static website hosting was configured on Amazon S3 using a dedicated bucket and controlled public object access. Website assets were uploaded and validated through the S3 website endpoint, confirming correct object delivery from client to service endpoint.

## CloudFormation Deployment

Infrastructure components were provisioned and updated through CloudFormation templates written in YAML. Stack operations included EC2, security groups, EBS attachment, S3 integration, and VPC deployment with routing components.

![CloudFormation EC2 Stack Complete](images/cfn-ec2-stack-complete.png)
![CloudFormation EC2 Instance Running](images/cfn-ec2-instance-running.png)
![CloudFormation VPC Stack Complete](images/cfn-vpc-stack-complete.png)

## VPC Architecture and Routing

A custom Amazon VPC was implemented with segmented public and private subnets across multiple Availability Zones. Routing behavior was controlled through dedicated route tables and Internet Gateway integration.

![VPC Resource Map Manual](images/vpc-resource-map-manual.jpeg)
![VPC Resource Map CloudFormation](images/vpc-resource-map-cloudformation.png)
![Subnets Overview](images/subnets-overview.png)
![Route Tables Overview](images/route-tables-overview.png)
![Public Route Table IGW](images/public-route-table-igw.jpeg)
![Internet Gateways](images/internet-gateways.png)

## ECS Fargate Deployment

A containerized workload was deployed on Amazon ECS using the Fargate launch type. The task definition used an nginx image with configured networking, security controls, and public endpoint validation.

![ECS Clusters Overview](images/ecs-clusters-overview.png)
![ECS Task Running](images/ecs-task-running.png)
![ECS Task Networking](images/ecs-task-networking.png)
![Nginx Endpoint Validation](images/nginx-endpoint-validation.png)

## Key Technical Outcomes

- Automated EC2 initialization using user data
- Runtime metadata validation through IMDS
- ALB-based traffic distribution across multiple Availability Zones
- Dynamic scale-out behavior through CloudWatch and Auto Scaling
- Structured VPC segmentation with controlled public routing
- EBS attachment and filesystem validation at Linux OS level
- Static website hosting through Amazon S3
- Declarative infrastructure deployment using CloudFormation
- Container execution on ECS Fargate without instance management
