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

<hr>

### Step 2: Create Auto Scaling Group

Before creating an Auto Scaling Group, we need to create a Launch Template.
- Select Ubuntu OS Image with default configuration.
- Don't select subnet.
- Create a security group.
  
  ![image](https://github.com/Coder-Rushabh/VPC-with-Public-Private-Subnet/assets/47267236/e07714f7-4d54-4a69-a8d4-f5e1c99a599a)

- Create Launch template.

Search for EC2 and in the EC2 dashboard scroll down on the left-hand side there will be the option for ‘Auto Scaling Group’.
- Now, Launch an Auto Scaling Group with our template.
- In the network setting, Select a VPC and private subnets.
  
  ![image](https://github.com/Coder-Rushabh/VPC-with-Public-Private-Subnet/assets/47267236/884145da-87b1-47c2-b0b3-8ceb2091c5ea)

- We will not create a Load balancer now, will create it later.
- We specify Desired, Min, and Max capacity as 2, 1, and 4 respectively.

Before creating the Application Load Balancer, the application need to be installed on the servers. In order to do that we will ssh into the instances and install it.
In order to do that we must first create a Bastion Host as we did not provide our instances with a public IP address.

<hr>

### Step 3: Create a Bastion host

Till now, our two EC2 instances are running in each private subnet. They will not have a public IP. So to communicate with them we will create a Bastion host.

A bastion host is a server whose purpose is to provide access to a private network from an external network, such as the Internet.

Launch manually an EC2 instance with Ubuntu OS.

![image](https://github.com/Coder-Rushabh/VPC-with-Public-Private-Subnet/assets/47267236/ee9ce5a4-e1bf-4c84-838c-bc0c7c5d8618)

<hr>

### Step 4: SSH into the Bastion Host

We are going to communicate to instances in a private subnet through the Bastion host. For that, we will need a key-pair file (.pem file) in the Bastion host. 
To copy the .pem file from our PC to the Bastion host run the following command in CMD.

![image](https://github.com/Coder-Rushabh/VPC-with-Public-Private-Subnet/assets/47267236/4ba9da7e-3b35-4e12-b801-fa971d07f82f)


Now log into bastion host and run the command:

![image](https://github.com/Coder-Rushabh/VPC-with-Public-Private-Subnet/assets/47267236/31d00bfe-88b1-4d49-8905-82a951cab7ae)

Check whether the .pem file is successfully copied into bastion host by entering <code>ls</code>

Now to log into the private instance, enter the following command into the Bastion host

``` ssh -i bastion.pem ubuntu@<private IP> ```

<b>Note:</b> If you get a prompt saying <code>Unprotected Private Key File </code> it means that the permission to the file is to open and that needs to be changed.
To do this run:

```chmod 400 instancelogin.pem```

#### We are Logged In

<hr>

### Step 4: Create a Web Page

1. First create an html page by running

```vim index.html```

2. Create a simple HTML file here.

3. Launch the Python server by running

```python3 -m http.server 8000```

The application is now running in one of the instances in the private subnet.


<hr> 

### Step 5: Create an Application Load Balancer

Now we only logged into one instance as we are using the load balancer to demonstrate that traffic is going to only one instance and we are receiving the response, whereas if it goes to another instance we will get an error response as the application is not available there.

Go to the EC2 Dashboard in the AWS Console and select Load Balancer.

- Make it Internet-facing and in both public subnets.

- Create a target group on port 8000.

## Conclusion:
Our Website is running on a private subnet on port 8000.

We can access our website by hitting on the DNS address of our Load Balancer, which is accepting traffic on port 80.

It will redirect traffic from port 80 to port 8000.













