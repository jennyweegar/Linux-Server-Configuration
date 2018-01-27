# Linux-Server-Configuration
Linux setup on a virtual machine to host a web app- Udacity project #5

This is an installation of the Ubuntu Linux on a virtual machine to host a Flask web application. This includes the installation of updates, securing the system form a number of attack vectors and installing/configuring web and database servers.

## Walkthrough
Server Info

Public IP: 18.218.97.14 <br>
Port: 2200 <br>
http://34.210.194.203/ <br>

## Getting Started
This project uses Amazon Lightsail to create a Linux server instance.

### Get your server.

Start a new Ubuntu Linux server instance on [Amazon Lightsail](https://aws.amazon.com/lightsail/)

* Log in
* Create an instance
* Choose an instance image: Ubuntu (located under OS only)
* Choose your instance plan (lowest tier $5 is fine)
* Give your instance a hostname
* Wait for startup
* Once the instance has started up, follow the instructions provided to SSH into your server.
* Secure your server.

#### Update all currently installed packages.

  ```sudo apt-get update``` <br> 
  ```sudo apt-get upgrade```<br>
auto upgrades run sudo dpkg-reconfigure --priority=low unattended-upgrades

#### Change the SSH port from 22 to 2200. Make sure to configure the Lightsail firewall to allow it.

  ```sudo vim /etc/ssh/sshd_config```
  
change port form 22 to 2200

#### Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123).

 ```sudo ufw allow 2200/tcp```<br>
 ```sudo ufw allow 80/tcp```<br>
 ```sudo ufw allow 123/tcp```<br>
 ```sudo ufw enable```<br>
  
*(Warning: When changing the SSH port, make sure that the firewall is open for port 2200 first, so that you don't lock yourself out of the server. Review this video for details! When you change the SSH port, the Lightsail instance will no longer be accessible through the web app 'Connect using SSH' button. The button assumes the default port is being used. There are instructions on the same page for connecting from your terminal to the instance. Connect using those instructions and then follow the rest of the steps.)*

#### Give grader access.

In order for your project to be reviewed, the grader needs to be able to log in to your server.

* Create a new user account named grader.

 ```sudo adduser grader```
* Give grader the permission to sudo.

  ```sudo vim /etc/sudoers.d/grader```
add text ```grader ALL=(ALL) NOPASSWD:ALL```

* Create an SSH key pair for grader using the ssh-keygen tool.

      ```ssh-keygen -t rsa```
      
  * To login
      ```ssh -i .ssh/grader_udacity grader@34.210.194.203 -p 2200```
#### Prepare to deploy your project.

* Configure the local timezone to UTC.

  ```sudo dpkg-reconfigure tzdata```
  
select none of the above, then UTC

* Install and configure Apache to serve a Python mod_wsgi application.

ngnix is used instead of Apache

configuration steps can be found here

https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-uwsgi-and-nginx-on-ubuntu-16-04

* Install and configure PostgreSQL:

  ```sudo apt-get install postgresql```
#### Do not allow remote connections

Create a new database user named catalog that has limited permissions to your catalog application database.

https://help.ubuntu.com/community/PostgreSQL

#### Deploy the Item Catalog project

http://34.210.194.203/
