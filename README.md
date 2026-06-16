# WEB STACK INPLMENTAION (LAMP STACK) IN AWS
## Introduction
The LAMP stack is one of the most open-source popular web stack technology or web development tools which consists of Linux as the operating system, Apache as the web server, MySQL as the web database/storage and Php as the programming language. This documentation outline the step followed, configuratiions and the implementation of Lamp Stack with AWS.

# STEP 0 - Prerequisites
 1. EC2 Instance of t3.micro type and Ubuntu 24.04 LTS (HVM) was lunched in the eu-north-1 region using the AWS console.
![creating ec2 on AWS](https://github.com/Afeez-Bak/MY-LAMP-STACK-PROJECT/blob/main/LAMP%20STACK/Ec2%20server%20on%20AWS%201.png)

2. Ec2 Server properties
![Ec2 server properties](https://github.com/Afeez-Bak/MY-LAMP-STACK-PROJECT/blob/main/LAMP%20STACK/Ec2%20server%20PROPOERTIES.png)

3. Created a ssh key pair named LAMPKEY to get access to the Ecs instance created on AWS through port 22.
4. The security group properties and configurations are as followed:
   - Allow traffic access on port 80 (HTTP) with source from anywhere on the internet.
   - Allow traffic access on port 443 (HTTPS) with source from anywhere on the internet.
   - Allow traffic on port 22 (SSH) with source from any IP address.
![Ec2 security group properties](https://github.com/Afeez-Bak/MY-LAMP-STACK-PROJECT/blob/main/LAMP%20STACK/Ec2%20server%20SECURITY%20GROUP.png)
5. The private ssh key of the Lamp server that was created and downloaded was located on my local harddrive in the download folder, but permission was denied while trying to connect with server due to the folder accessibility properties which allow others to access the private key.
![ssh access](https://github.com/Afeez-Bak/MY-LAMP-STACK-PROJECT/blob/main/LAMP%20STACK/connecting%20to%20Ec2%20instance.png)
6. created a new directory named (.ssh) on the home directory and moving my ssh private key to the newly created directory usin the the following command
> mkdir -p ~/.ssh  
> cp /mnt/users/user/Downloads/LAMPKEY.pem ~/.ssh/
![creat new folder for ssh](https://github.com/Afeez-Bak/MY-LAMP-STACK-PROJECT/blob/main/LAMP%20STACK/creating%20.ssh%20folder%20for%20pem%20key.png)
7. Successfully connect to my Ecs server using:
> ssh -i LAMPKEY.pem ubuntu@13.51.170.234    
where LAMPKEY.pem = my ssh key, ubuntu = my ec2 username and 13.51.170.234 = my public ip address
![ssh successful](https://github.com/Afeez-Bak/MY-LAMP-STACK-PROJECT/blob/main/LAMP%20STACK/ssh%20into%20Ec2%20instatnce%20successful.png)

# STEP 1 - Installing Apache and Updating Firewall
1. Update and upgrade list of packages in package manager
![server update](https://github.com/Afeez-Bak/MY-LAMP-STACK-PROJECT/blob/main/LAMP%20STACK/server%20and%20firewall%20update.png)
![server update](https://github.com/Afeez-Bak/MY-LAMP-STACK-PROJECT/blob/main/LAMP%20STACK/server%20and%20firewall%20update%202.png)
2. Apache2 package installation
![apache installation](https://github.com/Afeez-Bak/MY-LAMP-STACK-PROJECT/blob/main/LAMP%20STACK/apache%20install.png)
3. Enable and verify Apache2 running in my OS
![apache2 running](https://github.com/Afeez-Bak/MY-LAMP-STACK-PROJECT/blob/main/LAMP%20STACK/start%20%26%20verify%20apache%20running.png)
4.  Verify if my server is running and can be access locally in the ubuntu shell.
![local access verify](https://github.com/Afeez-Bak/MY-LAMP-STACK-PROJECT/blob/main/LAMP%20STACK/apache%20local%20host.png)
5. Verify through the public IP address if the Apache HTTP server can respond to request from the internet using the url on a browser.
![apache running](https://github.com/Afeez-Bak/MY-LAMP-STACK-PROJECT/blob/main/LAMP%20STACK/apache%20runing.png)

# STEP 2 - Installing MySQL
1. Installing a database (MySQL) for the project.
![installing mysql](https://github.com/Afeez-Bak/MY-LAMP-STACK-PROJECT/blob/main/LAMP%20STACK/install%20mysql.png)
2. Login into MySQL console server as the adminitrative database user root.
![mysql login](https://github.com/Afeez-Bak/MY-LAMP-STACK-PROJECT/blob/main/LAMP%20STACK/log%20into%20mysql.png)
3.  Securing MySQL by running an Interactive script. This script removes some insecure settings and lock down access to the database system.
![mysql](https://github.com/Afeez-Bak/MY-LAMP-STACK-PROJECT/blob/main/LAMP%20STACK/setup%20mysql%20secure%20installation.png)

# STEP 3 - Installing PHP
After installing Apache2 to serve content and MySQL installed to store and manage my data. PHP is the componenet in our setup that will process code to display dynamic content to the users.

1. The following programs were installed:
- php package
- php-mysql= PHP module that allows php to communicate with MySQL-based databases.
- libapache2-mod-php= to enable Apache to handle PHP files
![php installation](https://github.com/Afeez-Bak/MY-LAMP-STACK-PROJECT/blob/main/LAMP%20STACK/php%2C%20libapache2%20%26php-mysql%20installation.png)
2. Confirm PHP version
  ![php version](https://github.com/Afeez-Bak/MY-LAMP-STACK-PROJECT/blob/main/LAMP%20STACK/php%20version.png)

# STEP 4 - Creating a Virtual Host for Website using Apache
1. Creating  a new directory (lampproject) next to Apache default server block
2. Assigning ownership to the directory
![virtual host](https://github.com/Afeez-Bak/MY-LAMP-STACK-PROJECT/blob/main/LAMP%20STACK/def.%20virtual%20hosting%20page%20%26%20ownership%20assigned.png)
3. Create and open a new configuration file in apache’s “sites-available” directory using vim.
![conf file](https://github.com/Afeez-Bak/MY-LAMP-STACK-PROJECT/blob/main/LAMP%20STACK/vim%20editor.png)
4. Show the new file in sites-available
![new file](https://github.com/Afeez-Bak/MY-LAMP-STACK-PROJECT/blob/main/LAMP%20STACK/new%20file%20in%20site-available.png)

With the VirtualHost configuration, Apache will serve lampproject using /var/www/lampproject as its web root directory. 

5. Enable the new virtual host
![virtual host](https://github.com/Afeez-Bak/MY-LAMP-STACK-PROJECT/blob/main/LAMP%20STACK/a2ensite.png)
6. While bashing apache2ctl configtest to check the configuration file for any syntax error, the response was that the virtualhost was not closed.
![syntax error](https://github.com/Afeez-Bak/MY-LAMP-STACK-PROJECT/blob/main/LAMP%20STACK/syntaxx%20error.png)
7. I discovered that the lampproject configuration was supposed to be **</VirtulHost>** instead of **<VirtualHost>** which was why it says the vitualHost was not closed.
After edditing the configuration file using vim editor. the configuration file is now okay
![syntax okay](https://github.com/Afeez-Bak/MY-LAMP-STACK-PROJECT/blob/main/LAMP%20STACK/syntax%20okay.png)
8. Reload apache for all changes to take effect.
![reload apache](https://github.com/Afeez-Bak/MY-LAMP-STACK-PROJECT/blob/main/LAMP%20STACK/reload%20apache2.png)
9. Open the website with public ip address
![website](https://github.com/Afeez-Bak/MY-LAMP-STACK-PROJECT/blob/main/LAMP%20STACK/website%20active.png)

# STEP 5 - Enable PHP on the website

1. Open the dir.conf file with vim to edit the index page
![index page](https://github.com/Afeez-Bak/MY-LAMP-STACK-PROJECT/blob/main/LAMP%20STACK/index%20page.png)
2. Realod Apache for changes to take effect.
![reload apache](https://github.com/Afeez-Bak/MY-LAMP-STACK-PROJECT/blob/main/LAMP%20STACK/reload%20apache%20for%20index.png)
3. creating new file named index.php inside custom web root folder
4. add a php code inside the file and refresh.
![php](https://github.com/Afeez-Bak/MY-LAMP-STACK-PROJECT/blob/main/LAMP%20STACK/php%20page.png)

This page provides information about the server from the perspective of PHP. It is useful for debugging and to ensure the settings are being applied correctly.

After checking the relevant information about the server through this page, the file was removed as it contains sensitive information about the PHP environment and the ubuntu server. It can always be recreated if the information is needed later.

  
