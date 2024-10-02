# AWS Network Architecture w/ ALB + ASG + EFS

![Untitled Diagram](https://github.com/user-attachments/assets/7aa3669c-8277-4840-b85b-9f994df55902)


# Overview

This project entails creating a scalable, reliable, secure, and highly available cloud architecture for a client bank — utilizing their own VPC while maintaining secure connection to the outside world for their clientele. With an application load balancer, an auto sorting group, apache2 webservers, and even an elastic file system — we create a cloud architecture that far exceeds that capability of classic on-premise systems.

# Challenge/Problem

The finance/banking sector can change rapidly, and businesses must either improve and adapt, or lose. We need to create a cloud infrastructure that will allow our client to keep up with client demand, market conditions, and their competitors while remaining cost-efficient and agile.

# Solution Strategy

Utilize the power of the Cloud with AWS technologies to scale seemlessly with customer demand, all while avoiding upfront costs and maintenance. We do this by creating our own VPC with 3 public subnets in different AZs, accessing the internet through an internet gateway / utilizing an application load balancer to direct inbound traffic securely to an auto scaling group, which funnels traffic privately to our webservers running on EC2 instances — which share access to files through an Elastic File System.

# Technologies/Resources Setup

Prerequisites

VPC setup with CIDR 10.10.0.0/16,
3 public subnets (10.10.1.0/24 & 10.10.2.0/24 & 10.10.3.0/24),
Routing Tables have access to internet through IGW,
User data that installs apache,
web server security group allowing HTTP from ALB,
Load Balancer security group that allows inbound traffic from HTTP from 0.0.0.0/0,
Security Groups that allow traffic from NFS to EC2 instances, and back.

# setup VPC

![vpc1](https://github.com/user-attachments/assets/c80a1ba9-7f40-4e76-ae55-3710365e55a7)

![vpc2](https://github.com/user-attachments/assets/8e1e5ca3-471d-4e84-ae63-2cb06aa11207)

![vpc3](https://github.com/user-attachments/assets/70f76cae-51ee-4204-8b1c-90239a23782a)

![vpc4](https://github.com/user-attachments/assets/cfb151e6-e6ab-474e-bf20-a6a7647cbf45)

# setup EC2

EC2 launch template create

![ec1](https://github.com/user-attachments/assets/e55f318c-2096-4357-896f-e5b578805349)

name asgtemp 

![ec2](https://github.com/user-attachments/assets/539f4633-0bd5-4f61-bc80-db69f9560678)

choose ubuntu server 

![ec3](https://github.com/user-attachments/assets/4ede8a06-340e-4653-9288-b00a01804432)

select default existing group 

![ec4](https://github.com/user-attachments/assets/9634f968-2bab-41b7-b2a0-d98b83f85a6e)

create template

```bash
#!/bin/bash
sudo apt update -y
sudo apt install -y apache2
sudo systemctl start apache2
sudo systemctl enable apache2
echo "<h1>This message is from : $(hostname -i)</h1>"> /var/www/html/index.html
```

![ec5](https://github.com/user-attachments/assets/1127b212-4f16-427d-a599-4205a74a04a8)

# Create ASG and Launch Templates with correct hardware and user data scripts


![asg1](https://github.com/user-attachments/assets/364b7baf-cb94-485e-a074-92cc6360eff0)

![asg2](https://github.com/user-attachments/assets/ed2ed4f6-7705-489a-81af-5054c433033b)

With our VPC and subnets setup with access to the internet through the IGW — we then create an Auto-Scaling Group utilizing t2.micro instances with apache2 installed as our webserver

![asg3](https://github.com/user-attachments/assets/ccf37072-dd48-4bae-9166-736db402325c)


![asg4](https://github.com/user-attachments/assets/a4ea3c8e-1385-4bd1-993a-f8e7ee2c77f9)

# After selecting our VPC and subnets, we create a new load balancer and attach it to our ASG

This will front the traffic from outside and allow our VPC to remain private.

![asg5](https://github.com/user-attachments/assets/be1b23c4-9b95-4ed2-993a-e55e4ed23eda)

![asg6](https://github.com/user-attachments/assets/e0037524-cd04-4183-bc4a-f0c93de4d644)

![asg7](https://github.com/user-attachments/assets/d4305c30-fae9-4b88-b559-8155672ff835)

![asg8](https://github.com/user-attachments/assets/81e25f3c-20be-43b5-ad80-c5ca6d74f835)

![asg9](https://github.com/user-attachments/assets/473d15e7-458d-43bb-bf7b-82296c31324f)

Finish setup of ASG — verify its working (creating fresh instances)

![asg10](https://github.com/user-attachments/assets/5c086e4a-8152-488b-a126-e53acc958b40)

![asg11](https://github.com/user-attachments/assets/f61f2fed-45a5-4f29-8b3f-1e471ac1ca48)

if you have public ip adresss  then you not  attachead public ip adressed,
becuse i have not public ip adress,
then i attach elatic ip adress.


# Attached public ip adress

![ip1](https://github.com/user-attachments/assets/ec1a86eb-c215-4cae-8709-cce039091a8a)

go to Elastic ip and assocate public ip adress

![ip2](https://github.com/user-attachments/assets/5ebe4a94-3735-4885-8d92-15a8d64658fc)

![ip3](https://github.com/user-attachments/assets/25ab2401-8820-4c40-9a3f-8f4971428668)

![ip4](https://github.com/user-attachments/assets/3bf7ddd2-ac5f-4b59-903f-10cc5b3712cf)

![ip5](https://github.com/user-attachments/assets/f695972a-d2ed-481a-8f7a-2e5115eeecdc)

# attached a inbound rule 
http (80), https (443), ssh (22)

![ec2 1](https://github.com/user-attachments/assets/2162516d-b300-4106-981d-d8ea474907dc)

![ec2 2](https://github.com/user-attachments/assets/0c9ac9f7-39f1-4348-b3a0-39636b743915)

start a  terminal 

![ec2 3](https://github.com/user-attachments/assets/1e7602c2-a9b3-47df-bdf4-de4d8ceb8810)

run command line by line 

```bash
sudo apt update -y
```

![1](https://github.com/user-attachments/assets/4eecffb2-aabc-409e-98fe-60445d69540a)

```bash
sudo apt install -y apache2
sudo systemctl start apache2
sudo systemctl enable apache2
```

![2](https://github.com/user-attachments/assets/438f4234-0609-4ef0-baf2-945708477679)

```bash
echo "<h1>This message is from : $(hostname -i)</h1>"> /var/www/html/index.html
```

![3](https://github.com/user-attachments/assets/cb402823-ab84-4179-9d5a-7c91733fab13)

# congratulation are you succes in this project 



![4](https://github.com/user-attachments/assets/3bfc3beb-ac98-4e66-ab4e-7c4181b1b749)

# Install Stress test to EC2 instance, run the stress test to push CPU util. over 50% to test Dynamic Scaling Policy


```bash
sudo apt-get install stress -y
```

![5](https://github.com/user-attachments/assets/f3c5221d-d789-4177-a69c-591d9b26d1f2)

```bash
stress --cpu 4 --timeout 10
```

![6](https://github.com/user-attachments/assets/3e12147a-540c-4c37-b036-48fadc228deb)

i did successful run 

#Result

Our Client now has a fully functional VPC that is highly available, scalable, and secure. What would have previously taken weeks and a massive amount of cost can be done in a fraction of the time while saving massively on budget.






