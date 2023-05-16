# Install SSL on Odoo Server

## Install and configure SSL for Odoo

```
sudo apt install python3-certbot-nginx -y
```

## Run this command to receive certificates

```
sudo certbot --nginx certonly
```

###  Enter any email and agee to the terms and conditions. The Certbot client will automatically generate the new certificate for your domain. Now we need to update the Nginx config.

```
sudo rm  /etc/nginx/sites-enabled/odoo.conf
sudo rm  /etc/nginx/sites-available/odoo.conf
```

```
sudo nano /etc/nginx/sites-available/odoo.conf
```

## Redirect HTTP Traffic to HTTPS
### Open your site’s Nginx configuration file add replace everything with the following.

```
upstream odoo8069 {
     server 127.0.0.1:8069;
 }

 server {
     listen [::]:80;
     listen 80;

     server_name your-domain.com www.your-domain.com;

     return 301 https://your-domain.com$request_uri;
 }

 server {
     listen [::]:443 ssl;
     listen 443 ssl;

     server_name www.your-domain.com;

     ssl_certificate /etc/letsencrypt/live/your-domain.com/fullchain.pem;
     ssl_certificate_key /etc/letsencrypt/live/your-domain.com/privkey.pem;

     return 301 https://your-domain.com$request_uri;
 }

 server {
     listen [::]:443 ssl http2;
     listen 443 ssl http2;

     server_name your-domain.com;

     ssl_certificate /etc/letsencrypt/live/your-domain.com/fullchain.pem;
     ssl_certificate_key /etc/letsencrypt/live/your-domain.com/privkey.pem;

     access_log /var/log/nginx/odoo_access.log;
     error_log /var/log/nginx/odoo_error.log;

     proxy_read_timeout 720s;
     proxy_connect_timeout 720s;
     proxy_send_timeout 720s;
     proxy_set_header X-Forwarded-Host $host;
     proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
     proxy_set_header X-Forwarded-Proto $scheme;
     proxy_set_header X-Real-IP $remote_addr;

     location / {
        proxy_pass http://odoo8069;
     }

     location ~* /web/static/ {
         proxy_cache_valid 200 90m;
         proxy_buffering on;
         expires 864000;
         proxy_pass http://odoo8069;
     }

     gzip_types text/css text/less text/plain text/xml application/xml application/json application/javascript;
     gzip on;
 }
```
Hit CTRL+X followed by Y to save the changes.
###  Run the following command to symlink the file you just created with
```
/etc/nginx/sites-enabled/ directory
sudo ln -s /etc/nginx/sites-available/odoo.conf /etc/nginx/sites-enabled/odoo.conf
```

### Check your configuration and restart Nginx for the changes to take effect.
```
sudo nginx -t
sudo service nginx restart
```

That's it.