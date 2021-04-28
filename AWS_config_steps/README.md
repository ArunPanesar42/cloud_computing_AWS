# AWS VPC configuration with Subnets 
## Step 1
### Creating the VPC
1. Click `Your VPCs`, then `Create VPC`
2. Change the VPC nametag to `Eng84_arun_vpc`
3. Change IPv4 CIDR block to `0.0.0.0/16` where the first 2 numbers are unique e.g. `59.84.0.0/16`
4. Click `Create VPC`

## Step 2
### Creating an Internet Gateway to Attach our VPC
1. Click `Internet Gateways`, then `Create internet gateway`
2. Change the nametag to `Eng84_Arun_ig`
3. Click Create `Internet Gatway`
4. Click `Actions`, then `Attach to VPC`
5. Select the VPC you created, then click `Attach internet gateway`

## Step 3 
### Creating Subnets
1. Click `Subnets`, then `Create subnet`
2. Select your VPC
3. Add the Subnet name as `Eng84_arun_public_subnet`
4. Availability zone to `1c`, but `No preference` is fine
5. IPv4 CIDR block to `59.84.1.0/24` as per the VPC IP
6. Click `Create Subnet`
7. Repeat the above steps for the Private Subnet, but with the applicable name and the third number of the IPv4 CIDR block must be unique.

## Step 4
### Setting up the Route Tables 
1. Go on `Routing Tables`
2. Click on the unnamed route tables until you find the one with your VPC
3. Rename it to `Eng84_YourName_public_rt`
4. With the route table selected, select the `Routes tab`
5. Click `Edit routes` and do the following:
    - Set the destination to `0.0.0.0/0`
    - Set the target to `Internet Gateway`, then select your internet gateway
    - `Save` the configurations
6. Select the `Subnet Associations` tab to link the Public Subnet
7. Click `Edit subnet associations` and do the following:
    - Select the public subnet you have created
    - Click `Save`
  
### Creating a separate route table for the private subnet.**
1. Click `Create route table`, then do the following:
    - Set the Name tag to `Eng84_YourName_private_rt`
    - Select your VPC
    - Click `Create`
    - NOTE: This route table will not be connected to the internet.
2. With the new route table selected, select the `Subnet Associations` tab
3. Click `Edit subnet associations` and do the following:
    - Select the "PRIVATE" subnet you have created
    - Click `Save`

## Step 5 
### Creating the EC2 instances
1. Click `Launch Instance`
2. Choose `Ubuntu Server 16.04 LTS (HVM), SSD Volume Type` as the Amazon Machine Image (AMI)
3. Choose `t2.micro` as the instance type (the default)
4. On Configure Instance Details:
    - Change the VPC to your VPC
    - Change subnet to your public subnet
    - Enable `Auto-assign` Public IP for the app
5. Skip `Add Storage` (keep defaults)
6. Add a tag with the `Key` as `Name` e.g `eng84_arun_app`
7. Security group name should be `Eng84_arun_app_sg` for the app and add the SSH rules:
    - Set to Port 22
    - Source: My IP
    - Description: My IP
8. Add another rule for HTTP:
    - Set to Port 80
    - Custom Source: 0.0.0.0/0, ::/0 (default)
9. Review and Launch
10. Select the existing DevOpsStudent key:pair option for SSH

### Creating an instance for the database.
1. Repeat steps 1 to 3 from the above
2. Repeat step 4, but change the subnet to your private one instead.
3. Repeat steps 5 and 6. Adjust the name applicable for step 6.
4. Repeat step 7, but replace the HTTP rule for All traffic:
      - Custom Source: the `public security group`
      - This only allows the app to access the database
5. Repeat steps 8 and 9 as normal

## Step 6 
### Connecting to the EC2 instances
1. Go to the directory before the app folder.
2. Execute this command: ````scp -i ~/.ssh/DevOpsStudent.pem -r app/ ubuntu@app_ec2_public_ip:~/app/````
3. Change to the ```~/.ssh````
4. Click `Connect`, then run the SSH command given from the `SSH client` instructions
5. If you are asked for a finger print, type yes.
6. Create the provisions file and copy, paste the contents. Adjust the directories as needed (hint: `use pwd`).
7. Run the provision file using `./provision_file_name.sh`. Change permissions with `chmod` if needed.
8. Run the environment variable command, so the app can connect to the database: ```echo "export DB_HOST=mongodb://db_private_ip:27017/posts" >> ~/.bashrc```
9.Do the following if you want to apply the reverse proxy manually:
    - Execute: ```sudo nano /etc/nginx/sites-available/default```
    - Replace it all with the code below:
```` 
        server {
            listen 80;

            server_name _;

            location / {
                proxy_pass http://localhost:3000;
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection 'upgrade';
                proxy_set_header Host $host;
                proxy_cache_bypass $http_upgrade;
            }
        }
 ````

### Connecting to the database instance

1. The database is not connected to the internet, so a proxy SSH is required.
2. First, we need the Public IPv4 address of our app.
3. Next, we need the Private IPv4 address of our database.
4. Execute this command to SSH into the database: ````ssh -i ~/.ssh/DevOpsStudent.pem -o ProxyCommand="ssh -i ~/.ssh/DevOpsStudent.pem -W %h:%p ubuntu@app_public_ip" ubuntu@db_private_ip````

## Step 7
### Updating the database instance
1. First, we have to modify the security group:
    - Go on `Security`, then click the security group
    - Click `Edit inbound rules`
    - Add a HTTP rule and set the Source to `Anywhere`
  
2. Next, go to the VPC to modify the route table subnet associations:
    - Click `Route Tables`
    - Select your public route table
    - With your public route table selected, click the `Subnet Associations` tab
    - Edit the subnet associations, then select the private subnet.
3. The database instance is now accessible to the internet.
4. SSH into the database instance just like your app instance.
5. Create the provisions file and copy, paste the contents.
6. Run the provisions file
7. With the operations complete, remove the private subnet from the public route table.
8. Remove the HTTP rule from the security group.
9. Reconnect (SSH) into both instances.
    - NOTE: for the app, one will need to run `seed.js`

## Step 8
### Creating a Public NACL for the VPC
1. Ensure that you are in the VPC section (not EC2), we then:
    - Go to the `Network ACLs` section under `Security`
    - Click `Create network ACL`
    - Add the appropriate name and allocate the VPC, then create it
  
2. Now we set the inbound rules for the NACL:
    - With the NACL selected, click on the `Inbound rules` tab
    - Click `Edit inbound rules`
    - Remove the default rule and add the following rules:
        - 100: HTTP (80) with source `0.0.0.0/0` - this allows external HTTP traffic to enter the network
        - 110: SSH (22) with source `your_IP_address/32` - allows SSH connections to the VPC
        - 120: Custom TCP with Port range `1024-65535` and source `0.0.0.0/0` - allows inbound return traffic from hosts on the internet that are responding to requests originating in the subnet
        - NOTE: All rules should be Allow rules

3. Now, lets set the outbound rules:
    - Select the Outbound rules tab
    - Click Edit outbound rules
    - There should be the following rules:
        - 100: HTTP (80) with source 0.0.0.0/0
        - 110: Custom TCP with source private subnet CIDR block (59.84.2.0/32 in this case) and port 27017 for outbound access to our MongoDB server in the private subnet
        - 120: Custom TCP with port 1024-65535 and source 0.0.0.0/0 - allow short lived ports between 1024-65535
        - NOTE: All rules should be `Allow` rules

## Step 9 
### Creating a Private NACL for the VPC

1. With the NACL selected, click on the `Inbound rules` tab
2. Click `Edit inbound rules`
3. Remove the default rule and add the following rules:
    - 100: Custom TCP with source public subnet CIDR block (`59.84.1.0/32` in this case) - this allows the app subnet to access the database subnet
    - 110: SSH (22) with source your_IP_address/32 - allows SSH connections to the VPC
    - NOTE: All rules should be `Allow` rules
  
4. Now, lets set the outbound rules.
    - Select the `Outbound rules` tab
    - Click `Edit outbound rules`
    - There should be the following rules:
            - 100: All traffic with source `0.0.0.0/0` - allow all the traffic out
            - NOTE: All rules should be `Allow` rules

## Step 10
### Assigning Subnets to NACLs
1. Select the `Subnet associations` tab
2. Select the `Edit subnet associations` tab
3. Select the public/private subnet, depending on the NACL

## AWS VPC Flow Diagram
**Diagram:**
![VPC_Pic](https://github.com/ArunPanesar42/cloud_computing_AWS/blob/main/Images/AWS_Diagram.png?raw=true)

