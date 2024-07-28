# Installing and Configuring Nginx on Ubuntu

Nginx is a powerful web server and reverse proxy server known for its high performance, stability, rich feature set, simple configuration, and low resource consumption. This guide will walk you through the steps to install, configure, and secure Nginx on an Ubuntu system.

## Step 1: Update the Package List

First, update the package list to ensure you have the latest information on the newest versions of packages and their dependencies.

```bash
sudo apt update
```

## Step 2: Install Nginx

Use the apt package manager to install Nginx.

```bash
sudo apt install nginx
```

## Step 3: Start and Enable Nginx

Start the Nginx service and enable it to start automatically on boot.

```bash
sudo systemctl start nginx
```

```bash
sudo systemctl enable nginx
```

## Step 4: Verify Nginx is Running

Check the status of the Nginx service to ensure it is running correctly.

```bash
sudo systemctl status nginx
```

## Step 5: Install and Configure UFW (Uncomplicated Firewall)

Install UFW to manage the firewall.

```bash
sudo apt install ufw
```

## Step 6: Enable the Firewall

Enable UFW to start enforcing its rules.

```bash
sudo ufw enable
```

## Step 7: Check the Firewall Status

Verify that UFW is active and the rules are being enforced.

```bash
sudo ufw status
```

## Step 8: Allow Full Access to Nginx Through the Firewall

Allow full access to Nginx through the firewall.

```bash
sudo ufw allow 'Nginx Full'
```

## Step 8: Allow SSH Connections

Allow SSH connections through the firewall to ensure you don't get locked out of your server.

```bash
sudo ufw allow ssh
```

## Step 9: Allow Other Ports Through the Firewall

If you need to allow traffic on other ports, use the following command, replacing <port> with the desired port number. For example, to allow traffic on port 8080:

```bash
sudo ufw allow 8080
```
