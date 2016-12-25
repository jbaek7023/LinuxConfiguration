# LinuxConfiguration

1. IP address: http://35.166.123.172/
2. SSH:
```
ssh -i ~/.ssh/[RSA file] root@35.166.123.172
```

## The complete URL
[http://35.166.123.172/](http://35.166.123.172/) 
or 
[http://ec2-35-166-123-172.us-west-2.compute.amazonaws.com](http://ec2-35-166-123-172.us-west-2.compute.amazonaws.com)

## Summary of Linux Configuration
1. Launch VM of udacity account
2. SSH into the server
3. Create a new user named grader
4. Give the grader the permission to sudo
5. Update all currently installed packages
6. Change the SSH port from 22 to 2200
7. Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123)
8. Configure the local timezone to UTC
9. Install and configure Apache to serve a Python mod_wsgi application
10. Install and configure PostgreSQL:
11. Do not allow remote connections
12. Create a new user named catalog that has limited permissions to your catalog application database
================
### 2. SSH into the server 
```
ssh -i ~/.ssh/[RSA file] root@35.166.123.172
```
### 3. Create a new user named grader

1. ``sudo adduser grader``

### 4. Give the grader the permission to sudo
1. ``sudo nano /etc/sudoers.d/grader``
2. add ``"grader ALL=(ALL:ALL) ALL"`` to the file. 

### 5. Update all currently installed packages
``sudo apt-get update``
``sudo apt-get upgrade``

### 6. Change the SSH port from 22 to 2200
``sudo nano /etc/ssh/sshd_config``
Change ``Port 20`` to ``Port 2200``

### 7. Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123)
```
sudo ufw allow 2200/tcp
sudo ufw allow 80/tcp
sudo ufw allow 123/tcp
sudo ufw enable
```
### 8. Configure the local timezone to UTC
1. ``sudo dpkg-reconfigure tzdata``
2. Find UTC option
### 9. Install and configure Apache to serve a Python mod_wsgi application
```
sudo apt-get install apache2
sudo apt-get install python-setuptools libapache2-mod-wsgi
```
### 10. Install and configure PostgreSQL:
```
sudo apt-get install postgresql
sudo su - postgres
psql
CREATE DATABASE catalog;
CREATE USER catalog;
postgres ALTER catalog password '12345' 
```
## deploy Flask Application
Reference to deploy Flask app on Ubuntu 
[https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps)


