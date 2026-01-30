# Assignment 1 - VPC and Networking

* The goal of this assignment is to create a custom VPC with one public and private subnet, set up the correct routing for internet access, and deploy EC2 instances across them. Below will follow a step by step process on how to create this with screenshots documenting these steps.

## STEP 1: VPC creation

* created a custom VPC with a CIDR block of `10.0.0.0/16`
![VPC creation](https://github.com/morysk/Aws/blob/6b5c9076062cba6c43a94ee04ea8d4a973eaa893/assignment%201/screenshots/VPC.png)
* created one public subnet 
![Public subnet](https://github.com/morysk/Aws/blob/42b30234b29a1adaa7516384ede6d97c57d949f7/assignment%201/screenshots/subnet-pub.png)
* created one private subnet
![Private subnet](https://github.com/morysk/Aws/blob/42b30234b29a1adaa7516384ede6d97c57d949f7/assignment%201/screenshots/subnet-priv.png)

## Step 2: Internet access

In order for the VPC to gain internet access, an internet gateway was created. This was attached to the VPC. A NAT gateway was then created to allow outbound internet access for resources in the private subnet. An Elastic IPv4 address was then attached to the NAT gateway.
* Created and attach an Internet Gateway
![igw](https://github.com/morysk/Aws/blob/42b30234b29a1adaa7516384ede6d97c57d949f7/assignment%201/screenshots/igw.png)
* Created an Elastic IP
![igw](https://github.com/morysk/Aws/blob/42b30234b29a1adaa7516384ede6d97c57d949f7/assignment%201/screenshots/elastic%20ip.png)
* Created a NAT Gateway in the public subnet
![ngw](https://github.com/morysk/Aws/blob/42b30234b29a1adaa7516384ede6d97c57d949f7/assignment%201/screenshots/natgw.png)

## Step 3: Route tables

* Public route table was configured with a default route `(0.0.0.0/0)` to the Internet Gateway
![pub-rt](https://github.com/morysk/Aws/blob/42b30234b29a1adaa7516384ede6d97c57d949f7/assignment%201/screenshots/public-rt.png)
* Private route table was configured with a default route `(0.0.0.0/0)` to the NAT Gateway
![priv-rt](https://github.com/morysk/Aws/blob/42b30234b29a1adaa7516384ede6d97c57d949f7/assignment%201/screenshots/priv-rt.png)
* Each route table was associated with the respective subnets

## Step 4: EC2 instances and Bastion Host

* Public EC2 was launched in public subnet with a public IP assigned
![ec2-pub](https://github.com/morysk/Aws/blob/42b30234b29a1adaa7516384ede6d97c57d949f7/assignment%201/screenshots/public-ec2.png)
* Private EC2 was launched in private subnet without public IP assigned
![ec2-priv](https://github.com/morysk/Aws/blob/42b30234b29a1adaa7516384ede6d97c57d949f7/assignment%201/screenshots/private-ec2.png)

A Bastion host was deployed to allow access to the private EC2. This sits in the public subnet. limited inbound access was provided accepting only SSH.
![BH](https://github.com/morysk/Aws/blob/42b30234b29a1adaa7516384ede6d97c57d949f7/assignment%201/screenshots/bastion%20host.png)

## Step 5: Security

* Public EC2 Security group was configured to allow SSH/HTTP only from my IP
![pub-sg](https://github.com/morysk/Aws/blob/42b30234b29a1adaa7516384ede6d97c57d949f7/assignment%201/screenshots/public-sg.png)
* Private EC2 Security group was configured to allow only internal access my public EC2 and Bastion Host which was created
![priv-sg](https://github.com/morysk/Aws/blob/42b30234b29a1adaa7516384ede6d97c57d949f7/assignment%201/screenshots/private-sg.png)

## Outcome

A custom network was created containing using AWS services that contained a custom VPC with CIDR range. The network was divided into a Public Subnet and a Private Subnets to seperate resources allowing for unique configurations between them.

Correct routing for internet access within the public and private subnets was configured. The outocme resulted in two EC2 instances successfully launched to demonstrate how applications operate across public and private network layers. 

* SSH access to public EC2 instance was successfull as verified by matching private IPv4 address of `10.0.1.161`
![EC2-ssh-pub](https://github.com/morysk/Aws/blob/088428fcad1fe462cc6d51a2ddbec0096e484590/assignment%201/screenshots/ssh-pub.png)
* SSH access to Bastion Host was successfull as verified by matching private IPv4 address of `10.0.1.132`
![Bastion-Host](https://github.com/morysk/Aws/blob/e4ecc14ce3ca1bf43e53a6f44e41400e411517af/assignment%201/screenshots/ssh-bastion.png)
* SSH access to private EC2 instance via Bastion Host was successfull as verified by matching private IPv4 address of `10.0.4.195`
![EC2-ssh-priv](https://github.com/morysk/Aws/blob/e4ecc14ce3ca1bf43e53a6f44e41400e411517af/assignment%201/screenshots/priv-ssh.png)

