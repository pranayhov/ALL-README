# 🚀 Node.js + React Deployment Guide (NGINX + PM2 + SSL)

This guide walks you through deploying a Node.js backend and React frontend on an Ubuntu-based server (e.g., AWS EC2) using **NGINX**, **PM2**, and **Certbot** for HTTPS.

---

## 📆 Update & Upgrade Your Server

```bash
sudo apt update && sudo apt upgrade -y
```

---

## ⚙️ Configure NGINX

### 🛠️ Edit NGINX Default Config

**Option 1: Using `vim`**
```bash
sudo vim /etc/nginx/sites-available/default
```
- To save & exit:
  - Press `Esc`, type `:wq!` then hit `Enter`.

**Option 2: Using `nano`**
```bash
sudo nano /etc/nginx/sites-available/default
```
- To save: `Ctrl + O`
- To exit: `Ctrl + X`

### 💾 Sample NGINX Config

```nginx
server {
    listen 80;
    server_name lab.reidatasolutions.com;

    root /home/ubuntu/your-repo/frontend/build;
    index index.html index.htm;

    location / {
        try_files $uri /index.html;
    }

    location /api {
        proxy_pass http://localhost:8000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

### 🔀 Test and Reload NGINX

```bash
sudo nginx -t
sudo systemctl reload nginx
```

---

## 🔐 Install SSL via Certbot (Let's Encrypt)

```bash
sudo snap install core; sudo snap refresh core
sudo apt install snapd -y
sudo snap install --classic certbot
```

### 📌 Issue SSL Certificate

```bash
sudo certbot --nginx -d lab.reidatasolutions.com
```

---

## 🧠 PM2 Cheat Sheet – Node.js Process Manager

Use [PM2](https://pm2.keymetrics.io/) to keep your Node.js applications alive forever and manage them easily.

### 🚀 Start Your App

```bash
pm2 start app.js
pm2 start app.js --name my-app
pm2 start app.js --name my-app --env production
```

### 📋 Process Management

```bash
pm2 list                # Show all running apps
pm2 monit               # Real-time monitoring
pm2 show my-app         # App-specific details
```

### ⭯️ Restart & Stop Apps

```bash
pm2 restart my-app      # Restart one app
pm2 restart all         # Restart all apps
pm2 stop my-app         # Stop one app
pm2 stop all            # Stop all apps
```

### ❌ Delete Apps

```bash
pm2 delete my-app       # Delete one app
pm2 delete all          # Delete all apps
```

### 📅 Persist Processes on Reboot

```bash
pm2 save                # Save current process list
pm2 startup             # Generate startup script
# Run the command it outputs
```

### 🔄 Reload Saved State

```bash
pm2 resurrect
```

### 📜 Logs

```bash
pm2 logs               
pm2 logs my-app        
pm2 flush               
```

---

## ✅ Final Checklist

- ✅ NGINX properly routes frontend and backend
- ✅ SSL installed with Certbot
- ✅ PM2 manages your Node.js server
- ✅ DNS A record set up in Hostinger

---

# Sudo Commands Cheat Sheet

## 📁 File & Directory Operations
```bash
sudo mkdir /opt/myfolder           # Create a folder in a root-level directory
sudo rm -rf /opt/myfolder          # Force delete a folder
sudo cp myfile.txt /etc/           # Copy file to a protected location
sudo mv /path/a /path/b            # Move or rename files/directories
```

## 📝 File Editing
```bash
sudo nano /etc/hosts               # Edit a file with nano
sudo vim /etc/nginx/nginx.conf     # Edit a file with Vim
sudo touch /etc/newfile.conf       # Create a new file
```

## ⚙️ System Management
```bash
sudo reboot                        # Reboot the system
sudo shutdown now                  # Shut down immediately
sudo systemctl start nginx         # Start a service
sudo systemctl stop apache2        # Stop a service
sudo systemctl status docker       # Check the status of a service
```

## 👤 User & Permissions
```bash
sudo useradd newuser               # Add a new user
sudo passwd newuser                # Set a user's password
sudo chown user:group file.txt     # Change file ownership
sudo chmod 755 script.sh           # Change file permissions
```

## 📦 Package Management (Debian/Ubuntu)
```bash
sudo apt update                    # Update package list
sudo apt upgrade                   # Upgrade installed packages
sudo apt install nginx             # Install a package
sudo apt remove nginx              # Remove a package
```

## 📦 Package Management (RHEL/CentOS/Fedora)
```bash
sudo yum install git               # Install a package with yum
sudo dnf install docker            # Install a package with dnf
```

## 👨‍💻 Developer Tasks
```bash
sudo git clone https://github.com/user/repo.git   # Clone a repo with sudo
sudo npm install -g create-react-app              # Install a global npm package
sudo systemctl restart nginx                      # Restart a service
```

## 💡 Tips
```bash
sudo -i                          # Start a root shell
sudo bash                        # Run a bash shell with root permissions
```

> 🛡️ Always double-check commands with `sudo` — you have the power to break things!



> 🚀 You're now ready to serve your app at:  
> **https://lab.reidatasolutions.com**
