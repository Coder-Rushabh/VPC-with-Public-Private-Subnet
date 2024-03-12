# VPC-with-Public-Private-Subnet
VPC with public-private subnet in Production to host an application.

## About the Project

This example demonstrates how to create a VPC that we can use for servers in a production environment.
To improve resiliency, we will deploy the servers in two <code>Availability Zones</code> using an <code>Auto Scaling group</code> and an <code>Application Load Balancer.</code> For additional security, we will deploy the servers in a <code>private subnet</code>. The servers receive requests through the <code>load balancer.</code> The servers can connect to the internet by using a <code>NAT gateway</code>. To improve resiliency, we will deploy the NAT gateway in both availability zones.

<br>
  
![1_0rxpG2yXeMsqmi43x55AfQ](https://github.com/Coder-Rushabh/VPC-with-Public-Private-Subnet/assets/47267236/ae5fdef9-9864-49a1-b41d-7eb375c6e2fa)


### Overview
- The VPC has public subnets and private subnets in two Availability zones.
- Each public subnet contains a NAT gateway and a load balancer node.
- The servers run in the private subnets, are launched and terminated by using an Auto Scaling group, and receive traffic from the load balancer.
- The servers can connect to the internet by using the NAT gateway.

## Steps
### Step 1: Create a VPC
For this project, we are creating two subnets under two availability zones in the same VPC.

![image](https://github.com/Coder-Rushabh/VPC-with-Public-Private-Subnet/assets/47267236/c74c168b-b7b6-46ee-90f7-a8cda4abe408)

So, create a VPC according to the requirement.
- Name this VPC ’prodVPC’
- By default, the number of public subnets and private subnets should be two. If not set them to two.
- Set 1 NAT Gateway per AZ.
- Set VPC Endpoint to none.
- Create VPC.

![image](https://github.com/Coder-Rushabh/VPC-with-Public-Private-Subnet/assets/47267236/7abae1d4-8a8a-4a68-914e-7991bd5b5fc1) ![image](https://github.com/Coder-Rushabh/VPC-with-Public-Private-Subnet/assets/47267236/97739f22-e8ba-453b-a834-6cd042f105d7)

Launching a VPC might take some time.

![image](https://github.com/Coder-Rushabh/VPC-with-Public-Private-Subnet/assets/47267236/ba4f53e7-e5de-4f06-89fa-a2c532b0cb34)


