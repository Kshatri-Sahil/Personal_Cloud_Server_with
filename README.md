# Personal Cloud Server with Nextcloud
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

# Step 1: System Update:
Make sure your system is up to date before installing.
> sudo apt update && sudo apt upgrade -y

# Step 2: Install Apache, MariaDB, and PHP:
To run Nextcloud, we need a web server (Apache), a database (MariaDB), and PHP for scripting.
These commands install Apache, MariaDB, and all the required PHP extensions:

> sudo apt install apache2 mariadb-server libapache2-mod-php7.4

>sudo apt install php7.4 php7.4-mysql php7.4-xml php7.4-curl php7.4-gd php7.4-zip php7.4-mbstring php7.4-intl php7.4-json 

>sudo systemctl status apeche2

# Step 3: Set Up MariaDB for Nextcloud:

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

# Step 4: Download and Install Nextcloud:

Now, Download Nextcloud and set up permissions.
This command downloads and unzips the Nextcloud files to the /var/www/nextcloud directory and sets the appropriate permissions.

>wget https://download.nextcloud.com/server/releases/nextcloud-25.0.0.zip
>sudo unzip nextcloud-25.0.0.zip -d /var/www/

>sudo chown -R www-data:www-data /var/www/nextcloud

>sudo chmod -R 755 /var/www/nextcloud

Enable the site and necessary Apache modules:
>sudo a2ensite nextcloud.conf

>sudo a2enmod rewrite headers env dir mime

>sudo systemctl restart apache2

# Step 5: Configure Apache for Nextcloud

You need to configure Apache to handle both IPv4 and IPv6 connections and add the correct ServerName. Follow these steps

Create a new Apache configuration file for Nextcloud:
>sudo nano /etc/apache2/sites-available/nextcloud.conf

Add the following to the file:
>
<VirtualHost *:80>
    DocumentRoot /var/www/nextcloud/
    ServerName 198.168.192.149
    <Directory /var/www/nextcloud/>
        Require all granted
        AllowOverride All
        Options FollowSymLinks MultiViews
    </Directory>
    ErrorLog ${APACHE_LOG_DIR}/nextcloud_error.log
    CustomLog ${APACHE_LOG_DIR}/nextcloud_access.log combined</VirtualHost>

# Step 6: Complete Nextcloud Web Setup

Now, open your web browser and go to http://192.168.192.149 to complete the setup by entering your database credentials and creating an admin account.
 
# Step 7: Install and Configure Ngrok

Install Ngrok to expose your Nextcloud instance to the internet.

Install Ngrok:
>sudo snap install ngrok

Add your Ngrok authentication token:
>ngrok config add-authtoken 2nf5HUrxcY4wuzK3i6QU48qkIGJ_7Nz6YdSU4biDkss3F2x5b
 
Start Ngrok for HTTP traffic [This will give you a public URL that forwards to your local Nextcloud instance.]:
>ngrok http 80

Add the forwarding ip address in /var/www/html/nextcloud/config.config.php:

Open the file:
>sudo nano /var/www/html/nextcloud/config.config

Add the forwarding ip in trusted domains array:

‘trusted_domain’ =>
Array(
	0 => ‘192.168.192.149’, //localhost ip address
	1=> ‘https://b383-106-221-101-149.ngrok-free.app’,
),
 
# Step 8: Automate Ngrok on Startup Using crontab

Install Crontab:
>sudo apt install cron
>sudo systemctl enable cron
>sudo systemctl start cron

This ensures that the cron service is installed and running.

Set up Ngrok to start automatically:
Open the crontab editor:
> sudo crontab -e

Add the following line to the end of the file:
@reboot /usr/bin/ngrok http 80
 
# Step 9: Access Nextcloud Web Interface via Ngrok

After starting Ngrok, it will provide you with a public URL (https://b383-106-221-101-149.ngrok-free.app). Use this URL to access your Nextcloud instance from any device, anywhere.

# Step 10: Managing Nextcloud and NgrokTo monitor or manage your Nextcloud instance:

1. Restart Apache: >sudo systemctl restart apache2
2. Check Ngrok status: >ps aux | grep ngrok
 
# Step 11: Nextcloud login and Setup

After logging in as admin:
 
Adding Files into cloud:
1.	Navigate to “All files”.
2.	Click on “+New”.

# Step 12: Adding New user

1. We can add a new user so that they can also use the server to upload their own data. 
2. The New user cannot see or access other account. Not even the admin can.
3. Click on “New Account”
4. Fill the required details.
5. Click on “Add new user”. 

Now you can login through the user account (eg: sahil)

# Step 13: Setting up Nextcloud through mobile app.

Accessing the account through mobile app. 
