# Hosting a Website on a Server with Nginx

This guide will help you set up and host your website on a server using Nginx. Follow the steps below to configure your server and deploy your project.

## Prerequisites

- A server with Nginx installed
- Domain name pointed to your server's IP address
- Basic knowledge of using the terminal

## Steps

### 1. Create Project Directory

Create a directory for your project in the `/var/www` directory:

```bash
sudo mkdir /var/www/<PROJECTTITLE>
```

Set the appropriate permissions for the directory:

```bash
sudo chmod -R 755 /var/www/<PROJECTTITLE>
```

### 2. Create Nginx Configuration File

First, remove the default Nginx configuration file to avoid conflicts:

```bash
sudo rm /etc/nginx/sites-enabled/default
```

Create a new Nginx configuration file for your project:

```bash
sudo nano /etc/nginx/sites-available/<DOMAIN.COM>
```

Add the following basic configuration for serving static files

```bash
server {
    listen 80; # Listen on port 80 for IPv4
    listen [::]:80; # Listen on port 80 for IPv6

    server_name <DOMAIN.COM> <WWW.DOMAIN.COM>; # Domain names this server block responds to

    root /var/www/<PROJECTTITLE>; # Directory to serve files from
    index index.html; # Default file to serve if no specific file is requested

    location / {
        try_files $uri $uri/ =404; # Try to serve the file requested, if not found, return a 404 error
        # try_files $uri /index.html; # Uncomment this line to fallback to index.html if the requested file is not found
    }
}
```

### 3. Add Additional Location for Serving Static Files (Optional)

If you need to serve static files from an additional location, modify your configuration as follows:

```bash
server {
    listen 80; # Listen on port 80 for IPv4
    listen [::]:80; # Listen on port 80 for IPv6

    server_name <DOMAIN.COM> <WWW.DOMAIN.COM>; # Domain names this server block responds to

    #www.domain.com/project1

    location /project1 {
        alias /var/www/html/<PROJECTTITLE>; # Maps /filepath to /var/www/html/ResourceManagement
        index index.html; # Default file to serve if no specific file is requested in this location
        try_files $uri $uri/ /project1/index.html; # Try to serve the requested file, if not found, serve /filepath/index.html
    }

    #www.domain.com/project2

    location /project2 {
        alias /var/www/html/<PROJECTTITLE>; # Maps /filepath to /var/www/html/ResourceManagement
        index index.html; # Default file to serve if no specific file is requested in this location
        try_files $uri $uri/ /project2/index.html; # Try to serve the requested file, if not found, serve /filepath/index.html
    }

    location / {
        try_files $uri $uri/ =404; # Try to serve the file requested, if not found, return a 404 error
        # try_files $uri /index.html; # Uncomment this line to fallback to index.html if the requested file is not found
        # try_files $uri $uri/ /index.html; # Uncomment this line to first try the requested file and then fallback to index.html
    }
}
```

### 4. Enable the Site in Nginx

Create a symbolic link from the configuration file to the sites-enabled directory:

```bash
sudo ln -s /etc/nginx/sites-available/<DOMAIN.COM> /etc/nginx/sites-enabled/
```

### 5. Adjust Nginx Configuration (Optional)

Adjust the main Nginx configuration file if needed:

```bash
sudo nano /etc/nginx/nginx.conf
```

Uncomment or add the following line inside the http block:

```bash
http {
    ...
    server_names_hash_bucket_size 64; # Adjust this value if you have long domain names to avoid hash bucket issues
    ...
}
```

### 6. Check Nginx Configuration and Restart

Check the Nginx configuration for any syntax errors:

```bash
sudo nginx -t
```

If the configuration test is successful, restart Nginx to apply the changes:

```bash
sudo systemctl restart nginx
```
