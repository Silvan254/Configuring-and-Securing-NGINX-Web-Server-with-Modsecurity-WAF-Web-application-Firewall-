Home Lab Project: 

Installing and securing Nginx Web Server with Web Application Firewall on Ubuntu Server VM

Objectives:

Setting up a home lab project aimed at installing and securing an Nginx web server with a Web Application Firewall (WAF) on an Ubuntu Server Virtual Machine (VM). This project aims to provides a secure environment for hosting web applications by utilizing Nginx as the web server and integrating ModSecurity as the WAF for enhanced security, and to horn my skills on Web application security.

Hardware and Software Used:

Ubuntu Host: 16G 256SSD I5 3rd Gen with VirtualBox running:
    • Ubuntu Server VM: For installing Nginx web sever and 
    • Nginx Web Server
    • Modsecurity WAF
    


1. Power up Ubuntu server and kali VM on VirtualBox
#Running sudo apt update to refresh package lists for us to install required dependancies for nginx and modsecurity.
![1](https://github.com/Silvan254/Configuring-and-Securing-NGINX-Web-Server-with-Modsecurity-WAF-Web-application-Firewall-/assets/65334897/b9b81701-657a-4fd2-9e89-c099cf9faf7d)


2. Install the prerequisites:

   
![2](https://github.com/Silvan254/Configuring-and-Securing-NGINX-Web-Server-with-Modsecurity-WAF-Web-application-Firewall-/assets/65334897/5f9dc224-0bab-4210-abeb-bbe1891ac299)



3.Installing nginx, 

$ sudo apt install nginx

![3](https://github.com/Silvan254/Configuring-and-Securing-NGINX-Web-Server-with-Modsecurity-WAF-Web-application-Firewall-/assets/65334897/51aac9cd-479a-4901-ae8c-0b74cd25fae0)

Checking if my Nginx service is running:

$ sudo systemctl status Nginix

![Screenshot from 2024-05-19 16-49-09](https://github.com/Silvan254/Configuring-and-Securing-NGINX-Web-Server-with-Modsecurity-WAF-Web-application-Firewall-/assets/65334897/95154996-6a0b-4800-ac6c-b12822e2ee94)



and from my host browser
![4](https://github.com/Silvan254/Configuring-and-Securing-NGINX-Web-Server-with-Modsecurity-WAF-Web-application-Firewall-/assets/65334897/725c0e38-91af-4d3d-ad2f-4976bb2fd395)

3. Download and Compile the ModSecurity 3.0 Source Code
Clonning the GitHub repository:

![5](https://github.com/Silvan254/Configuring-and-Securing-NGINX-Web-Server-with-Modsecurity-WAF-Web-application-Firewall-/assets/65334897/611cc8c9-da19-4284-aaf3-7c8d143fa03e)


Change to the ModSecurity directory and compile the source code:
$ cd ModSecurity
$ git submodule init
$ git submodule update
$ ./build.sh
$ ./configure
$ make
$ make install
$ cd ..



4 – Downloading the NGINX Connector for ModSecurity and Compiling It as a Dynamic Module
Clone the GitHub repository:
$ git clone --depth 1 https://github.com/SpiderLabs/ModSecurity-nginx.git

![6](https://github.com/Silvan254/Configuring-and-Securing-NGINX-Web-Server-with-Modsecurity-WAF-Web-application-Firewall-/assets/65334897/877fdac5-d1a0-4ecd-8b2c-571870160982)

Determine which version of NGINX is running on the host where the ModSecurity module will be loaded:

![7](https://github.com/Silvan254/Configuring-and-Securing-NGINX-Web-Server-with-Modsecurity-WAF-Web-application-Firewall-/assets/65334897/7df3ef52-9b6c-406e-ba32-341f50072274)


Downloading the source code corresponding to the installed version of NGINX (the complete 
sources are required even though only the dynamic module is being compiled):
![8](https://github.com/Silvan254/Configuring-and-Securing-NGINX-Web-Server-with-Modsecurity-WAF-Web-application-Firewall-/assets/65334897/f4f717ec-14ac-4664-be87-3339ea8a0e33)

![1](https://github.com/Silvan254/Configuring-and-Securing-NGINX-Web-Server-with-Modsecurity-WAF-Web-application-Firewall-/assets/65334897/77abd59f-17ba-4296-89e3-ba06dee34fd4)

Compile the dynamic module and copy it to the standard directory for modules:
$ cd nginx-1.13.1
$ ./configure --with-compat --add-dynamic-module=../ModSecurity-nginx
$ make modules
$ cp objs/ngx_http_modsecurity_module.so /etc/nginx/modules
5 – Loading the NGINX ModSecurity Connector Dynamic Module:
Adding load_module directive to the main (top‑level) context in /etc/nginx/nginx.conf. It instructs NGINX to load the ModSecurity dynamic module when it processes the configuration:
load_module modules/ngx_http_modsecurity_module.so;


6 – Configure, Enable, and Test ModSecurity
The final step is to enable and test ModSecurity.

Set up the appropriate ModSecurity configuration file. Here we’re using the recommended ModSecurity configuration provided by TrustWave Spiderlabs, the corporate sponsors of ModSecurity.

$ mkdir /etc/nginx/modsec
$wget-P/etc/nginx/modsec/https://raw.githubusercontent.com/SpiderLabs/ModSecurity/v3/master/modsecurity.conf-recommended
$ mv /etc/nginx/modsec/modsecurity.conf-recommended /etc/nginx/modsec/modsecurity.conf

server {
    # ...
    modsecurity on;
    modsecurity_rules_file /etc/nginx/modsec/main.conf;
}

![1](https://github.com/Silvan254/Configuring-and-Securing-NGINX-Web-Server-with-Modsecurity-WAF-Web-application-Firewall-/assets/65334897/77abd59f-17ba-4296-89e3-ba06dee34fd4)

Customize the configuration according to your specific requirements, including server name, port, and proxy settings.

![Screenshot from 2024-05-19 17-07-09](https://github.com/Silvan254/Configuring-and-Securing-NGINX-Web-Server-with-Modsecurity-WAF-Web-application-Firewall-/assets/65334897/f4e62fed-d81c-4738-a133-a0a1bf813d1d)

Save the changes and exit the text editor.
4. Installing ModSecurity:

Install ModSecurity for Nginx using the following command:

sudo apt install libnginx-mod-security
5. Enabling ModSecurity in Nginx:

Create a symbolic link to enable ModSecurity in Nginx configuration:
bash
Copy code
sudo ln -s /etc/nginx/mods-available/security.conf /etc/nginx/conf.d/
6. Configuring ModSecurity:

Customize the ModSecurity configuration file located at /etc/nginx/mods-available/security.conf to suit your security requirements.
Configure rules, settings, and other parameters as needed to enhance the security of your web application.
7. Restarting Nginx:

After making configuration changes, restart the Nginx service to apply the changes:
Copy code
sudo systemctl restart nginx
8. Testing:

Test your web application to ensure it is functioning correctly and that the Web Application Firewall is properly securing it.
Perform various tests, including functional testing and security testing, to verify the effectiveness of the setup.
9. Maintenance and Monitoring:

Regularly update Nginx and ModSecurity to ensure you have the latest security patches and updates.
Monitor server logs for any suspicious activity and adjust firewall rules accordingly.
Implement HTTPS using SSL/TLS certificates for additional security.
Backup configuration files regularly to prevent data loss in case of system failure or misconfiguration.
Conclusion:
This home lab project provides a practical guide for installing and securing an Nginx web server with a Web Application Firewall on an Ubuntu Server VM. By following the steps outlined in this documentation, users can create a secure environment for hosting web applications, thereby mitigating various online threats and vulnerabilities. Continuous monitoring, maintenance, and updates are essential to ensure the ongoing security and reliability of the web server setup.
