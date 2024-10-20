# Personal_Cloud_Server_with_Nextcloud

This project involves the deployment of a Nextcloud server on an Ubuntu-based system, allowing users to create a private cloud storage solution that is accessible from anywhere in the world. Nextcloud is an open-source software suite that enables file synchronization and sharing, providing a robust alternative to commercial cloud storage services. The server is configured to be publicly accessible using Ngrok, a tool that creates secure tunnels to localhost, allowing for external access to the Nextcloud instance without the need for complex network configurations like port forwarding.

The below content will guide you to Install, Setup, Configure nextcloud,MariaDB, PHP, Ngrok which are required to accomplish Personal Cloud Server.
Table of Contents
1.	Prerequisites
2.	Step 1: System Update
3.	Step 2: Install Apache, MariaDB, and PHP
4.	Step 3: Set Up MariaDB for Nextcloud
5.	Step 4: Download and Install Nextcloud
6.	Step 5: Configure Apache for Nextcloud
7.	Step 6: Complete Nextcloud Web Setup
8.	Step 7: Install and Configure Ngrok
9.	Step 8: Automate Ngrok on Startup Using Crontab
10.	Step 9: Access Nextcloud Web Interface via Ngrok
11.	Step 10: Managing Nextcloud and Ngrok
12.	Step 11: Nextcloud login and Setup
13.	Step 12: Adding New user
14.	Step 13: Setting up Nextcloud through mobile app.

Prerequisites:

Before starting, ensure you have:
- Ubuntu system (20.04 or later recommended)
- Sudo privileges
- Basic knowledge of Linux commands
- Stable internet connection

Step 1: System Update:
Make sure your system is up to date before installing.
> sudo apt update && sudo apt upgrade -y

Step 2: Install Apache, MariaDB, and PHP:
To run Nextcloud, we need a web server (Apache), a database (MariaDB), and PHP for scripting.
These commands install Apache, MariaDB, and all the required PHP extensions:

>sudo apt install apache2 mariadb-server libapache2-mod-php7.4
>sudo apt install php7.4 php7.4-mysql php7.4-xml php7.4-curl php7.4-gd php7.4-zip php7.4-mbstring php7.4-intl php7.4-json 
>sudo systemctl status apeche2

Step 3: Set Up MariaDB for Nextcloud:

Run the secure installation to set up root passwords and secure the database:
>sudo mysql_secure_installation
 
Log into MariaDB as root and Create a database and a user in MariaDB for Nextcloud.:

>sudo mysql -u root -p
CREATE DATABASE nextcloud;
CREATE USER 'nextclouduser'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL ON nextcloud.* TO 'nextclouduser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
This sets up a secure MariaDB installation, creates a database called nextcloud, and a user with the necessary permissions.

Step 4: Download and Install Nextcloud:

Now, Download Nextcloud and set up permissions.
This command downloads and unzips the Nextcloud files to the /var/www/nextcloud directory and sets the appropriate permissions.

>wget https://download.nextcloud.com/server/releases/nextcloud-25.0.0.zip
>sudo unzip nextcloud-25.0.0.zip -d /var/www/

Accessing the account through mobile app. 
