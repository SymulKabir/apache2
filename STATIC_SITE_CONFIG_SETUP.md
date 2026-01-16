### How to setup static applicaiton domain

#### Motive
Configare `xyz.com` to your server

#### Create Your Static Website Folder

Website Root: `/var/www/xyz`

```bash
sudo mkdir -p /var/www/xyz
sudo nano /var/www/xyz/index.html
```
Add any simple content:

```html
<h1>Welcome to xyz.com!</h1>
<p>Your static site is live.</p>
```
Set correct permissions:

```bash
sudo chown -R $USER:$USER /var/www/xyz
sudo chmod -R 755 /var/www/xyz
```

#### Create Apache Virtual Host
```bash
sudo nano /etc/apache2/sites-available/xyz.com.conf
```
Paste the following configuration:

```apache
<VirtualHost *:80>
    ServerName xyz.com
    #ServerAlias www.xyz.com
    DocumentRoot /var/www/xyz

    <Directory /var/www/xyz>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/xyz.com_error.log
    CustomLog ${APACHE_LOG_DIR}/xyz.com_access.log combined
</VirtualHost>
```
`Save` and `exit`

#### Enable the Site and Restart Apache
```bash
sudo a2ensite xyz.com.conf
```
`(Optional)` if you need to disable this site

```bash
sudo a2dissite xyz.com.conf
```

Reload Apache:
```bash
sudo systemctl reload apache2
```

#### Update DNS Records for xyz.com
Go to your domain registrar (GoDaddy, Namecheap, etc.) and set:
```
Type	Host	Value	TTL
A	@	YOUR_SERVER_PUBLIC_IP	Default
A	www	YOUR_SERVER_PUBLIC_IP	Default
```
Wait 5–30 minutes for propagation.

**OR**
Set domain to your host

```bash
sudo nano /etc/hosts
```
Added this line bottom of the file
```nano
127.0.0.1       xyz.com
```
`save` and `exit`

#### Verify Installation
Open your browser:
```url
http://xyz.com
```
**OR**

```bash
curl http://xyz.com
```

### ⭐ Optional: Enable HTTPS with Free SSL (Let's Encrypt)

#### Install Certbot
```bash
sudo apt install certbot python3-certbot-apache -y
```

#### Run SSL setup:
```bash
sudo certbot --apache -d xyz.com -d www.zyz.com
```
Open your browser:
```url
https://xyz.com
```
**OR**

```bash
curl https://xyz.com
```

Done! Your site is now secured with HTTPS.

