# create-an-AWS-vpc-from-scratch

## Lab Overview and Highlevel

In this lab we will create a VPC from scratch in the AWS console using services/components like subnets, EC2 Instances, internet gateways, NAT gateways, etc. The VPC architecture that will be built in this lab is shown below.

**VPC Architecture Design**

![1  Serverless Microservices Architecture - VPC](https://user-images.githubusercontent.com/126350373/228558066-e55286c3-f72f-4d57-8e30-ee61ec53e3a4.png)

## Setup

### Step 1 - Create the VPC

1. Open the VPC console
2. Click on "Your VPCs" on the left-hand panel
3. Click "Create VPC" button in the top right hand corner
4. Name the VPC "Demo VPC"
5. Set the IPv4 CIDR block to 10.0.0.0/16 (that's 65,536 IP Addresses we are assigning to our VPC)
6. Leave everything the same and click "VPC" at the bottom right hand corner.
DONE

![image](https://user-images.githubusercontent.com/126350373/228560127-3469a2eb-caf1-4a3e-8667-68f244eea459.png)

**PRO TIP: Filter your VPC console with the "Demo VPC" we just created. This hides components already created for your default VPC. Shown in screenshotbelow.**
![image](https://user-images.githubusercontent.com/126350373/228577852-c2697594-d1cb-485e-8690-30749c14dd9a.png)

### Step 2 - Create the Subnets

**We are creating 4 subnets - 2 Public Subnets in separate Availability Zone (AZ) and 2 Private Subnets in the same separate AZs**

1. Open the VPC console
2. Click on "Subnets" on the left-hand panel 
3. Click "Create subnet" button in the top right hand corner
4. Click the drop down and select the VPC named "Demo VPC" that we created in Step 1
5. Name Subnet 1 "Public Subnet A"
6. Choose an AZ "a" for whatever region you are within. For example: "us-east-1a"
7. Set the IPv4 CIDR block to 10.0.0.0/24 [10.0.0.0 - 10.0.0.255] (that's 256 IP addresses)
8. Then click the button "Add new subnet" at the bottom left hand corner. We will complete similar steps shown in 5-7 for the other 3 subnets we will create.
9. Name Subnet 2 "Public Subnet B", Choose AZ "b", Set the IPv4 CIDR block to 10.0.1.0/24 [10.0.1.0 - 10.0.1.255], click "Add new subnet" at the bottom left hand corner
10. Name Subnet 3 "Private Subnet A", Choose AZ "a" (same AZ as Subnet 1), Set the IPv4 CIDR block to 10.0.16.0/20 [10.0.16.0 - 10.0.31.255] (that's 4096 IP addresses), click "Add new subnet" at the bottom left hand corner
11. Name Subnet 4 "Private Subnet B", Choose AZ "b" (same AZ as Subnet 2), Set the IPv4 CIDR block to 10.0.32.0/20 [10.0.32.0 - 10.0.47.255]
12. Click the "Create subnet" button in the bottom right hand corner.
DONE

**Subnet 1 and 2**

![image](https://user-images.githubusercontent.com/126350373/228568919-d8ac38b8-33ab-427b-903f-e90003074ef6.png)

**Subnet 3 and 4**

![image](https://user-images.githubusercontent.com/126350373/228569344-369ecf53-c992-4db2-b68a-da67840facbf.png)



### Step 3 - Create Internet Gateway and Route Tables

**Create Internet Gateway**
1. Open the VPC Console
2. Click on "Internet gateways" on the left-hand panel 
3. Click "Create internet gateway" button in the top right hand corner
4. Name the internet gateway "Demo Internet Gateway"
5. Click "Create internet gateway" in the bottom right hand corner.
DONE

![image](https://user-images.githubusercontent.com/126350373/228571396-7f1b9538-db67-4a0f-a1bd-d9a241e35e9e.png)

**Attach the Internet Gateway to "Demo VPC"** 
1. Go back to the internet gateway dashboard
2. Select the newly created internet gateway called "Demo Internet Gateway"
3. Click the "Actions" dropdown button at the top right hand corner and click "Attach to VPC"
![image](https://user-images.githubusercontent.com/126350373/228572668-5a759b37-9a02-481c-9d49-76c87df9dd3c.png)

4. Select the "Demo VPC" in the drop down and click "Attach internet gateway" button at the bottom right hand corner
DONE
![image](https://user-images.githubusercontent.com/126350373/228572859-53862fbb-88b3-4398-b429-f4fe1fd78b4c.png)

**Create the Route Tables**

*A default route table has already been created for our created VPC. However, we will not use the default route table associated with our VPC. **We will 3 route tables of our own (Public Route Table, Private Route Table A, Private Route Table B)***

1. Open the VPC Console
2. Click on "Route tables" on the left-hand panel 
3. Click "Create route table" button in the top right hand corner
4. Name the 1st route table "Public Route Table", select "Demo VPC", click "Create route table" button at the bottom right hand corner. 

**Public Route Table**
![image](https://user-images.githubusercontent.com/126350373/228575580-9ef0b433-35bf-4e05-b3a5-b06964121ce1.png)

5. Name the 2nd route table "Private Route Table A", select "Demo VPC", click "Create route table" button at the bottom right hand corner. 

**Private Route Table A**
![image](https://user-images.githubusercontent.com/126350373/228576235-6307c5b9-154e-4c56-a105-41fd677e6759.png)

6. Name the 3rd route table "Private Route Table B", select "Demo VPC", click "Create route table" button at the bottom right hand corner. 
DONE

**Private Route Table B**
![image](https://user-images.githubusercontent.com/126350373/228576825-affc7e25-31ce-4033-9ce6-ecb704cbe421.png)

**Assign Subnets to Route Tables**

*Assign Public Subnets to "Public Route Table"*
1. Go back to the route tables dashboard
2. Select "Public Route Table"
3. Click the "Actions" dropdown button at the top right hand corner and click "Edit subnet associations"
![image](https://user-images.githubusercontent.com/126350373/228581548-563cfc33-6e07-4158-ad37-3e7e15c44518.png)

4. Select "Public Subnet A" and "Public Subnet B" then click "Save associations" button in the bottom right hand corner
![image](https://user-images.githubusercontent.com/126350373/228582272-5957adcf-7001-4891-bd67-01d06a10f422.png)

*Assign Private Subnet to "Private Subnet A"*
1. Go back to the route tables dashboard
2. Select "Private Route Table A"
3. Click the "Actions" dropdown button at the top right hand corner and click "Edit subnet associations"
4. Select "Private Subnet A" and click "Save associations" button in teh bottom right hand corner
![image](https://user-images.githubusercontent.com/126350373/228583957-621094a0-f012-4980-ad66-358a97d9b10c.png)

*Assign Private Subnet to "Private Subnet B"*
1. Go back to the route tables dashboard
2. Select "Private Route Table B"
3. Click the "Actions" dropdown button at the top right hand corner and click "Edit subnet associations"
4. Select "Private Subnet B" and click "Save associations" button in teh bottom right hand corner
DONE
![image](https://user-images.githubusercontent.com/126350373/228584165-9695f2f3-ef3a-487f-ab2a-92e7a639685a.png)

**Update Public Route Table to make "Public Subnet A" and "Public Subnet B" Public**
1. Go back to the route tables dashboard
2. Select "Public Route Table"
3. Click the "Actions" dropdown button at the top right hand corner and click "Edit routes"
4. Click "Add route", for Destination route select "0.0.0.0/0", for Target select "Internet Gateway" and select "Demo Internet Gateway", click "Save changes" button at the bottom right hand corner.
DONE

![image](https://user-images.githubusercontent.com/126350373/228596996-ff9e569a-b014-44fc-8b93-b569f106ba4a.png)


### Step 4 - Create NAT Gateway (NATGW)

**For high availability we will create 2 NATGW. One in each AZ (One in Public Subnet A and One in Public Subnet B)**
1. Open the VPC Console
2. Click on "NAT gateways" on the left-hand panel 
3. Click "Create NAT gateway" button in the top right hand corner
4. Name the 1st NATGW "NATGW A", Select "Public Subnet A" that we created in a previous step, click "Allocate Elastic IP" button, and click "Create NAT gateway" button in the bottom right hand corner

![image](https://user-images.githubusercontent.com/126350373/228589852-71cb71e1-3edb-4149-af80-899e10deed72.png)

5. Go back to the NATGW dashboard and click "Create NAT gateway" button in the top right hand corner
6. Name the 2nd NATGW "NATGW B", Select "Public Subnet B" that we created in a previous step, click "Allocate Elastic IP" button, and click "Create NAT gateway" button in the bottom right hand corner
DONE

![image](https://user-images.githubusercontent.com/126350373/228591148-56c2e540-32b6-42cf-93c3-b36fc178eb65.png)

**Connect Route Tables to NATGW**
1. Open the VPC Console
2. Click on "Route tables" on the left-hand panel 
3. Select "Private Route Table A"
4. Click the "Actions" dropdown button at the top right hand corner and click "Edit routes"
5. Click "Add route", for Destination route select "0.0.0.0/0", for Target select "NAT Gateway" and select "NATGW A", click "Save changes" button at the bottom right hand corner.

![image](https://user-images.githubusercontent.com/126350373/228594577-b4956203-547e-4761-8d27-cd6ad636d462.png)

6. Go back to the Route tables dashboard
7. Select "Private Route Table B"
8. Click "Actions" dropdown button at the top right hand corner and click "Edit routes"
9. Click "Add route", for Destination route select "0.0.0.0/0", for Target select "NAT Gateway" and select "NATGW B", click "Save changes" button at the bottom right hand corner.
DONE

![image](https://user-images.githubusercontent.com/126350373/228595882-30cdb64f-ee69-427b-beb9-b2c67e6c7df9.png)

### NOTE: The default NACL allows all inbound and all outbound. We will not change the default NACL settings in this lab. Found in VPC console.

**NACL Inbound Rules**

![image](https://user-images.githubusercontent.com/126350373/228600720-8b890a6d-c46c-4625-967f-b6a6d439e411.png)

**NACL Outbound Rules**

![image](https://user-images.githubusercontent.com/126350373/228600965-4d9aef6e-e43e-409b-bbdc-e8f98bb7e3ce.png)



### Step 5 - Create Bastion Host

### Step 6 - Create Private EC2 Instances

## Test System

### Step 7 - SSH into Bastion Host and into Private Instances to Test Connectivity

## Clean Up

### Step 8 - Delete the VPC
