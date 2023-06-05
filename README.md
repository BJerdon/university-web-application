# AWS Web Application Project

## Objective
Create and host a highly available and scalable web application for a theoretical university using AWS.

## Solution
Use AWS to build and deploy the infrastructure necessary to host a cost effective and scalable web application.
+ Create a VPC which extends over two Availability Zones for redundancy.
+ Deploy the first EC2 instances to host our web application.
+ Configure an Application Load Balancer help us manage incoming traffic and to function as an endpoint for our VPC.
+ Implement an EC2 Auto Scaling group to lauch and scale EC2 instances which will be used to host the univserity web application.

## Phase 1 Planning the Design and Estimating the Cost
Before a single resource is provisioned, it's important to lay out a design and estimate cost so that most of the heavy lifting takes place outside of the AWS enviornment, and no money or time is
wasted making changes during the resource allocation process. LucidChart is a create tool to help in visualizing cloud infrastructure and to help create a map of all necessary resources. 

## Phase 2 Creating a multi-AZ VPC and NAT Gateway
The very first step that goes into building any cloud infrastructure is the creation of the Virtual Private Cloud (VPC) it will reside on.
The VPC functions as an private, isolated section of the cloud where we can safely custruct and manage our cloud resources.
+ During the VPC creation process we can manage the provisioning of 4 subnets into 2 AZs.
+ PublicSubnet1 and PrivateSubnet1 are both placed into the us-east-1a AZ.
+ PublicSubnet2 and PrivateSubnet2 are both placed into the us-east-1b AZ.
+ Before finializing the VPC's creation, a NAT Gateway is also created in the us-east-1a AZ. We will use a route table to ensure both private subnets have access to this NAT Gateway.
+ Finally, the VPC is created with all of the basic networking infrastructure provisioned.
With the VPC, AZs, subnets, and NAT Gateway, and route table configured, we can move forward in provisioning the necissarying hardware needed to host the univserity web application.

## Phase 3 Configuring and Deploying EC2 Instances


## Phase 4 Utilizing an Application Load Balancer

## Phase 5 Building and Implementing an EC2 Auto Scaling Group

## Phase 6 Testing our Infrastructure Against High Internet Traffic
