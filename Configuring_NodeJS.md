# Node.js Application Setup and Hosting Guide

This guide provides step-by-step instructions to set up and host a Node.js application using NVM (Node Version Manager), PM2 for process management, and Nginx for proxying requests.

## Prerequisites

- A server with a Unix-based OS
- Root or sudo access to the server
- Basic knowledge of the command line

## Steps

### 1. Install NVM (Node Version Manager)

NVM allows you to manage multiple Node.js versions on your machine.

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
```

### 2. Source the Shell Profile

To start using NVM in the current terminal session, source your shell profile:

```bash
source ~/.bashrc
```

### 3. Install Node.js LTS Version Using NVM

List all available Node.js versions:

```bash
nvm list-remote
```

Install the Long Term Support (LTS) version of Node.js:

```bash
nvm install --lts
```

### 4. Verify the Installed Node.js Version

Check if Node.js is installed correctly:

```bash
node -v
```

### 5. Install PM2 for Process Management

PM2 is a production process manager for Node.js applications. Install PM2 globally:

```bash
sudo npm install pm2@latest -g
```

### 6. Set Up Node.js Project Directory

Navigate to the web directory and create your project folder:

```bash
cd /var/www
sudo mkdir <PROJECTTITLE>
```

Set the appropriate permissions for the project directory:

```bash
sudo chmod -R 755 <PROJECTTITLE>
```

### 7. Configure Nginx for Proxying Node.js Application

First, remove the default Nginx configuration file to avoid conflicts:

```bash
sudo rm /etc/nginx/sites-enabled/default
```

Open Nginx configuration file for your domain:

```bash
sudo nano /etc/nginx/sites-available/<DOMAIN.COM>
```

Add the following configuration to proxy requests to your Node.js application running on port 3000 (adjust as needed):

```bash
server {
    listen 80;
    server_name <DOMAIN.COM> <WWW.DOMAIN.COM>;

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

Save and close the file.

Enable the Nginx configuration by creating a symbolic link:

```bash
sudo ln -s /etc/nginx/sites-available/<DOMAIN.COM> /etc/nginx/sites-enabled/
```

### 8. Test the Nginx Configuration

Test the Nginx configuration for syntax errors:

```bash
sudo nginx -t
```

### 9. Reload Nginx to Apply Changes

If the test is successful, reload Nginx:

```bash
sudo systemctl reload nginx
```

### 10. Start Node.js Application with PM2

Navigate to your project directory:

```bash
cd /var/www/<PROJECTTITLE>
```

Start your Node.js application using PM2 (replace app.js with your main application file):

```bash
pm2 start app.js --name <PROJECTTITLE> --watch
```

### 11. Check the Status of PM2-Managed Applications

List all PM2-managed applications to ensure your app is running:

```bash
pm2 list
```

### 12. Auto-Start PM2 on System Boot

To ensure PM2 restarts your application on system reboots, set up PM2 to launch on startup:

```bash
pm2 startup
```

Follow the instructions provided by PM2 to complete the setup, and then save the PM2 process list:

```bash
pm2 save
```
