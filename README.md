# Personal_Cloud_Server_with_Nextcloud

>sudo snap install ngrok

Add your Ngrok authentication token:
>ngrok config add-authtoken 2nf5HUrxcY4wuzK3i6QU48qkIGJ_7Nz6YdSU4biDkss3F2x5b
 
Start Ngrok for HTTP traffic [This will give you a public URL that forwards to your local Nextcloud instance.]:
>ngrok http 80

Add the forwarding ip address in /var/www/html/nextcloud/config.config.php:

Open the file;
>sudo nano /var/www/html/nextcloud/config.config

Add the forwarding ip in trusted domains array:
‘trusted_domain’ =>
Array(
	0 => ‘192.168.192.149’, //localhost ip address
	1=> ‘https://b383-106-221-101-149.ngrok-free.app’,
),
 
Step 8: Automate Ngrok on Startup Using crontab

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
 
Step 9: Access Nextcloud Web Interface via Ngrok
After starting Ngrok, it will provide you with a public URL (https://b383-106-221-101-149.ngrok-free.app). Use this URL to access your Nextcloud instance from any device, anywhere.

Step 10: Managing Nextcloud and Ngrok
To monitor or manage your Nextcloud instance:
•	Restart Apache: >sudo systemctl restart apache2
•	Check Ngrok status: >ps aux | grep ngrok
 
Step 11: Nextcloud login and Setup

After logging in as admin:
 
Adding Files into cloud:
1.	Navigate to “All files”.
2.	Click on “+New”.

Step 12: Adding New user 
•	We can add a new user so that they can also use the server to upload their own data. 
•	The New user cannot see or access other account. Not even the admin can.
•	Click on “New Account”
•	Fill the required details.
•	Click on “Add new user”. 

Now you can login through the user account (eg: sahil)



