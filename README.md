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
*Note: For high availability, it would be best to have one bastion host in each AZ. However, for simplicity, we will only create one Bastion Host - located in Public Subnet A (as shown in the "VPC Architecture Design" at the beginning of this ReadMe*

1. Open the EC2 console
2. Click "Instances" on the left-hand panel
3. Click "Launch instances" button in the top right hand corner
4. Name the instance "Bastion Host"
5. Keep AMI as Linux, Keep architecutre 64-bit(x86), Keep instance type t2 micro (to stay within the free tier)
6. Key Pair: Select "Proceed without a key pair (Not recommended)" *In this lab we will be using the EC2 Connect to SSH into our instance and to test our architecture. Thus, we will not need a key pair here. However, if you want to further secure your instance and if use your own SSH client then a key pair will be needed.*
7. Edit Network Settings: Change the Default VPC to "Demo VPC", Change subnet to "Public Subnet A", Enable auto-assign public IP.
8. Edit Security Group (SG): Select "Create security group", name the SG "BastionHostSG", Description:"Security group for Bastion Host" (Optional), Allow SSH from anywhere, 0.0.0.0/0
9. Click "Launch instance" button at the bottom right hand corner
DONE

![image](https://user-images.githubusercontent.com/126350373/228643892-7adb9346-339f-456c-888e-04f57c7fee11.png)

![image](https://user-images.githubusercontent.com/126350373/228644143-05007220-11c9-4487-9fab-1ee2988e4f77.png)


### Step 6 - Create Private EC2 Instances

**Create Private Insance in Private Subnet A**
1. Go back to the EC2 console
2. Click "Launch instances" button in the top right hand corner
3. Name the instance "Private Instance A"
4. Keep AMI as Linux, Keep architecutre 64-bit(x86), Keep instance type t2 micro (to stay within the free tier)
5. Key Pair: Click "Create new key pair", Name the keypair "VPCKeyPair", keep everything else default and click "Create key pair" button at the bottom right hand corner - the private key pair file will be downloaded to your computer. *Make sure to store this file in a known place on your computuer. We will have to use the contents in this file  a later step*

![image](https://user-images.githubusercontent.com/126350373/228646209-628abad6-e9bc-42de-98df-25b13ff9071b.png)
 
6. Edit Network Settings: Change the Default VPC to "Demo VPC", Change subnet to "Private Subnet A", DO NOT enable auto-assign public IP (this is our private instance and it should not have a public IP address)
7. Edit Security Group (SG): Select "Create security group", name the SG "PrivateInstanceSG", Description: "Security group for private instance A and private instance B" (Optional), Allow SSH from Custom: Select the SG of the Bastion Host
8. Click "Launch instance" button at the bottom right hand corner
DONE

![image](https://user-images.githubusercontent.com/126350373/228650676-6fef5274-ee49-4a5e-81b4-f71bfff74e95.png)

![image](https://user-images.githubusercontent.com/126350373/228650998-9eaed998-3688-4101-8a93-433f6393e7e3.png)



**Create Private Instance in Private Subnet B**
1. Go back to the EC2 console
2. Click "Launch instances" button in the top right hand corner
3. Name the instance "Private Instance B"
4. Keep AMI as Linux, Keep architecutre 64-bit(x86), Keep instance type t2 micro (to stay within the free tier)
5. Key Pair: In the dropdown select the keypair "VPCKeyPair"
6. Edit Network Settings: Change the Default VPC to "Demo VPC", Change subnet to "Private Subnet B", DO NOT enable auto-assign public IP (this is our private instance and it should not have a public IP address)
7. Edit Security Group (SG): Click "Select existing security group", Select the "PrivateInstanceSG" SG, 
8. Click "Launch instance" button at the bottom right hand corner
DONE

![image](https://user-images.githubusercontent.com/126350373/228653364-1bd7f61d-cca5-432f-87a7-a255d47d0a6f.png)

![image](https://user-images.githubusercontent.com/126350373/228653516-ca811480-d6da-4d5e-b364-a01ea850e147.png)

## Test System
We are going to test our infrastructure to make sure we can properly SSH into our Bastion host and our private EC2 instances. We will also make sure our Bastion host and our private instances have access to the internet via the route tables, the internet gateway, and the NAT gateway (private instances only)

### Step 7 - SSH into Bastion Host and into Private Instances to Test Connectivity

**1. SSH into our Bastion Host using EC2 Connect**

*EC2 connect is an AWS feature that allows us to easily and securely SSH into our instances without the need of an external SSH client like Putty*

1. Open the EC2 console
2. Select the "Bastion Host"
3. Click the "Connect" button at the top right of the page

![image](https://user-images.githubusercontent.com/126350373/228654938-26393ee3-a861-44d9-b157-eb8d86e04324.png)

4. Click "Connect" button at the bottom right of the screen

![image](https://user-images.githubusercontent.com/126350373/228655079-98275ea2-3720-4114-9457-d257fdaf2e2b.png)

5. If you get a screen like the one above then you have successfully SSH into your Bastion host
DONE

![image](https://user-images.githubusercontent.com/126350373/228655564-2a6b359a-5453-46c0-8836-cc66973d7d39.png)

**2. Test if the Bastion Host has access to the internet**
1. Type "ping www.google.com" and hit Enter into the terminal window. You should receive feedback as shown in the screen shot below.
2. Make sure to hold Ctrl and press "C" to stop the ping.
3. If your outcome is similar to what's shown below then your Bastion host has access to the internet
DONE

![image](https://user-images.githubusercontent.com/126350373/228657219-8beb58e7-3770-45d0-96d4-1466dbff39f8.png)

**3. SSH into Private Instance A from our Bastion Host**
1. Type "nano VPCKeyPair.pem" and hit Enter (nano command allows us to create and store a text file within the terminal. We need to upload our VPCKeyPair so we can reference it when we SSH into the Private Instance)
2. Go to the VPCKeyPair.pem file saved on your computer. Open it. Copy ALL the content. Paste it into the terminal (hint hold ctrl and shift and press "v")

![image](https://user-images.githubusercontent.com/126350373/228658604-c20e78ed-3e44-46a1-a43c-dd21b656a489.png)

3. Hold ctrl and press "X" to exit. Save the content by press Y for yes. Then press "Enter".

*Our uploaded VPCKeyPair.pem text file is formated for others to access it (current chmod access code is 644 - "chmod 644"). It is required that your private key files are NOT accessible by others. Therefore, we must change the access permissions for only us to have access ("chmod 400"). For that we will use the "chmod" command*

4. Type "chmod 400 VPCKeyPair.pem" and press Enter. This remove access for others and only allows read access to us. 

*Now we are ready to SSH into our Private Instance A via our Bastion host by using our keypair*

5. In a seperate tab, go to the EC2 console

6. Select "Private instance A" and copy the private IP address as shown in the screenshot below. We will need to reference this IP address to SSH into it. Go back to the EC2 Connect Window terminal.

![image](https://user-images.githubusercontent.com/126350373/228661710-5ff5a683-f441-4456-91f3-460760ac8da1.png)

*Now we are ready to SSH into our Private Instance A via our Bastion host by using our keypair*

7. Type "ssh ec2-user@INSERT YOUR PRIVATE IP ADDRESS YOU JUST COPIED -i VPCKeyPair.pem". Hit Enter.  *As an example Your code should look like this (your IP address will be different): "ssh ec2-user@10.0.19.224 -i VPCKeyPair.pem"*
8. A prompt will come up asking if your are sure you want to connect. Type "yes" and you should have successfully SSH into your Private Instance A via your bastion host.
DONE

![image](https://user-images.githubusercontent.com/126350373/228663065-d23be228-e87d-4fa1-af66-ab0c02be8b09.png)

**4. Test if Private Instance A has access to the internet**
1. Type "ping www.google.com" and hit Enter into the terminal window. You should receive feedback as shown in the screen shot below.
2. Make sure to hold Ctrl and press "C" to stop the ping.
3. If your outcome is similar to what's shown below then your Bastion host has access to the internet
DONE

![image](https://user-images.githubusercontent.com/126350373/228663491-56e7dff6-bd56-4a0b-81f8-7c9f3d9d4385.png)

**Repeat the Steps 3-4 for Private Instance B if you want to SSH into it and if you want to test if it has access to the internet.** *NOTE: Remember to use the correct private IP address when executing the "ssh ec2-user@10.0.19.224 -i VPCKeyPair.pem" command*


## Clean Up

### Step 8 - Delete the VPC
