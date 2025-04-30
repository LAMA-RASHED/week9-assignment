# Clarusway AWS Web App Deployment - Week 9 Assignment

### SDA2037-Lama Rashed

## Goal
Deploy a static website on AWS using:
- S3 bucket for static files
- Auto Scaling Group (ASG) for NGINX servers
- Application Load Balancer (ALB) to distribute traffic

---

## What I Did

### Part 1: S3 Static Website
- I created a bucket called lama-clarusway-assets in eu-north-1
- I uploaded these files:
  - index.html
  - logo.png
  - sda.png
- I enabled static website hosting in the bucket settings
- I added a bucket policy to make the files public
- I tested the S3 link in the browser and with curl -I, and it worked

---

### Part 2: Auto Scaling Group
- I created a launch template with Amazon Linux 2 and t3.micro instance type
- I created an IAM Role with AmazonS3ReadOnlyAccess and attached it to the template
- I added this user-data script to install NGINX and copy the index.html from S3:

```bash
#!/bin/bash
yum update -y
yum install nginx -y
systemctl start nginx
systemctl enable nginx
aws s3 cp s3://lama-clarusway-assets/index.html /usr/share/nginx/html/index.html
