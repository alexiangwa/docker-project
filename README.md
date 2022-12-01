# docker-project
amazonlinux is the base image of the docker image
use "docker pull amazonelinux" to pull the image from dockerhub 

the command below is similar to the one on dockerfile
FROM amazonlinux:latest

#!/bin/bash
sudo su
yum update -y
yum install -y httpd
cd /var/www/html
wget https://github.com/azeezsalu/jupiter/archive/refs/heads/main.zip
unzip main.zip
cp -r jupiter-main/* /var/www/html/
rm -rf jupiter-main main.zip
systemctl enable httpd 
systemctl start httpd