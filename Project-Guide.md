
# Step-by-Step Guide: Deploy WordPress on AWS with Free Hosting & SSL Certificate

## Introduction
In this step-by-step guide, we will walk through the process of setting up a WordPress website on an Amazon Web Services (AWS) EC2 instance. The best part? AWS offers a free tier for the first 12 months, making this a cost-effective solution. Additionally, we’ll secure our site with a free auto-renewing SSL certificate using Let’s Encrypt and Certbot.

## Prerequisites
Before you begin, ensure you have the following:

- An AWS account (if not, sign up for 12 months FREE at AWS Free Tier).
- A registered domain name (e.g., purchased from a registrar like Google Domains etc).
- Basic knowledge of the command line.

## Step 1: Launch an EC2 Instance
1. Visit [AWS Free Tier](https://aws.amazon.com/free/) and log in to your AWS account.
2. Navigate to the EC2 dashboard and click “Launch Instance.”
3. Filter the eligible instances for the free tier and choose “Ubuntu Server 20.04 LTS.”
4. Select the t2.micro instance type and configure storage (you can increase to 30GB).
5. Add tags if needed and configure security groups to allow SSH, HTTP, and HTTPS traffic.
6. Launch the instance, create a new key pair, download it, and launch the instance.
7. Allocate Elastic IP Address, then Associate Elastic IP Address to your EC2 Instance for a static IP address.

## Step 2: Configure Domain and DNS
1. Copy the public IP of the EC2 instance.
2. In your domain registrar (e.g., Google Domains), add DNS records:
   - A record for your domain pointing to the EC2 instance IP.
   - A record for www.yourdomain.com pointing to the same IP.
3. Allow time for DNS propagation.

## Step 3: Connect to the EC2 Instance
1. Once the instance is running, copy the public IP address.
2. Open a terminal and use SSH to connect:
   ```bash
   chmod 400 your-key.pem
   ssh -i your-key.pem ubuntu@your-public-ip
   ```

## Step 4: Install LEMP Stack (Linux, Nginx, MySQL, PHP)
1. Update and upgrade the system:
   ```bash
   sudo apt update
   sudo apt upgrade
   ```
2. Install the LEMP stack:
   ```bash
   sudo apt install nginx mariadb-server php-fpm php-mysql
   ```

## Step 5: Install WordPress
1. Create a WordPress directory and download WordPress:
   ```bash
   cd /var/www
   sudo wget https://wordpress.org/latest.tar.gz
   sudo tar xzvf latest.tar.gz
   sudo rm latest.tar.gz
   ```
2. Set ownership and permissions:
   ```bash
   sudo chown -R www-data:www-data wordpress
   sudo find wordpress/ -type d -exec chmod 755 {} \;
   sudo find wordpress/ -type f -exec chmod 644 {} \;
   ```

## Step 6: Setup the Database
1. Secure your MariaDB installation by adding a password and disabling other features.
   ```bash
   sudo mysql_secure_installation
   ```
2. Access the MariaDB console with the password that you just created.
   ```bash
   sudo mysql -u root -p
   ```
3. Within the MariaDB console, create a database for WordPress.
   ```sql
   create database example_db default character set utf8 collate utf8_unicode_ci;
   create user 'example_user'@'localhost' identified by 'example_pw';
   grant all privileges on example_db.* TO 'example_user'@'localhost';
   flush privileges;
   exit
   ```

## Step 7: Configure Nginx Web Server
1. Navigate to the directory which contains configuration files for the Nginx web server, and create a new configuration file.
   ```bash
   sudo vim /etc/nginx/sites-available/wordpress.conf
   ```
2. Paste the configuration and save.
   ```nginx
   upstream php-handler {
       server unix:/var/run/php/php7.4-fpm.sock;
   }
   server {
       listen 80;
       server_name your_domain.com www.your_domain.com;
       root /var/www/wordpress;
       index index.php;
       location / {
           try_files $uri $uri/ /index.php?$args;
       }
       location ~ \.php$ {
           include snippets/fastcgi-php.conf;
           fastcgi_pass php-handler;
       }
   }
   ```
3. Enable the Nginx configuration and restart the server:
   ```bash
   sudo ln -s /etc/nginx/sites-available/wordpress.conf /etc/nginx/sites-enabled
   sudo nginx -t
   sudo systemctl restart nginx
   ```

## Step 8: Install SSL Certificate with Certbot
1. Install Certbot:
   ```bash
   sudo snap install core
   sudo snap refresh core
   sudo snap install --classic certbot
   sudo ln -s /snap/bin/certbot