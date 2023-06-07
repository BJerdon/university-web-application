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
Before a single resource is provisioned, it's important to lay out a design and estimate cost so that most of the heavy lifting takes place outside of the AWS environment. LucidChart is a great tool to use for visualizing cloud infrastructure and to help create a map of all necessary resources. ![image](https://github.com/BJerdon/university-web-application/assets/133431472/204c5868-415e-4691-8b79-a982a9008a96)
My original design was very complex. Most notably it featured AWS WAF, CloudFront, and two RDS Databases which were segmented from the rest of the network. I found that these were all superfluous and a lot of money would be saved by cutting these resources out of the final design.![image](https://github.com/BJerdon/university-web-application/assets/133431472/aa8d2621-4b38-4112-9bff-4e15d30bb43a)
My original pricing estimate was much higher than what I would actually spend creating and testing the infrastructure. However, this pricing calculator accounted for having thousands of users utilizing the web application at peak hours which then would require beefier hardware than what I would actually use. This estimate also includes the cost of using services like AWS WAF and RDS which as I said before were removed from the final design. 
https://github.com/BJerdon/university-web-application/blob/08638d1023be7a3c5cf88e13b8327d58efa14fe4/My%20Estimate%20-%20AWS%20Pricing%20Calculator.pdf
Despite the large costs present on this pdf, once all of the work was done being completed I had only used roughly 40 dollars building and testing the infrastructure.

## Phase 2 Creating a multi-AZ VPC, NAT Gateway, and Security Group
The very first step that goes into building any cloud infrastructure is the creation of the Virtual Private Cloud (VPC) it will reside on.
The VPC functions as a private, isolated section of the cloud where we can safely construct and manage our cloud resources.
+ During the VPC creation process, I can manage the provisioning of 4 subnets into 2 AZs.
+ PublicSubnet1 and PrivateSubnet1 are both placed into the us-east-1a AZ.
+ PublicSubnet2 and PrivateSubnet2 are both placed into the us-east-1b AZ.
+ Before finalizing the VPC's creation, a NAT Gateway is also created in the us-east-1a AZ. We will use a route table to ensure both private subnets have access to this NAT Gateway.
+ Finally, I make sure to create a security group that blocks all inbound and outbound traffic except over HTTPS. (TCP 443)
![image](https://github.com/BJerdon/university-web-application/assets/133431472/067903d4-1332-41a1-89fe-defc92816bf1)
With the VPC, AZs, subnets, NAT Gateway, route table configured, and security group created, I can move forward in provisioning the necessary hardware needed to host the university web application.

## Phase 3 Configuring and Deploying an EC2 Instance
Before we can move forward with creating the rest of the necessary cloud infrastructure, I must ensure that the web application works. An EC2 instance will be created in one of the public subnets to do this. This EC2 instance will be a temporary host for our web application and will also act as the template for future EC2 instances that will be generated using an EC2 Auto Scaling Group.
+ When selecting which Amazon Machine Imagine to run on my EC2 instance, I'll decide on using the Ubuntu Server 22.04 LTS AMI because it works best when running the web application.
+ Because this EC2 is for testing, I made sure to save money by choosing the t2.micro instance type.
+ Key Pair is set to vockey.
+ Security group is set to the newly created Web Security Group.
+ The web application is uploaded to the EC2 user data.
+ After the instance has fully deployed, I simply drop the public IP into my browser URL and the university web application is visible.
![image](https://github.com/BJerdon/university-web-application/assets/133431472/fd5c513f-9411-43e6-8ec5-b2af057390c6)

## Phase 4 Utilizing an Application Load Balancer
To ensure that network traffic is equally distributed between all of the EC2 instances hosting our web application, an Application Load Balancer needs to be implemented. The Application Load Balancer will need to be connected directly to each EC2 instance in both AZs.
+ I place the ALB into the previously created VPC
+ us-east-1a and us-east-1b are both mapped to the ALB as well.
+ Port 80 HTTP is opened up as a listener port.

After I create the EC2 Auto Scaling Group, the newly created Application Load Balancer will become the new access point for the university web application. Before we would connect using the EC2 instance's public IP address. Now we will use the ALB's DNS name to connect.
![image](https://github.com/BJerdon/university-web-application/assets/133431472/2299984b-a667-4f95-8f11-f7701bb984cd)

## Phase 5 Building and Implementing an EC2 Auto Scaling Group
The final step toward creating a functional and scalable university web server is creating an EC2 Auto Scaling Group. This Auto Scaling Group will observer the work load on each EC2 instance and if one becomes overworked, a new EC2 instance is created and the workload is redistributed evenly. Likewise, if an EC2 instance isn't being worked at all it is removed in order to save money.
+ First, I create the launch template using the first EC2 instance I orginally created in Phase 3.
+ I configure the EC2 Auto Scaling Group to use the load balancer.
+ A tracking policy is also implemented in order for the Auto Scaling Group to know when it needs to add or remove EC2 instances. I will be able to manage and observer this tracking via CloudWatch.
+ The Auto Scaling Group is set to always have at least 2 EC2 instances running plus give or take 4 more EC2 instances.
![image](https://github.com/BJerdon/university-web-application/assets/133431472/7f7212f8-e16f-4baf-8bf1-bc3a8de45896)
Now all of the neccissary infrastructure is in place to run the univserity web application in an efficent and cost effective way. With this done. The last thing to do is test our work to ensure it all works properly.

## Phase 6 Testing our Infrastructure Against High Internet Traffic
Due to the fact that there actually won't be thousands of people flooding to my web server, I have to find an alternative way to ensure that both the Application Load Balancer and the EC2 Auto Scaling Group are both working. The method I had success with requires the use of the AWS IDE called Cloud9. Using Cloud9 I could easily install a loadtest package and then run it.
+ I create a Cloud9 environment called UniversityIDE.
+ The environment is associated with my univserity vpc and subnets.
+ After my new environment is done being created, I load into the Cloud9 terminal and run the following two commands.
+ npm install -g loadtest
+ loadtest --rps 2000 -c 1000 -k http://[LoadBalancerDNS]
+ Now I just switch over to CloudWatch and observer as the workload on each of my EC2 instances skyrocket.
///
After a little wait a new EC2 instance is created by the EC2 Auto Scaling Group. This proves that the university web server functions properly and my work is complete.
![image](https://github.com/BJerdon/university-web-application/assets/133431472/cdb0c022-d77e-4eea-83ec-a1b7026347d3)
