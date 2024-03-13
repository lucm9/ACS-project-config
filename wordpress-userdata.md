#!/bin/bash
mkdir /var/www/
sudo mount -t efs -o tls,accesspoint=fsap-0ab7307886c5ec67b fs-07f4d72115cd33ad8:/ /var/www/
sudo yum install -y httpd 
sudo systemctl start httpd
sudo systemctl enable httpd
sudo yum module reset php -y
sudo yum module enable php:remi-8.3 -y
sudo yum install -y php php-common php-mbstring php-opcache php-intl php-xml php-gd php-curl php-mysqlnd php-fpm php-json
sudo systemctl start php-fpm
sudo systemctl enable php-fpm
sudo wget http://wordpress.org/latest.tar.gz
sudo tar xzvf latest.tar.gz
sudo rm -rf latest.tar.gz
sudo cp wordpress/wp-config-sample.php wordpress/wp-config.php
mkdir /var/www/html/
sudo cp -R wordpress/* /var/www/html/
cd /var/www/html/
touch healthstatus
sudo sed -i "s/localhost/cls-database.c1cei6qi8n1k.us-east-2.rds.amazonaws.com/g" wp-config.php 
sudo sed -i "s/username_here/admin/g" wp-config.php 
sudo sed -i "s/password_here/adminadmin/g" wp-config.php 
sudo sed -i "s/database_name_here/wordpressdb/g" wp-config.php 
sudo chcon -t httpd_sys_rw_content_t /var/www/html/ -R
sudo systemctl restart httpd









