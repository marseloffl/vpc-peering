# VPC Peering : Connecting Two EC2 Instances Using VPC Peering Connection.

## Aim:
* To Create Two EC2 machine with two different VPC Configurations and connecting it together using VPC Peering in AWS Cloud.

![VPC-Peering-Architecture](https://drive.google.com/uc?export=view&id=1zl8u58woqhkB98BIO_sOcs40_IttRkLW)

## Step 1. Creating VPC
* We need two VPC, we already have one VPC in ```172.31.0.0/16``` CIDR and  all configurations done in default. So we will take that and you name it as ```default-VPC``` if you need.
![default-vpc](https://drive.google.com/uc?export=view&id=1QEfzF-GSi2oZIhTJ7d1iW5QpQqkXvOQP)
* Let's Create another VPC ```test-vpc``` in ```192.168.0.0/16``` CIDR.

  1. Click Create VPC. Provide any name and CIDR Range and Click Create VPC
  ![VPC2Name-CIDR](https://drive.google.com/uc?export=view&id=1UJkojTriA5F3ApyRM1_DBNiyXt5MC6D2)
  
     ### Create Subnet :
  2. Next Create New Subnet for ```test-vpc```. We have already some default subnets for our default vpc, we should not delete or change it. Click Create subnet and choose test-vpc and create as you wish. I will Create one subnet for test purpose and name it as ```public-subnet-1a```, you can create any multiple subnet according to your location availability zone
  ![public-subnet](https://drive.google.com/uc?export=view&id=1lXUPgkjpMbGDDVhwJhP7C23wSqUpLgKM)

     ### Create Internet Gateway :
  3. Name it as ```test-igw``` and click Create internet gateway 
  ![NewInternetGateway](https://drive.google.com/uc?export=view&id=1uqaid5DjmCuAqO1bpmCye7eP_rWJNp_Q)
    - 3.1 Attach IGW to test VPC
    ![AttachIGWToVPC](https://drive.google.com/uc?export=view&id=1XaUVtOPPX8D70HkIwgUviIwFLw798uw5)

      ### Create Route Table :

  4. Name it as ```test-demo-rt```, select test vpc and click create route table
  ![RouteTable](https://drive.google.com/uc?export=view&id=1D5kXbFvhgMtS5o6WtUIYRipyNfsP5MDo)  
  - 4.1 Click ```test-demo-rt``` and edit route and attach the newly created internet gateway.
  ![RouteIGW](https://drive.google.com/uc?export=view&id=1Qz5PyGQhS-R2VvQ-glsWlJJi2Vs3mKpS)
  - 4.2 Click on Subnet Association and Associate the created ```public-subnet-1a``` subnet and click save association.
  ![SubnetAssociation](https://drive.google.com/uc?export=view&id=1EJyd758AcS2BvP_vICQYE_JJJqBWn9VU)


## Step 2. Creating EC2 Instances
* First Instance : My First Machine will run on Ubuntu ami, ```ap-south-1b``` AZ and ```default-vpc``` VPC. I named it as Ubuntu1
![Ubuntu1-Networking](https://drive.google.com/uc?export=view&id=1UpykkGjzGjX6PL4ARUcxAXKY856Rv4hO)
* Second Instance : This Machine will run on Ubuntu ami, ```ap-south-1a``` AZ and ```test-vpc``` VPC. I named it as Ubuntu2
![Ubuntu2-Networking](https://drive.google.com/uc?export=view&id=1i82DDE-9M83BvNBkXpKhr4LW5QWfX6Gr)
* Now Two Instances are Launched and running in two Different VPC & AZ successfully.
![RunningInstances](https://drive.google.com/uc?export=view&id=1AhfQiaHUMFsqiXmqxb2AtQhZ11opt08z)

## Step 3. Create VPC Peering

* Click Peer Connection on VPC Service, Click Create peering connection.
* Name it as ```test-vpc-peering``` and select the requester vpc and accepter vpc
![VPCPeeringSeeting](https://drive.google.com/uc?export=view&id=1JFnxZeiBhM2Tk1PmOf0xKzx0hjXG609Y)
* Accept Peering Request
![AcceptPeeringRequest](https://drive.google.com/uc?export=view&id=1SmDLeoy0v5wFQ3SqIpYl0mX6LRT5m6JG)

* Add test-vpc's CIDR range on default-vpc route table and select peering connection on tagret then add assigned peering connection.
![DVPC-CIDR-Route](https://drive.google.com/uc?export=view&id=1TsJdsU4-t6zT-XFeRoEspI_CEsorvSMO)

* Add default-vpc's CIDR range on test-vpc route table and select peering connection on tagret then add assigned peering connection.
![TVPC-CIDR-Route](https://drive.google.com/uc?export=view&id=1W0CGwaSeMa7zqDNMqu-2S_BVfbaiK_ij)

## Step 4. Check the Connection between two instances.
* Change Hostname using below command so that it won't confuse which machine are you using.
```
sudo hostnamectl set-hostname Ubuntu1
```
* SSH and login into Ubuntu1 Machine and Ping Ubuntu2's Public IP Address and Private IP Address
![VPCPeeringConnectionCheck](https://drive.google.com/uc?export=view&id=1v43SUYvdNT7YKnZ-beS-bpIO-ieZCOw-)

* Yes connected successfully!!! I can connect with both public and private ip address.

* After All Demonstration Delete your VPC Peering and other stuffs which your created newly and be careful don't delete the default VPC Configurations.

## FAQ
### 1. Is VPC Peering Service is free ?
Yes but only if the Availabilty Zone is same. If different AZ then AWS gonna charge you.

### 2. What are the Chances of Error ?
The important things you should be careful while setting Route Table Configuration and Security group. Subnet must be associated with Route table where the EC2 instance was launched.

### 3. Can we connect different AWS Account in VPC Peering ?
Yes, account number/id is required while configuring and the person from other side must accept the peering connection.