# 2420_assign2

## Step 1 
Follow the video and create two droplets, VPC, and Firewall

## Step 2
Create a regular users on both droplets

**ssh -i ~/.ssh/DO_key root@droplet_ip
useradd -ms /bin/bash user
usermod -aG sudo user
passwd user
rsync --archive --chown=user:user ~/.ssh /home/user**

## Step 3
Install caddy in both droplets

**wget https://github.com/caddyserver/caddy/releases/download/v2.6.2/caddy_2.6.2_linux_amd64.tar.gz
tar xvf caddy_2.6.2_linux_amd64.tar.gz
sudo chown root: caddy
sudo cp caddy /usr/bin/**

## Step 4 
On your local machine create two new directories **html** and **src**
1. Create a new directory on local machine **mkdir 2420-assign-two**
2. Inside 2420-assign-two directory create two new directories html and src
3. Inside html directory create an index.html page **touch index.html**
4. Inside index.html **vim index.html**
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <b>Server 1/2</b>
</body>
</html>
```
5. Inside src directory use the following commands
```
npm init
npm i fastify
touch index.js
```
Inside index.js
```
// Require the framework and instantiate it
const fastify = require('fastify')({ logger: true })

// Declare a route
fastify.get('/', async (request, reply) => {
  return { hello: 'Server x' }
})

// Run the server!
const start = async () => {
  try {
    await fastify.listen({ port: 3000 })
  } catch (err) {
    fastify.log.error(err)
    process.exit(1)
  }
}
start()
```

6. Move html and src directories to both droplets 
**sftp -i ~/.ssh/key user@droplet_ip
put -r src
put -r html**
7. Move html and src directories to correct place

sudo mkdir -p ../../var/www/\
sudo mv html /var/www/\
sudo mv src /var/www/


## Step 5
Write caddyfile on local machine on both droplets
```
sudo mkdir /etc/caddy
sudo touch /etc/caddy/Caddyfile
sudo vim /etc/caddy/Caddyfile
```
Inside CaddyFile insert below code using vim Caddyfile
```
http://143.244.208.55 {
    root * /var/www
    reverse_proxy /api localhost:3030
    file_server
}
```


## Step 6
Install node and npm with Volta on both droplets (Need sudo if is mac)
```
curl https://get.volta.sh | bash
source ~/.bashrc
volta install node
```

## Step 7
Write a service file on local machine to start node application
1. Create a file **touch hello_web.service**
2. Edit the file **vim hello_web.service**
```
[Unit]
Description=runs a hello world webapp
After=network-online.target
Wants=network-online.target

[Service]
ExecStart=/home/user/.volta/bin/node /var/www/src/index.js
User=username
Group=user
Restart=always
RestartSec=10
TimeoutStopSec=90
SyslogIdentifier=hello_web

[Install]
WantedBy=multi-user.target
```

## Step 8
1.Upload hello_web file to both droplets \
using **sftp -i ~/.ssh/key user@droplet_ip **
2. Then move the hello_web file \
using **mv hello_web.service /etc/systemd/system**

