#!/bin/bash
sudo yum install -y nginx
sudo systemctl start nginx
sudo systemctl enable nginx
git clone https://github.com/lucm9/ACS-project-config.git
sudo mv ACS-project-config/reverse.conf /etc/nginx/
sudo mv /etc/nginx/nginx.conf /etc/nginx/nginx.conf-distro
cd /etc/nginx/
sudo touch nginx.conf
sudo sed -n 'w nginx.conf' reverse.conf
sudo systemctl restart nginx
sudo rm -rf reverse.conf
sudo rm -rf /ACS-project-config



