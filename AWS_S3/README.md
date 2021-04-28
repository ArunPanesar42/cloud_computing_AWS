# AWS S3

## What is Amazon AWS S3?
Amazon S3 or Amazon Simple Storage Service is a service offered by Amazon Web Services that provides object storage through a web service interface. Amazon S3 uses the same scalable storage infrastructure that Amazon.com uses to run its global e-commerce network
### Who is using S3 
### Configuring AWSCLI
### Getting Authentication done 
### AWS Access and Secret 
### Using a Backup to apply CRUD 

**We need running EC2 to ssh into the instance and AWS access and secret**
## commands
`aws s3 mb s3://eng84arunss3` = Makes a bucket
`aws s3 sync s3://eng84arunss3 README.md` = to download a file bucket 
`aws s3 cp README.md s3://eng84arunss3` = to copy file from local instance to s3
`aws s3 rm s3://eng84arunss3/README.md` = to remove a file 
`aws s3 rb s3://eng84arunss3` = to remove a bucket



