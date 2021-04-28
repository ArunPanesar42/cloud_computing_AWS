# AWS VPC configuration with Subnets 
## Step 1
### Creating the VPC
1. Click Your VPCs, then Create VPC
2. Change the VPC nametag to Eng84_william_vpc
3. Change IPv4 CIDR block to 0.0.0.0/16 where the first 2 numbers are unique e.g. 59.84.0.0/16
4. Click Create VPC

## Step 2
### Creating an Internet Gateway to Attach our VPC
1. Click Internet Gateways, then Create internet gateway
2. Change the nametag to Eng84_william_ig
3. Click Create Internet Gatway
4. Click Actions, then Attach to VPC
5. Select the VPC you created, then click Attach internet gateway

## Step 3 
### Creating Subnets
1. Click Subnets, then Create subnet
2. Select your VPC
3. Add the Subnet name as Eng84_william_public_subnet
4. Availability zone to 1c, but No preference is fine
5. IPv4 CIDR block to 59.84.1.0/24 as per the VPC IP
6. Click Create Subnet
7. Repeat the above steps for the Private Subnet, but with the applicable name and the third number of the IPv4 CIDR block must be unique.
