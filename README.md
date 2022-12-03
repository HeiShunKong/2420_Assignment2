# 2420_Assignment2

Step 1 
Follow the video and create two droplets, VPC, and Firewall

Step 2
Create a regular users on both droplets

ssh -i ~/.ssh/DO_key root@droplet_ip
useradd -ms /bin/bash 
usermod -aG sudo 
passwd 
rsync --archive --chown=johnny:johnny ~/.ssh /home/name

Step 3
Install caddy in both droplets

wget https://github.com/caddyserver/caddy/releases/download/v2.6.2/caddy_2.6.2_linux_amd64.tar.gz
tar xvf caddy_2.6.2_linux_amd64.tar.gz
sudo chown root: caddy
sudo cp caddy /usr/bin/

Step 4 
On your local machines create two new directories html and src
