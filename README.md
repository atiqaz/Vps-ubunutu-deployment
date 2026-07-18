# 🚀 VPS Deployment Guide (Node.js + Nginx + PM2 + SSL)

This guide explains how to deploy a **Node.js application** on a VPS using **Nginx**, **PM2**, and **SSL (HTTPS)**.

---

## 📌 Prerequisites

* VPS (Ubuntu recommended)
* Domain name (optional but recommended)
* GitHub repository (public/private)
* SSH access to server

---

## 🔐 1. Connect to Server

```bash
ssh username@your_server_ip
```

Example:

```bash
ssh ubuntu@123.45.67.89
```

---

## 📁 2. Setup Project Directory

```bash
mkdir -p ~/projects
cd ~/projects
mkdir my-app
cd my-app
```

---

## 🌿 3. Clone Repository

### 🔹 Public Repo

```bash
git clone https://github.com/username/repo.git
```

### 🔹 Private Repo (Token Based)

```bash
git clone https://<TOKEN>@github.com/username/repo.git
```

---

## ⚙️ 4. Setup Environment Variables

```bash
cd repo-name
vi .env
```

### Editor usage:

* Press `i` → insert mode
* Paste content
* Press `ESC`
* Type `:wq` → save & exit

---

## 🔧 5. Install Node (via NVM)

```bash
sudo apt update
sudo apt install curl -y

curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
source ~/.bashrc
```

Install Node:

```bash
nvm install --lts
node -v
npm -v
```

---

## 📦 6. Install Dependencies & Run App

```bash
npm install
npm start
```

Test:

```
http://your_server_ip:PORT
```

---

## 🔥 7. Setup Firewall

```bash
sudo ufw enable
sudo ufw allow 22
sudo ufw allow 80
sudo ufw allow 443
sudo ufw status
```

---

## ⚡ 8. Setup PM2

```bash
npm install -g pm2
```

Start app:

```bash
pm2 start app.js --name "my-app"
```

Cluster mode (recommended):

```bash
pm2 start app.js -i max
```

Save process:

```bash
pm2 save
pm2 startup
```

---

## 🌐 9. Install Nginx

```bash
sudo apt install nginx
```

---

## ⚙️ 10. Nginx Configuration

Create config file:

```bash
sudo nano /etc/nginx/sites-available/my-app
```

---

### 🔹 Backend (Node API)

```nginx
server {
    listen 80;
    server_name yourdomain.com www.yourdomain.com;

    location / {
        proxy_pass http://localhost:8001;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }

    access_log /var/log/nginx/myapp_access.log;
    error_log /var/log/nginx/myapp_error.log;
}
```

---

### ⚛️ Frontend (React Build)

```nginx
server {
    listen 80;
    server_name admin.yourdomain.com;

    root /home/ubuntu/projects/my-app/build;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }

    access_log /var/log/nginx/admin_access.log;
    error_log /var/log/nginx/admin_error.log;
}
```

---

## 🔁 11. Enable Config

```bash
sudo ln -s /etc/nginx/sites-available/my-app /etc/nginx/sites-enabled/
```

---

## ✅ 12. Test & Restart Nginx

```bash
sudo nginx -t
sudo systemctl restart nginx
```

---

## 🔐 13. Setup SSL (HTTPS)

Install certbot:

```bash
sudo apt install certbot python3-certbot-nginx
```

Apply SSL:

```bash
sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com
```

Check renewal:

```bash
sudo certbot renew --dry-run
```

---

## 📦 Multiple Apps Setup (Optional)

You can create multiple configs:

```
/etc/nginx/sites-available/
  ├── api-app
  ├── admin-app
  ├── client-app
```

Each can have:

* Different domain/subdomain
* Different ports
* Separate logs

---

## ⚠️ Common Issues

* Port mismatch between Node & Nginx
* Nginx not restarted
* Wrong frontend build path
* PM2 not saved
* Firewall blocking ports

---

## 🎯 Deployment Flow

1. Connect via SSH
2. Create project folder
3. Clone repo
4. Setup `.env`
5. Install Node
6. Run app
7. Setup PM2
8. Install Nginx
9. Configure domain
10. Restart Nginx
11. Apply SSL

---

## 💥 Done!

Your app is now:

* ✅ Live
* ✅ Auto-restarting (PM2)
* ✅ Secured with HTTPS
* ✅ Production Ready 🚀

---

## 🤝 Contribution

Feel free to improve this guide and submit a PR.

---

## 📄 License

MIT
