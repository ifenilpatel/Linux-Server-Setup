# Setting Up SSL/TLS with Certbot

This guide will help you set up SSL/TLS for your Nginx server using Certbot. Follow the steps below to install Certbot, obtain an SSL/TLS certificate, and configure automatic renewal.

## Prerequisites

- A server with Nginx installed
- Domain name pointed to your server's IP address
- Basic knowledge of using the terminal

## Steps

### 1. Update Package List

Update your package list to ensure you have the latest information about available packages:

```bash
sudo apt update
```

### 2. Install Certbot and the Nginx Plugin

Install Certbot and the Nginx plugin to manage SSL/TLS certificates:

```bash
sudo apt install certbot python3-certbot-nginx
```

### 3. Obtain an SSL/TLS Certificate

Use Certbot to obtain an SSL/TLS certificate for your domain. Replace <DOMAIN.COM> and <WWW.DOMAIN.COM> with your actual domain names:

```bash
sudo certbot --nginx -d <DOMAIN.COM> -d <WWW.DOMAIN.COM>
```

Follow the prompts to complete the certificate installation. Certbot will automatically configure Nginx to use the new certificate.

### 4. Verify Certbot Auto-Renewal Service

Certbot includes a systemd service and timer to handle automatic renewal of your certificates. Verify that these services are running:

Check the status of the renewal service:

```bash
sudo systemctl status snap.certbot.renew.service
```

Check the status of the renewal timer:

```bash
sudo systemctl status certbot.timer
```

### 5. Test the Renewal Process

It's a good idea to test the renewal process to ensure there won't be any issues when your certificate needs to be renewed:

```bash
sudo certbot renew --dry-run
```

This command performs a dry run of the renewal process, allowing you to verify that everything is set up correctly without making any changes.
