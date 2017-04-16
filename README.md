# LinuxConfiguration

1. IP address: http://35.166.123.172/ (The server is currently closed (April 2016))
2. SSH:
```
ssh -i ~/.ssh/[RSA file] root@35.166.123.172 -p 2200
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
### SSH into the server 
```
ssh -i ~/.ssh/[RSA file] root@35.166.123.172
```
### Create a new user named grader

1. ``sudo adduser grader``

### Give the grader the permission to sudo
1. ``sudo nano /etc/sudoers.d/grader``
2. add ``"grader ALL=(ALL:ALL) ALL"`` to the file. 

### Update all currently installed packages
``sudo apt-get update``
``sudo apt-get upgrade``

### Change the SSH port from 22 to 2200
1. ``sudo nano /etc/ssh/sshd_config``
2. Change ``Port 20`` to ``Port 2200``
3. ``sudo service ssh restart`` 
4. Next time to login, ``ssh -i ~/.ssh/udacity_key.rsa grader@35.166.123.172 -p 2200``

### Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123)
1. From ``/etc/services``, edit ssh port from 22 to 2200
```
sudo ufw allow ssh
sudo ufw allow 2200/tcp
sudo ufw allow http
sudo ufw allow 80/tcp
sudo ufw allow ntp
sudo ufw allow 123/tcp
sudo ufw enable
```
Reference :[digital-ocean ufw status and rules](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu-14-04)
### Configure the local timezone to UTC
1. ``sudo dpkg-reconfigure tzdata``
2. Find UTC option
### Install and configure Apache to serve a Python mod_wsgi application
```
sudo apt-get install apache2
sudo apt-get install python-setuptools libapache2-mod-wsgi
```
### Install and configure PostgreSQL:
```
sudo apt-get install postgresql
sudo su - postgres
psql
CREATE DATABASE catalog;
CREATE USER catalog;
postgres ALTER catalog password '12345' 
```
### Ensure that another user (such as grader) is allowed to remote/ssh login and sudo before disabling the remote root login, otherwise, the server could get irreversibly locked. 
1. Login to VM and copy authorized key file to the grader's ssh directory. 

```
ssh -i ~/.ssh/[RSA file] grader@35.166.123.172 -p 2200
su root
cp /root/.ssh/authorized_keys /home/grader/.ssh/
```

2. copy the content of the udacity_key.rsa from local machine to the authorized key file. 
3. Change the permission of ssh file, authorized key. Change owner of the ssh file. 

``` 
sudo chmod 700 /home/grader/.ssh
sudo chmod 644 /home/grader/.ssh/authorized_keys
sudo chown -R grader:grader /home/grader/.ssh
```

4. Test if I can login as grader. ``ssh -i ~/.ssh/udacity_key.rsa grader@35.166.123.172``

### Disable remote Login of the 'root' user
1. In /etc/ssh/sshd_config, change ``PermitRootLogin without-passwod`` to ``PermitRootLogin no``
2. ``sudo service ssh restart`` 
## Deploy Flask Application Reference
Reference to deploy Flask app on Ubuntu 
[https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps)


