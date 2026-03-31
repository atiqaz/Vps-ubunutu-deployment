

    
## VPS Deployement Node js

1. connect  terminus using ssh

### Install Node  and Npm 
```# Download and install nvm:
nvm install --lts
```

### Setup Firewall
```
sudo ufw enable
sudo ufw status
sudo ufw allow 22
sudo ufw allow 80
sudo ufw allow 443
```

### Install Nginx
 ``` 
 sudo apt install nginx
```

### Clone Your Repository
 Install dependencies and test app.
 configure env file  
 ``` 
 vi .env 
- insert mode by presssing. I
- paste content
- presss esc  to go out of insert mode
- then :wq for save
```

### Install Pm2
- PM2 provides automatic crash recovery, startup script generation, and process monitoring to keep applications reliably online.
- PM2’s cluster mode (pm2 start app.js -i max) runs multiple instances across all available cores and load-balances traffic between them, dramatically improving throughput.
``` 
npm install pm2 -g

pm2 start app.js --name "my-app"
pm2 save
pm2 startup
```


### Configure Nginx

``` 
sudo nano /etc/nginx/sites-available/your-site.conf
```
or you can use another name instead of default . it will create a file in  ```/etc/nginx/sites-available``` .

Add the following 

```
server {
    listen 80;
    server_name yourdomain.com www.yourdomain.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}

```
for frontend like build/ dist etc 
```
# =========================
    # FRONTEND (React Admin)
    # =========================

    root /home/ubuntu/projects/...; // replace with you path
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }
```

for logs add 

```
 # =========================
    # LOGS
    # =========================
    access_log /var/log/nginx/crm_access.log;
    error_log /var/log/nginx/crm_error.log;
```

#### Enable the site (create symlink)

``` 
sudo ln -s /etc/nginx/sites-available/your-site.conf /etc/nginx/sites-enabled/
```
#### Test & Reload

```
# Check NGINX config
sudo nginx -t

# Restart NGINX
sudo nginx -s reload

```

### Apply SSL using certbot

``` 
# install 
sudo apt install certbot python3-certbot-nginx

# apply ssl

sudo certbot --nginx -d yourdomain
```
