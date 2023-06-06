# AWS Web Application Project

## Objective
Create and host a highly available and scalable web application for a theoretical university using AWS.

## Solution
Use AWS to build and deploy the infrastructure necessary to host a cost-effective and scalable web application.
+ Create a VPC that extends over two Availability Zones for redundancy.
+ Deploy the first EC2 instances to host our web application.
+ Configuring an Application Load Balancer helps us manage incoming traffic and to function as an endpoint for our VPC.
+ Implement an EC2 Auto Scaling group to launch and scale EC2 instances which will be used to host the university web application.

## Phase 1 Planning the Design
Before a single resource is provisioned, it's important to lay out a design and estimate cost so that most of the heavy lifting takes place outside of the AWS environment. LucidChart is a great tool to use for visualizing cloud infrastructure and to help create a map of all necessary resources. ![image](https://github.com/BJerdon/university-web-application/assets/133431472/8b0d94bc-581e-4d8e-89b0-8ab9b474aca3)
My original design was very complex. Most notably it featured AWS WAF, CloudFront, and two RDS Databases which were segmented from the rest of the network. I found that these were all superfluous and a lot of money would be saved by cutting these resources out of the final design.![image](https://github.com/BJerdon/university-web-application/assets/133431472/e9c6ae97-8073-423e-b9a3-716823c73425)

## Phase 2 Creating a multi-AZ VPC, NAT Gateway, and Security Group
The very first step that goes into building any cloud infrastructure is the creation of the Virtual Private Cloud (VPC) it will reside on.
The VPC functions as a private, isolated section of the cloud where we can safely construct and manage our cloud resources.
+ During the VPC creation process, I can manage the provisioning of 4 subnets into 2 AZs.
+ PublicSubnet1 and PrivateSubnet1 are both placed into the us-east-1a AZ.
+ PublicSubnet2 and PrivateSubnet2 are both placed into the us-east-1b AZ.
+ Before finalizing the VPC's creation, a NAT Gateway is also created in the us-east-1a AZ. We will use a route table to ensure both private subnets have access to this NAT Gateway.
+ Finally, I make sure to create a security group that blocks all inbound and outbound traffic except over HTTPS. (TCP 443)
![image](https://github.com/BJerdon/university-web-application/assets/133431472/91d9c4cd-4871-48b6-ad81-7290a0705bea)
With the VPC, AZs, subnets, NAT Gateway, route table configured, and security group created, I can move forward in provisioning the necessary hardware needed to host the university web application.

## Phase 3 Configuring and Deploying EC2 Instances
Before we can move forward with creating the rest of the necessary cloud infrastructure, I must ensure that the web application works. An EC2 instance will be created in one of the public subnets to do this. This EC2 instance will be a temporary host for our web application and will also act as the template for future EC2 instances that will be generated using an EC2 Auto Scaling Group.
+ When selecting which Amazon Machine Imagine to run on my EC2 instance, I'll decide on using the Ubuntu Server 22.04 LTS AMI because it works best when running the web application.
+ Because this EC2 is for testing, I made sure to save money by choosing the t2.micro instance type.
+ Key Pair is set to vockey.
+ Security group is set to the newly created Web Security Group.
+ The web application is uploaded to the EC2 user data.
+ After the instance has fully deployed, I simply drop the public IP into my browser URL and the university web application is visible.

## Phase 4 Utilizing an Application Load Balancer

## Phase 5 Building and Implementing an EC2 Auto Scaling Group

## Phase 6 Testing our Infrastructure Against High Internet Traffic
