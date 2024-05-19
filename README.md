# Configuring-and-Securing-NGINX-Web-Server-with-Modsecurity-WAF-Web-application-Firewall-
Configuring and Securing NGINX Web Server with Modsecurity WAF(Web application Firewall)
# Installing and Securing Nginx Web Server with Web Application Firewall on Ubuntu Server VM

This guide walks you through the process of installing and securing the Nginx web server with a Web Application Firewall (WAF) on an Ubuntu Server virtual machine (VM).

## Prerequisites

- An Ubuntu Server VM (tested on Ubuntu 22.04)
- SSH access to the server with a user that has sudo privileges
- Basic knowledge of using the terminal

## Step 1: Update System Packages

Before installing any software, it's important to update your system packages:

```bash
sudo apt update
sudo apt upgrade -y
sudo systemctl start nginx
sudo systemctl enable nginx
