#!/bin/bash
mkdir /var/www/
sudo mount -t efs -o tls,accesspoint=fsap-07ba29f42608656c8 fs-0c8b7977ad1c0bb72:/ /var/www/
sudo yum install -y httpd 
sudo systemctl start httpd
sudo systemctl enable httpd
sudo yum module reset php -y
sudo yum module enable php:remi-8.3 -y
sudo yum install -y php php-common php-mbstring php-opcache php-intl php-xml php-gd php-curl php-mysqlnd php-fpm php-json
sudo systemctl start php-fpm
sudo systemctl enable php-fpm
git clone https://github.com/xashy-devops/votingapp.git
mkdir /var/www/html
sudo cp -R tooling/html/*  /var/www/html/
cd /tooling
sudo mysql -h cls-database.c1cei6qi8n1k.us-east-2.rds.amazonaws.com -u admin -p toolingdb < tooling-db.sql
cd /var/www/html/
touch healthstatus
sudo sed -i "s/$db = mysqli_connect('mysql.tooling.svc.cluster.local', 'admin', 'admin', 'tooling');/$db = mysqli_connect('cls-database.c1cei6qi8n1k.us-east-2.rds.amazonaws.com', 'admin', 'adminadmin', 'votingdb');/g" functions.php
sudo chcon -t httpd_sys_rw_content_t /var/www/html/ -R
sudo systemctl restart httpd







