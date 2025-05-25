# Deployment

This section provides comprehensive guidance on deploying the Tonga National Portal to production environments for Tonga's government services, including server requirements, capacity planning, step-by-step deployment processes, environment setup, and performance optimization. The recommended configuration supports hundreds of concurrent users on a single server, which is more than sufficient for Tonga's population of approximately 110,000 citizens.

## Server Requirements

### Hardware Requirements

| Component | Requirement | Recommendation |
|-----------|-------------|----------------|
| CPU | Minimum 4 cores | 8+ cores for better performance |
| RAM | Minimum 16GB | 32GB for larger installations |
| Storage | Minimum 100GB SSD | 250GB+ SSD with RAID configuration |
| Network | High-speed internet connection with 1Gbps bandwidth | Redundant connections for high availability |

The hardware specifications above are designed to provide optimal performance for WordPress installations serving government websites. SSD storage is strongly recommended over traditional HDD due to the significant performance benefits, especially for database operations and content delivery.

### Capacity Planning

Capacity planning for the Tonga National Portal must account for the nation's population of approximately 110,000 citizens, with specific requirements to support 200 concurrent users. The portal is designed as a read-only informational website for citizens with minimal interactive elements.

| Metric | Estimated Capacity | Tonga Context |
|--------|-------------------|---------------|
| Concurrent active users | 200-300 | Meets requirement for 200 simultaneous users |
| Peak hourly visitors | 1,000+ | Sufficient for high-traffic periods |
| Database size | Up to 10GB | Adequate for content and media files |
| File storage | Up to 80GB | Sufficient for document storage and media library |

**User Interaction Profile:**
- Read-only access for general public (no user registration)
- Limited interactive elements:
  - Contact form submissions
  - Newsletter subscription forms
  - MxChat AI chatbox for content inquiries

With the recommended hardware configuration (4+ cores, 16GB RAM, 100GB SSD), the WordPress installation can comfortably support:

- 200-300 concurrent users browsing content and using the limited interactive features
- Several thousand concurrent passive sessions (browsing without interaction)
- Daily peaks of up to 5,000 unique visitors
- Efficient handling of contact form submissions and AI chatbox queries

This capacity significantly exceeds the expected requirements for Tonga's government services, providing headroom for future growth and peak usage scenarios. Since users don't need to register or log in, the system can handle more concurrent visitors with the same hardware resources compared to a site with complex user authentication and personalization features.

### Software Requirements

The WordPress installation requires specific software components to ensure optimal performance, security, and functionality.

| Software | Version | Notes |
|----------|---------|-------|
| WordPress | 6.5+ | Latest stable version recommended |
| PHP | 8.1+ | PHP 8.2 or 8.3 recommended for best performance |
| MySQL | 8.0+ | Primary database option |
| MariaDB | 10.6+ | Alternative to MySQL |
| Nginx | Latest stable | Preferred web server for performance |
| Apache | 2.4+ | Alternative web server |
| SSL Certificate | - | Required for HTTPS implementation |
| Redis | 6.0+ | Optional, for object caching |
| Memcached | 1.6+ | Alternative caching solution |
| Git | Latest stable | For version control |
| WP-CLI | Latest stable | Command-line interface for WordPress |

#### Required PHP Extensions

WordPress requires several PHP extensions to function properly. The following extensions must be installed and enabled:

- **Core Requirements:**
  - `mysqli` or `pdo_mysql`: For database connectivity
  - `curl`: For making remote requests
  - `gd` or `imagick`: For image processing
  - `json`: For data interchange
  - `mbstring`: For multi-byte string handling
  - `xml`: For XML processing
  - `zip`: For package handling

- **Performance Enhancements:**
  - `opcache`: For PHP bytecode caching
  - `apcu`: For object caching
  - `redis` or `memcached`: For persistent object caching (if using these systems)

- **Security Enhancements:**
  - `openssl`: For cryptographic operations
  - `filter`: For input validation
  - `hash`: For data hashing
  - `sodium`: For modern cryptography (WordPress 5.2+)

- **Internationalization:**
  - `intl`: For proper internationalization support
  - `iconv`: For character set conversion

To verify installed PHP extensions, you can use the following command:

```bash
php -m | grep -E 'mysqli|pdo_mysql|curl|gd|imagick|json|mbstring|xml|zip|opcache|apcu|redis|memcached|openssl|filter|hash|sodium|intl|iconv'
```

### Operating System

The platform must be deployed on:

- **Linux**:
  - Ubuntu 24.04 LTS

Ubuntu 24.04 LTS is the target deployment operating system for this application due to its long-term support, stability, security features, and optimal compatibility with the required software stack.

## Step-by-Step Deployment Process

### 1. Server Preparation

This section covers the initial setup of your Ubuntu 24.04 LTS server for WordPress 6.5+ deployment.

#### System Updates and Package Installation

First, ensure your system is up-to-date with the latest security patches and software:

```bash
# Update package lists and upgrade installed packages
sudo apt update
sudo apt upgrade -y

# Install essential utilities
sudo apt install -y software-properties-common curl wget gnupg2 git unzip
```

#### PHP 8.2 Installation

WordPress 6.5+ performs optimally with PHP 8.2 or higher:

```bash
# Add PHP repository
sudo add-apt-repository ppa:ondrej/php -y
sudo apt update

# Install PHP 8.2 and required extensions
sudo apt install -y php8.2-fpm php8.2-mysql php8.2-curl php8.2-gd php8.2-mbstring \
php8.2-xml php8.2-zip php8.2-bcmath php8.2-intl php8.2-soap php8.2-opcache \
php8.2-apcu php8.2-imagick php8.2-redis php8.2-iconv
```

#### Web Server Installation

Install and configure Nginx as the web server:

```bash
# Install Nginx
sudo apt install -y nginx

# Enable and start Nginx service
sudo systemctl enable nginx
sudo systemctl start nginx
```

#### Database Server Installation

Install MySQL 8.0 for database services:

```bash
# Install MySQL 8.0
sudo apt install -y mysql-server

# Enable and start MySQL service
sudo systemctl enable mysql
sudo systemctl start mysql
```

#### Firewall Configuration

Configure the firewall to allow web traffic and SSH:

```bash
# Allow Nginx HTTP and HTTPS traffic
sudo ufw allow 'Nginx Full'

# Allow SSH connections
sudo ufw allow ssh

# Enable firewall
sudo ufw enable

# Verify firewall status
sudo ufw status
```

#### Server Hardening Measures

Implement these security measures to protect your server:

1. **Secure SSH Configuration**:
   ```bash
   # Edit SSH configuration
   sudo nano /etc/ssh/sshd_config
   ```
   
   Make these changes:
   ```
   # Disable root login
   PermitRootLogin no
   
   # Use SSH key authentication only
   PasswordAuthentication no
   
   # Limit SSH access to specific users (optional)
   AllowUsers your_username
   
   # Change default SSH port (optional, enhances security)
   Port 2222
   ```
   
   Restart SSH service:
   ```bash
   sudo systemctl restart sshd
   ```

2. **Automatic Security Updates**:
   ```bash
   # Install unattended-upgrades package
   sudo apt install -y unattended-upgrades apt-listchanges
   
   # Configure automatic updates
   sudo dpkg-reconfigure -plow unattended-upgrades
   ```

3. **Install and Configure Fail2ban**:
   ```bash
   # Install fail2ban
   sudo apt install -y fail2ban
   
   # Create a local configuration file
   sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
   sudo nano /etc/fail2ban/jail.local
   ```
   
   Add these settings:
   ```
   [sshd]
   enabled = true
   port = ssh
   filter = sshd
   logpath = /var/log/auth.log
   maxretry = 5
   bantime = 3600
   
   [nginx-http-auth]
   enabled = true
   filter = nginx-http-auth
   port = http,https
   logpath = /var/log/nginx/error.log
   maxretry = 5
   bantime = 3600
   ```
   
   Start and enable fail2ban:
   ```bash
   sudo systemctl enable fail2ban
   sudo systemctl start fail2ban
   ```

4. **Secure Shared Memory**:
   ```bash
   # Edit fstab
   sudo nano /etc/fstab
   ```
   
   Add this line:
   ```
   tmpfs /run/shm tmpfs defaults,noexec,nosuid 0 0
   ```
   
   Apply changes:
   ```bash
   sudo mount -a
   ```

### 2. Database Setup

This section covers the setup and configuration of MySQL for WordPress.

#### Secure MySQL Installation

Run the MySQL secure installation script to set up root password and remove insecure defaults:

```bash
# Secure MySQL installation
sudo mysql_secure_installation
```

Follow the prompts to:
- Set a strong root password
- Remove anonymous users
- Disallow root login remotely
- Remove test database
- Reload privilege tables

#### Create WordPress Database and User

Connect to MySQL as root and create the database and user for WordPress:

```bash
# Connect to MySQL
sudo mysql -u root -p
```

Once logged into MySQL, execute these SQL commands:

```sql
# Create database with proper character set and collation
CREATE DATABASE tongaportal CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_520_ci;

# Create dedicated user with strong password
CREATE USER 'tongaportal'@'localhost' IDENTIFIED BY 'use_a_strong_unique_password_here';

# Grant privileges to the user
GRANT ALL PRIVILEGES ON tongaportal.* TO 'tongaportal'@'localhost';

# Apply changes
FLUSH PRIVILEGES;

# Exit MySQL
EXIT;
```

#### Verify Database Connection

Ensure the database is accessible with the new user:

```bash
# Test connection with the new user
mysql -u tongaportal -p -D tongaportal

# If successful, exit
EXIT;
```

#### MySQL Performance Optimization

For optimal WordPress performance, adjust MySQL configuration:

```bash
# Create a custom configuration file
sudo nano /etc/mysql/conf.d/wordpress.cnf
```

Add these performance settings:

```
[mysqld]
# InnoDB settings
innodb_buffer_pool_size = 1G
innodb_log_file_size = 256M
innodb_flush_method = O_DIRECT
innodb_flush_log_at_trx_commit = 2

# Connection settings
max_connections = 300
table_open_cache = 2000
thread_cache_size = 16

# Query cache settings
query_cache_type = 1
query_cache_size = 64M
query_cache_limit = 2M
```

Restart MySQL to apply changes:

```bash
sudo systemctl restart mysql
```

### 3. WordPress Installation

This section covers downloading, configuring, and securing WordPress 6.5+.

#### Download and Extract WordPress

Download the latest version of WordPress and set up the installation directory:

```bash
# Create web directory
sudo mkdir -p /var/www/tongaportal

# Download latest WordPress
cd /tmp
wget https://wordpress.org/latest.tar.gz

# Extract WordPress
tar -xzf latest.tar.gz

# Copy WordPress files to web directory
sudo cp -a /tmp/wordpress/. /var/www/tongaportal/

# Clean up
rm -rf /tmp/wordpress latest.tar.gz
```

#### Set Proper Permissions

Set secure ownership and permissions for WordPress files:

```bash
# Set ownership
sudo chown -R www-data:www-data /var/www/tongaportal

# Set directory permissions
sudo find /var/www/tongaportal -type d -exec chmod 755 {} \;

# Set file permissions
sudo find /var/www/tongaportal -type f -exec chmod 644 {} \;

# Create uploads directory with proper permissions
sudo mkdir -p /var/www/tongaportal/wp-content/uploads
sudo chmod 775 /var/www/tongaportal/wp-content/uploads
sudo chown -R www-data:www-data /var/www/tongaportal/wp-content/uploads
```

#### WordPress Configuration

Create and configure the WordPress configuration file:

```bash
# Navigate to WordPress directory
cd /var/www/tongaportal

# Create wp-config.php from sample
sudo cp wp-config-sample.php wp-config.php
sudo nano wp-config.php
```

Update the configuration with these optimized settings:

```php
// Database settings
define( 'DB_NAME', 'tongaportal' );
define( 'DB_USER', 'tongaportal' );
define( 'DB_PASSWORD', 'use_a_strong_unique_password_here' );
define( 'DB_HOST', 'localhost' );
define( 'DB_CHARSET', 'utf8mb4' );
define( 'DB_COLLATE', 'utf8mb4_unicode_520_ci' );

// Security keys - generate from https://api.wordpress.org/secret-key/1.1/salt/
define( 'AUTH_KEY',         'unique-value-here' );
define( 'SECURE_AUTH_KEY',  'unique-value-here' );
define( 'LOGGED_IN_KEY',    'unique-value-here' );
define( 'NONCE_KEY',        'unique-value-here' );
define( 'AUTH_SALT',        'unique-value-here' );
define( 'SECURE_AUTH_SALT', 'unique-value-here' );
define( 'LOGGED_IN_SALT',   'unique-value-here' );
define( 'NONCE_SALT',       'unique-value-here' );

// Table prefix - change for security
$table_prefix = 'wp_tonga_';

// Performance optimizations
define( 'WP_CACHE', true );
define( 'WP_MEMORY_LIMIT', '256M' );
define( 'WP_MAX_MEMORY_LIMIT', '512M' );

// Automatic updates
define( 'WP_AUTO_UPDATE_CORE', 'minor' );

// File system method - direct is preferred when possible
define( 'FS_METHOD', 'direct' );

// Disable post revisions or limit them
define( 'WP_POST_REVISIONS', 5 );

// Empty trash after 30 days
define( 'EMPTY_TRASH_DAYS', 30 );

// Disable editing of theme and plugin files from the admin
define( 'DISALLOW_FILE_EDIT', true );

// Debug settings (disable in production)
define( 'WP_DEBUG', false );
define( 'WP_DEBUG_LOG', false );
define( 'WP_DEBUG_DISPLAY', false );
```

Generate unique security keys from WordPress API:

```bash
# Generate security keys
curl -s https://api.wordpress.org/secret-key/1.1/salt/
```

Copy the output and replace the placeholder security key section in wp-config.php.

#### Secure wp-config.php

Protect the configuration file:

```bash
# Set restrictive permissions on wp-config.php
sudo chmod 600 /var/www/tongaportal/wp-config.php
sudo chown www-data:www-data /var/www/tongaportal/wp-config.php
```

#### Install WP-CLI (WordPress Command Line Interface)

Install WP-CLI for easier WordPress management:

```bash
# Download WP-CLI
curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar

# Make executable and move to path
chmod +x wp-cli.phar
sudo mv wp-cli.phar /usr/local/bin/wp

# Verify installation
wp --info
```

### 4. Web Server Configuration

This section covers configuring Nginx to serve WordPress securely and efficiently.

#### Create Nginx Server Block

Create a dedicated Nginx configuration for the WordPress site:

```bash
# Create server block configuration
sudo nano /etc/nginx/sites-available/tongaportal
```

Add this optimized configuration:

```nginx
server {
    listen 80;
    listen [::]:80;
    server_name www.gov.to;
    
    # Redirect HTTP to HTTPS (after SSL is configured)
    # return 301 https://$host$request_uri;
    
    root /var/www/tongaportal;
    index index.php index.html index.htm;
    
    # WordPress permalink structure
    location / {
        try_files $uri $uri/ /index.php?$args;
    }
    
    # Pass PHP scripts to FastCGI server
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.2-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
        
        # FastCGI optimizations
        fastcgi_buffer_size 128k;
        fastcgi_buffers 4 256k;
        fastcgi_busy_buffers_size 256k;
        fastcgi_read_timeout 300;
    }
    
    # Deny access to sensitive files
    location ~ /\.(?!well-known) {
        deny all;
    }
    
    # Deny access to wp-config.php
    location = /wp-config.php {
        deny all;
    }
    
    # Deny access to PHP files in the uploads directory
    location ~* /(?:uploads|files)/.*\.php$ {
        deny all;
    }
    
    # Static file handling with caching
    location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg|woff|woff2|ttf|eot|otf)$ {
        expires max;
        log_not_found off;
        access_log off;
        add_header Cache-Control "public, max-age=31536000";
    }
    
    # Favicon handling
    location = /favicon.ico {
        log_not_found off;
        access_log off;
    }
    
    # Robots.txt handling
    location = /robots.txt {
        log_not_found off;
        access_log off;
        allow all;
    }
    
    # Security headers
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-XSS-Protection "1; mode=block" always;
    add_header Referrer-Policy "strict-origin-when-cross-origin" always;
    
    # Gzip compression
    gzip on;
    gzip_vary on;
    gzip_proxied any;
    gzip_comp_level 6;
    gzip_types text/plain text/css text/xml application/json application/javascript application/xml+rss application/atom+xml image/svg+xml;
}

# HTTPS server block (uncomment after SSL certificate is installed)
# server {
#     listen 443 ssl http2;
#     listen [::]:443 ssl http2;
#     server_name www.gov.to;
#
#     # SSL certificate configuration
#     ssl_certificate /etc/letsencrypt/live/www.gov.to/fullchain.pem;
#     ssl_certificate_key /etc/letsencrypt/live/www.gov.to/privkey.pem;
#     ssl_trusted_certificate /etc/letsencrypt/live/www.gov.to/chain.pem;
#
#     # SSL parameters
#     ssl_protocols TLSv1.2 TLSv1.3;
#     ssl_prefer_server_ciphers on;
#     ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384;
#     ssl_session_cache shared:SSL:10m;
#     ssl_session_timeout 1d;
#     ssl_session_tickets off;
#     ssl_stapling on;
#     ssl_stapling_verify on;
#
#     # HSTS (comment out if testing)
#     add_header Strict-Transport-Security "max-age=63072000; includeSubDomains; preload" always;
#
#     # Root directory and index files
#     root /var/www/tongaportal;
#     index index.php index.html index.htm;
#
#     # Include the same location blocks as the HTTP server
#     # [Copy all location blocks from the HTTP server here]
# }
```

#### Enable the Site and Test Configuration

Enable the site configuration and verify Nginx settings:

```bash
# Enable site by creating symbolic link
sudo ln -s /etc/nginx/sites-available/tongaportal /etc/nginx/sites-enabled/

# Remove default site (optional)
sudo rm /etc/nginx/sites-enabled/default

# Test Nginx configuration
sudo nginx -t

# If test is successful, restart Nginx
sudo systemctl restart nginx
```

#### PHP-FPM Optimization

Optimize PHP-FPM for WordPress performance:

```bash
# Create custom PHP-FPM configuration
sudo nano /etc/php/8.2/fpm/pool.d/www.conf
```

Update these settings:

```
; Process manager settings
pm = dynamic
pm.max_children = 50
pm.start_servers = 5
pm.min_spare_servers = 5
pm.max_spare_servers = 35
pm.max_requests = 500

; Timeout settings
request_terminate_timeout = 300
```

Create a custom PHP configuration file:

```bash
# Create custom PHP configuration
sudo nano /etc/php/8.2/fpm/conf.d/99-wordpress.ini
```

Add these optimized settings:

```
; Memory limits
memory_limit = 256M
upload_max_filesize = 64M
post_max_size = 64M
max_execution_time = 300
max_input_vars = 3000

; OPcache settings
opcache.enable = 1
opcache.memory_consumption = 128
opcache.interned_strings_buffer = 8
opcache.max_accelerated_files = 10000
opcache.revalidate_freq = 2
opcache.save_comments = 1
```

Restart PHP-FPM to apply changes:

```bash
sudo systemctl restart php8.2-fpm
```

#### Verify Web Server Setup

Test that Nginx is serving the WordPress installation correctly:

```bash
# Check Nginx status
sudo systemctl status nginx

# Test site accessibility
curl -I http://localhost
```

You should see a "200 OK" response, indicating that Nginx is properly serving the WordPress files.

### 5. SSL Certificate Installation

#### Understanding Domain Restrictions

The `.gov.to` second-level domain (SLD) under the `.to` top-level domain (TLD) is a restricted namespace reserved for official government entities of Tonga. As such, it is subject to specific technical and administrative limitations:

- The domain `gov.to` is reserved and managed by the `.to` registry operator in accordance with country-specific policies.
- Let's Encrypt and other Certificate Authorities (CAs) may be unable to issue SSL certificates for the apex domain `gov.to` if domain validation cannot be completed at the root level (e.g., due to lack of DNS or HTTP challenge response at `gov.to`).
- SSL certificates **can** be issued for subdomains such as `www.gov.to`, provided the requester has control of the subdomain and completes standard domain validation procedures.

---

### Public Suffix List: Tonga

The [Public Suffix List (PSL)](https://publicsuffix.org/) is used by web browsers and Certificate Authorities to determine domain ownership boundaries. Domains listed in the PSL are considered "public suffixes" — domain spaces under which users **cannot** privately register subdomains or request SSL certificates for the base domain.

The following entries are part of the Tonga `.to` namespace and are treated as public suffixes:

```text
// to : https://www.iana.org/domains/root/db/to.html
// Submitted by registry <egullich@colo.to>
to
com.to
edu.to
gov.to
mil.to
net.to
org.to
```

As a result, SSL certificates **cannot** be issued for these apex domains (e.g., `gov.to`, `edu.to`), but they **can** be issued for valid subdomains like `www.gov.to` or `services.edu.to`, provided standard validation requirements are met.

---

For more information:
- [IANA Root Zone Database for `.to`](https://www.iana.org/domains/root/db/to.html)
- [Public Suffix List Documentation](https://publicsuffix.org/learn/)
- [Public Suffix List](https://publicsuffix.org/list/public_suffix_list.dat)


#### Using Let's Encrypt for Subdomains

Let's Encrypt provides free SSL certificates that can be used to secure the `www.gov.to` subdomain:

```bash
# Install Certbot and the Nginx plugin
sudo apt install -y certbot python3-certbot-nginx

# Obtain and install certificate for the www subdomain only
sudo certbot --nginx -d www.gov.to

# Verify auto-renewal is working properly
sudo certbot renew --dry-run
```

#### Important Notes

- Do not attempt to obtain a certificate for the root domain `gov.to` as the request will fail
- The certificate will be valid for the `www.gov.to` subdomain only
- Let's Encrypt certificates are valid for 90 days and will auto-renew if configured correctly
- Ensure your Nginx configuration properly handles redirects from HTTP to HTTPS
- The Certbot Nginx plugin will automatically update your Nginx configuration

#### Verification

After installation, verify the certificate is working correctly:

```bash
# Check certificate status
sudo certbot certificates

# Test the HTTPS connection
curl -I https://www.gov.to
```

Your site should now be accessible via HTTPS at `https://www.gov.to` with a valid SSL certificate.

#### Auto-Renewal Configuration

Let's Encrypt certificates are valid for 90 days. Certbot automatically configures a renewal service, but it's important to verify and ensure proper setup:

1. **Verify the auto-renewal timer is active**:

```bash
# Check the systemd timer status
sudo systemctl status certbot.timer

# View upcoming timers including certbot
sudo systemctl list-timers
```

2. **Manually configure auto-renewal** (if the automatic setup didn't work):

```bash
# Create a cron job for twice-daily renewal attempts
echo "0 0,12 * * * root python3 -c 'import random; import time; time.sleep(random.random() * 3600)' && certbot renew -q" | sudo tee -a /etc/crontab > /dev/null
```

3. **Test the renewal process**:

```bash
# Perform a dry run to test the renewal process
sudo certbot renew --dry-run
```

4. **Best practices for reliable renewals**:

- Ensure ports 80 and 443 are accessible from the internet for validation
- Set up renewal attempts to run twice daily (renewal only happens when needed)
- Add a random delay before renewal to distribute load on Let's Encrypt servers
- Configure monitoring to alert you if renewal fails
- Keep Certbot updated to the latest version

The renewal process is automatic and will attempt to renew certificates when they are 30 days from expiration. No manual intervention is required unless the renewal process fails.

### 6. Deploying Pre-configured WordPress Package

For the Tonga National Portal, a pre-configured WordPress package will be provided to the client. This package includes:

1. A zip archive containing all server files
2. A complete database backup

Follow these steps to deploy the pre-configured package:

#### 6.1 Deploy File System

```bash
# Create web directory if it doesn't exist
sudo mkdir -p /var/www/tongaportal

# Extract the provided zip archive to the web directory
sudo unzip /path/to/tongaportal_files.zip -d /var/www/tongaportal/

# Set proper ownership
sudo chown -R www-data:www-data /var/www/tongaportal

# Set proper permissions
sudo find /var/www/tongaportal -type d -exec chmod 755 {} \;
sudo find /var/www/tongaportal -type f -exec chmod 644 {} \;
sudo chmod 600 /var/www/tongaportal/wp-config.php
sudo chmod 775 /var/www/tongaportal/wp-content/uploads
```

#### 6.2 Import Database

```bash
# Create database if it doesn't exist already
sudo mysql -u root -p -e "CREATE DATABASE IF NOT EXISTS tongaportal CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_520_ci;"
sudo mysql -u root -p -e "GRANT ALL PRIVILEGES ON tongaportal.* TO 'tongaportal'@'localhost';"
sudo mysql -u root -p -e "FLUSH PRIVILEGES;"

# Import the provided database backup
sudo mysql -u root -p tongaportal < /path/to/tongaportal_database.sql
```

#### 6.3 Update Site URL

If the domain name needs to be updated to match the new server:

```bash
# Update site URL in the database
cd /var/www/tongaportal
sudo -u www-data wp search-replace 'original-domain.com' 'www.gov.to' --all-tables

# Clear cache
sudo -u www-data wp cache flush
```

#### 6.4 Verify Configuration

```bash
# Check WordPress configuration
cd /var/www/tongaportal
sudo -u www-data wp config list

# Verify database connection
sudo -u www-data wp db check
```

#### 6.5 Update wp-config.php

If the database credentials or other settings need to be updated:

```bash
# Edit wp-config.php
sudo nano /var/www/tongaportal/wp-config.php
```

Update the following values to match your server configuration:

```php
// Database settings
define( 'DB_NAME', 'tongaportal' );
define( 'DB_USER', 'tongaportal' );
define( 'DB_PASSWORD', 'your_database_password' );
define( 'DB_HOST', 'localhost' );

// Site URL settings (if needed)
define( 'WP_HOME', 'https://www.gov.to' );
define( 'WP_SITEURL', 'https://www.gov.to' );
```

#### 6.6 Regenerate Salts

For security reasons, it's recommended to regenerate the security keys:

```bash
# Generate new security keys
curl -s https://api.wordpress.org/secret-key/1.1/salt/
```

Replace the existing security keys in wp-config.php with the newly generated ones.

### 9. Domain and DNS Configuration

This section provides comprehensive guidance on configuring domain names and DNS settings for the Tonga National Portal WordPress installation. Proper DNS configuration is essential for making your website accessible to users and ensuring all services function correctly.

#### 9.1 Understanding DNS Records

Domain Name System (DNS) records are instructions that link your domain name to various services, most importantly to your web server's IP address. Think of DNS as a phone book that translates human-readable domain names (like www.gov.to) into machine-readable IP addresses.

#### 9.2 Required DNS Records for WordPress

For a standard WordPress installation like the Tonga National Portal, you'll need to configure these essential DNS records:

##### 9.2.1 Primary Domain Records

```
# A Records (IPv4)
www.gov.to.     IN    A    [YOUR_SERVER_IP]
gov.to.         IN    A    [YOUR_SERVER_IP]  # Apex/root domain

# AAAA Records (IPv6, if supported)
www.gov.to.     IN    AAAA    [YOUR_IPv6_ADDRESS]
gov.to.         IN    AAAA    [YOUR_IPv6_ADDRESS]
```

> **Note for Tonga Government Domain:** As mentioned in the SSL Certificate section, the `.gov.to` domain is a restricted namespace. When configuring DNS, ensure you're working with the authorized domain registrar for Tonga government domains.

##### 9.2.2 Email-Related Records

If you plan to use email services with your domain:

```
# MX Records (for email delivery)
gov.to.    IN    MX    10 mail.gov.to.    # Primary mail server
gov.to.    IN    MX    20 mail2.gov.to.   # Backup mail server

# SPF Record (to prevent email spoofing)
gov.to.    IN    TXT   "v=spf1 ip4:[YOUR_SERVER_IP] include:_spf.google.com ~all"

# DKIM Record (if using DKIM email authentication)
selector._domainkey.gov.to.    IN    TXT    "v=DKIM1; k=rsa; p=MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQC..."

# DMARC Record (email authentication policy)
_dmarc.gov.to.    IN    TXT    "v=DMARC1; p=quarantine; rua=mailto:dmarc@gov.to"
```

##### 9.2.3 Additional Service Records

For enhanced functionality and security:

```
# For Let's Encrypt SSL verification (if needed)
_acme-challenge.www.gov.to.    IN    TXT    "verification_token_here"

# For Google Search Console verification
gov.to.    IN    TXT    "google-site-verification=token_here"

# For subdomain services (if applicable)
api.gov.to.       IN    A    [YOUR_SERVER_IP]
services.gov.to.  IN    A    [YOUR_SERVER_IP]
```

#### 9.3 Step-by-Step DNS Configuration

Follow these steps to configure your DNS records with your domain registrar:

1. **Log in to your domain registrar's control panel**
   - This is typically the company where you registered your domain name
   - For Tonga government domains, contact the authorized administrator

2. **Locate the DNS management section**
   - This might be called "DNS Settings," "Name Servers," or "Domain Management"

3. **Add or modify A records**
   ```
   # For the www subdomain
   Type: A
   Host/Name: www
   Value/Points to: [YOUR_SERVER_IP]
   TTL: 3600 (or 1 hour)
   
   # For the apex/root domain
   Type: A
   Host/Name: @ (or leave blank)
   Value/Points to: [YOUR_SERVER_IP]
   TTL: 3600 (or 1 hour)
   ```

4. **Add MX records for email (if applicable)**
   ```
   Type: MX
   Host/Name: @ (or leave blank)
   Priority: 10
   Value/Points to: mail.gov.to
   TTL: 3600 (or 1 hour)
   ```

5. **Add TXT records for email authentication and verification**
   ```
   Type: TXT
   Host/Name: @ (or leave blank)
   Value/Text: "v=spf1 ip4:[YOUR_SERVER_IP] include:_spf.google.com ~all"
   TTL: 3600 (or 1 hour)
   ```

6. **Save your changes**
   - Different registrars have different interfaces, but look for a "Save," "Apply," or "Update" button

#### 9.4 DNS Propagation and Verification

After configuring your DNS records, changes need time to propagate throughout the global DNS system:

1. **Wait for propagation**
   - DNS changes typically take 15 minutes to 48 hours to fully propagate
   - During this time, different users might see different versions of your site

2. **Verify A records**
   ```bash
   # Check the A record for your domain
   dig +short www.gov.to A
   dig +short gov.to A
   
   # Expected output: [YOUR_SERVER_IP]
   ```

3. **Verify MX records**
   ```bash
   # Check MX records
   dig +short gov.to MX
   
   # Expected output: 10 mail.gov.to.
   ```

4. **Verify TXT records**
   ```bash
   # Check TXT records
   dig +short gov.to TXT
   
   # Expected output should include your SPF record
   ```

5. **Online DNS verification tools**
   - Use online tools like [MXToolbox](https://mxtoolbox.com/SuperTool.aspx) or [DNSChecker](https://dnschecker.org/) to verify propagation across different regions

#### 9.5 Troubleshooting Common DNS Issues

| Issue | Possible Cause | Solution |
|-------|---------------|----------|
| Website not accessible | DNS not propagated or incorrect A record | Verify A records and wait for propagation |
| "Server not found" error | DNS resolution failure | Check nameserver configuration |
| Email delivery issues | Incorrect MX records | Verify MX record configuration |
| Subdomain not working | Missing subdomain DNS record | Add A or CNAME record for the subdomain |
| SSL certificate errors | DNS validation failure | Verify TXT records for domain validation |

**Common Troubleshooting Commands:**

```bash
# Check if domain resolves to correct IP
nslookup www.gov.to

# Trace DNS resolution path
dig +trace www.gov.to

# Check DNS propagation across multiple servers
dig @8.8.8.8 www.gov.to  # Google DNS
dig @1.1.1.1 www.gov.to  # Cloudflare DNS
```

#### 9.6 DNS Best Practices for WordPress Sites

1. **Use www and non-www versions**
   - Configure both www.gov.to and gov.to to point to your server
   - Choose one as canonical and redirect the other via WordPress settings

2. **Set appropriate TTL values**
   - Use lower TTL values (300-900 seconds) before planned changes
   - Use higher TTL values (3600+ seconds) for stable production sites

3. **Implement DNSSEC for enhanced security**
   - If supported by your registrar, enable DNSSEC to prevent DNS spoofing
   ```bash
   # Verify DNSSEC is enabled
   dig +dnssec gov.to
   ```

4. **Monitor DNS health**
   - Set up monitoring for your DNS records to detect unauthorized changes
   - Use services like UptimeRobot or StatusCake for DNS monitoring

5. **Document your DNS configuration**
   - Maintain documentation of all DNS records and their purposes
   - Include contact information for the domain registrar and DNS provider

6. **Use DNS redundancy**
   - Consider using multiple DNS providers for critical services
   - Services like AWS Route 53 or Cloudflare provide robust DNS infrastructure

#### 9.7 Special Considerations for Tonga Government Domains

The `.gov.to` domain space has specific requirements and restrictions:

1. **Authorization requirements**
   - Only authorized government entities can register and manage `.gov.to` domains
   - Changes may require approval from the domain administrator

2. **Contact information**
   - Maintain updated technical contact information with the domain administrator
   - Establish a clear process for requesting DNS changes

3. **DNS management responsibility**
   - Determine whether DNS is managed by:
     - The central IT department
     - The individual government agency
     - A contracted service provider

4. **Change management process**
   - Document the process for requesting and implementing DNS changes
   - Include approval workflows and emergency change procedures

By following these guidelines, you'll ensure proper domain and DNS configuration for your WordPress installation, providing a reliable foundation for the Tonga National Portal.


## Caching Strategies

Implementing effective caching is critical for optimizing WordPress performance, especially for a government portal like the Tonga National Portal that needs to serve content efficiently to citizens. This section provides comprehensive guidance on implementing multi-layered caching strategies to significantly improve site speed, reduce server load, and enhance user experience.

### Understanding WordPress Caching Layers

WordPress sites benefit from a multi-layered caching approach, with each layer addressing different performance bottlenecks:

| Caching Layer | Description | Performance Impact |
|---------------|-------------|-------------------|
| Object Cache | Stores database query results in memory | Reduces database load by 30-70% |
| Page Cache | Stores complete HTML output of pages | Reduces server processing by 90-99% |
| Browser Cache | Stores static assets in visitors' browsers | Reduces bandwidth usage by 30-80% |
| CDN Cache | Distributes static content across global servers | Improves global access speeds by 40-80% |
| Database Cache | Optimizes database query performance | Reduces query execution time by 20-50% |

### 1. Object Caching Implementation

Object caching stores the results of complex database queries and API calls in memory, significantly reducing database load and PHP execution time.

#### 1.1 Redis Object Cache Setup

Redis provides persistent object caching with high performance and reliability. For the Tonga National Portal, we recommend Redis over Memcached due to its persistence features and broader data structure support.

```bash
# Install Redis server and PHP extension (updated for PHP 8.2)
sudo apt install -y redis-server php8.2-redis

# Secure Redis configuration
sudo nano /etc/redis/redis.conf
```

Apply these optimized Redis settings:

```
# Basic configuration
port 6379
bind 127.0.0.1
protected-mode yes
daemonize yes

# Performance tuning
maxmemory 1gb
maxmemory-policy allkeys-lru
maxmemory-samples 10

# Persistence settings
save 900 1
save 300 10
save 60 10000
rdbcompression yes

# Connection settings
timeout 300
tcp-keepalive 60
```

Restart and enable Redis:

```bash
sudo systemctl restart redis-server
sudo systemctl enable redis-server
sudo systemctl status redis-server
```

#### 1.2 WordPress Redis Integration

Install and configure the Redis Object Cache plugin:

```bash
# Navigate to WordPress directory
cd /var/www/tongaportal

# Install Redis Object Cache plugin
sudo -u www-data wp plugin install redis-cache --activate

# Configure and enable Redis
sudo -u www-data wp config set WP_REDIS_HOST 127.0.0.1 --raw
sudo -u www-data wp config set WP_REDIS_PORT 6379 --raw
sudo -u www-data wp config set WP_REDIS_TIMEOUT 1 --raw
sudo -u www-data wp config set WP_REDIS_READ_TIMEOUT 1 --raw
sudo -u www-data wp config set WP_REDIS_DATABASE 0 --raw
sudo -u www-data wp config set WP_CACHE_KEY_SALT 'tongaportal_' --raw

# Enable Redis object caching
sudo -u www-data wp redis enable
```

#### 1.3 Verifying Redis Object Cache

Confirm Redis is working correctly:

```bash
# Check Redis connection status
sudo -u www-data wp redis status

# Monitor Redis performance
redis-cli info stats
redis-cli info memory

# Test Redis hit rate after site has been active
redis-cli
> info stats
```

Look for metrics like `keyspace_hits` and `keyspace_misses` to calculate the hit rate. A hit rate above 80% indicates effective caching.

### 2. Page Caching Implementation

Page caching stores the complete HTML output of pages, eliminating PHP execution and database queries for repeat visitors.

#### 2.1 Server-Level Page Caching with Nginx FastCGI Cache

For optimal performance, implement server-level caching with Nginx FastCGI Cache:

```bash
# Create cache directory
sudo mkdir -p /var/cache/nginx/fastcgi_cache
sudo chown -R www-data:www-data /var/cache/nginx/fastcgi_cache

# Configure Nginx for FastCGI caching
sudo nano /etc/nginx/conf.d/fastcgi-cache.conf
```

Add the following configuration:

```nginx
# FastCGI cache settings
fastcgi_cache_path /var/cache/nginx/fastcgi_cache levels=1:2
                   keys_zone=WORDPRESS:100m
                   inactive=60m
                   max_size=1g
                   use_temp_path=off;

# FastCGI cache key definition
fastcgi_cache_key "$scheme$request_method$host$request_uri";

# Cache valid responses for different HTTP status codes
fastcgi_cache_valid 200 301 302 60m;
fastcgi_cache_valid 404 10m;

# Cache bypass and cache control headers
fastcgi_cache_bypass $http_pragma;
fastcgi_cache_bypass $http_authorization;
fastcgi_cache_bypass $cookie_wordpress_logged_in;
fastcgi_cache_bypass $request_method;

# Add cache status to response headers
add_header X-FastCGI-Cache $upstream_cache_status;
```

Update your Nginx server block:

```bash
sudo nano /etc/nginx/sites-available/tongaportal
```

Modify the PHP location block:

```nginx
# Pass PHP scripts to FastCGI server with caching
location ~ \.php$ {
    include snippets/fastcgi-php.conf;
    fastcgi_pass unix:/var/run/php/php8.2-fpm.sock;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    include fastcgi_params;
    
    # FastCGI caching
    fastcgi_cache WORDPRESS;
    fastcgi_cache_bypass $cookie_wordpress_logged_in;
    fastcgi_no_cache $cookie_wordpress_logged_in;
    
    # Skip caching for WordPress admin, API, and login pages
    set $skip_cache 0;
    if ($request_uri ~* "/wp-admin/|/wp-json/|/wp-login.php") {
        set $skip_cache 1;
    }
    if ($http_cookie ~* "wordpress_logged_in") {
        set $skip_cache 1;
    }
    if ($request_method = POST) {
        set $skip_cache 1;
    }
    fastcgi_cache_bypass $skip_cache;
    fastcgi_no_cache $skip_cache;
    
    # FastCGI optimizations
    fastcgi_buffer_size 128k;
    fastcgi_buffers 4 256k;
    fastcgi_busy_buffers_size 256k;
    fastcgi_read_timeout 300;
}
```

Test and restart Nginx:

```bash
sudo nginx -t
sudo systemctl restart nginx
```

#### 2.2 WordPress Page Caching with WP Super Cache

As a complementary approach, implement WP Super Cache for application-level caching:

```bash
# Install WP Super Cache
cd /var/www/tongaportal
sudo -u www-data wp plugin install wp-super-cache --activate
```

Configure WP Super Cache with these optimized settings:

1. **Caching**: Enable caching and select "Use mod_rewrite to serve cache files"
2. **Cache Delivery Method**: Use mod_rewrite (fastest method)
3. **Cache Timeout**: Set to 3600 seconds (1 hour)
4. **Content Compression**: Enable gzip compression
5. **Cache Restrictions**:
   - Don't cache pages for logged-in users
   - Don't cache pages with GET parameters
   - Don't cache pages with cookies
   - Cache rebuild (enable for high-traffic sites)
6. **Advanced Settings**:
   - Enable 304 Not Modified browser caching
   - Extra homepage checks (enable)
   - Only refresh current page when comments made
   - Compress pages with gzip
   - 404 caching (enable to prevent overloading on missing files)

Add the following to your wp-config.php to optimize WP Super Cache:

```php
// WP Super Cache configuration
define('WP_CACHE', true);
define('WPCACHEHOME', '/var/www/tongaportal/wp-content/plugins/wp-super-cache/');
```

#### 2.3 Cache Preloading Strategy

Implement cache preloading to ensure pages are cached before users visit them:

```bash
# Set up a cron job to preload the cache every 6 hours
echo "0 */6 * * * www-data cd /var/www/tongaportal && wp super-cache preload" | sudo tee -a /etc/crontab
```

Configure preload settings in WP Super Cache:
- Enable preload mode
- Preload homepage and all published posts/pages
- Refresh preloaded content every 6 hours
- Preload 10 posts at a time (adjust based on server capacity)

### 3. Browser Caching Implementation

Browser caching instructs visitors' browsers to store static assets locally, reducing bandwidth usage and load times for repeat visits.

#### 3.1 Configure Nginx for Browser Caching

Add or update browser caching directives in your Nginx configuration:

```bash
sudo nano /etc/nginx/sites-available/tongaportal
```

Add these optimized browser caching rules:

```nginx
# Browser caching for static assets
location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg|woff|woff2|ttf|eot|otf)$ {
    expires max;
    log_not_found off;
    access_log off;
    add_header Cache-Control "public, max-age=31536000, immutable";
    add_header X-Content-Type-Options nosniff;
    add_header X-XSS-Protection "1; mode=block";
}

# Cache HTML and XML files briefly
location ~* \.(html|xml)$ {
    expires 30m;
    add_header Cache-Control "public, max-age=1800";
}

# Special handling for webfonts
location ~* \.(woff|woff2|ttf|eot|otf)$ {
    add_header Access-Control-Allow-Origin "*";
}
```

#### 3.2 WordPress Cache-Control Headers

Add cache control headers for dynamic content by installing and configuring the Cache-Control plugin:

```bash
cd /var/www/tongaportal
sudo -u www-data wp plugin install cache-control --activate
```

Configure the plugin with these recommended settings:
- Set Cache-Control max-age to 3600 for posts and pages
- Set Cache-Control max-age to 86400 for category and tag archives
- Enable "public" directive for all cacheable content
- Add "stale-while-revalidate" directive for improved performance

### 4. CDN Integration

A Content Delivery Network (CDN) distributes static assets across global servers, reducing latency for users across Tonga's islands.

#### 4.1 Cloudflare CDN Setup

Cloudflare provides a robust CDN with a generous free tier suitable for the Tonga National Portal:

1. **Create a Cloudflare account** and add the domain (www.gov.to)
2. **Update nameservers** with your domain registrar to point to Cloudflare
3. **Configure Cloudflare settings**:
   - SSL: Full (strict)
   - Caching Level: Standard
   - Browser Cache TTL: 4 hours
   - Always Online: Enabled
   - Auto Minify: Enable for HTML, CSS, and JavaScript
   - Brotli Compression: Enabled
   - Rocket Loader: Enabled (for JavaScript optimization)
   - Argo Smart Routing: Consider enabling for improved global performance

#### 4.2 WordPress CDN Integration

Install and configure the official Cloudflare plugin:

```bash
cd /var/www/tongaportal
sudo -u www-data wp plugin install cloudflare --activate
```

Configure the plugin with your Cloudflare API credentials and enable these features:
- Automatic Platform Optimization
- Development Mode toggle
- Cache purge functionality
- Automatic HTTPS rewrites

#### 4.3 CDN Cache Purging Strategy

Implement automated cache purging when content changes:

```bash
# Install WP-CLI Cloudflare command
cd /var/www/tongaportal
sudo -u www-data wp package install cloudflare/cli-plugin:@stable

# Set up a hook to purge CDN cache when content is updated
sudo -u www-data wp config set CLOUDFLARE_API_KEY 'your_api_key' --raw
sudo -u www-data wp config set CLOUDFLARE_EMAIL 'your_email@example.com' --raw
```

### 5. Database Caching and Optimization

Optimize database performance to complement caching strategies.

#### 5.1 MySQL Query Cache Configuration

Configure MySQL query cache for frequently executed queries:

```bash
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```

Add or update these settings:

```
# Query cache configuration
query_cache_type = 1
query_cache_size = 128M
query_cache_limit = 2M
query_cache_min_res_unit = 4K
```

Restart MySQL:

```bash
sudo systemctl restart mysql
```

#### 5.2 Database Optimization Maintenance

Schedule regular database optimization tasks:

```bash
# Create a weekly database optimization script
cat > /usr/local/bin/wp-db-optimize.sh << 'EOF'
#!/bin/bash
cd /var/www/tongaportal
wp db optimize
wp transient delete --all
wp cache flush
EOF

# Make the script executable
sudo chmod +x /usr/local/bin/wp-db-optimize.sh

# Add to crontab to run weekly
echo "0 2 * * 0 www-data /usr/local/bin/wp-db-optimize.sh" | sudo tee -a /etc/crontab
```

### 6. Monitoring and Maintaining Cache Performance

Implement monitoring to ensure caching systems remain effective.

#### 6.1 Cache Monitoring Tools

Install and configure monitoring tools:

```bash
# Install New Relic for performance monitoring (optional)
curl -Ls https://download.newrelic.com/install/newrelic-cli/scripts/install.sh | bash
sudo NEW_RELIC_API_KEY=your_key NEW_RELIC_ACCOUNT_ID=your_account_id /usr/local/bin/newrelic install

# Install and configure Query Monitor plugin for WordPress
cd /var/www/tongaportal
sudo -u www-data wp plugin install query-monitor --activate
```

#### 6.2 Cache Performance Testing

Regularly test cache effectiveness:

```bash
# Test page load speed with and without cache
curl -s -w "%{time_total}\n" -o /dev/null https://www.gov.to/
curl -s -w "%{time_total}\n" -o /dev/null -H "Cache-Control: no-cache" https://www.gov.to/

# Check Redis cache hit rate
redis-cli info stats | grep hit_rate

# Test Nginx FastCGI cache
curl -I https://www.gov.to/ | grep X-FastCGI-Cache
```

#### 6.3 Cache Maintenance Schedule

Implement a regular cache maintenance schedule:

| Task | Frequency | Command |
|------|-----------|---------|
| Flush Redis cache | Monthly | `wp redis flush` |
| Purge page cache | After major updates | `wp super-cache flush` |
| Rebuild preloaded cache | Weekly | `wp super-cache preload` |
| Purge CDN cache | After theme/plugin updates | `wp cloudflare purge` |
| Database optimization | Weekly | `wp db optimize` |
| Transient cleanup | Weekly | `wp transient delete --all` |

Create a comprehensive maintenance script:

```bash
# Create maintenance script
cat > /usr/local/bin/wp-cache-maintenance.sh << 'EOF'
#!/bin/bash
cd /var/www/tongaportal

# Log start time
echo "Cache maintenance started at $(date)" >> /var/log/wp-cache-maintenance.log

# Optimize database
wp db optimize
wp transient delete --all

# Flush and rebuild caches
wp redis flush
wp super-cache flush
wp super-cache preload

# Purge CDN cache if needed
# wp cloudflare purge

echo "Cache maintenance completed at $(date)" >> /var/log/wp-cache-maintenance.log
EOF

# Make executable
sudo chmod +x /usr/local/bin/wp-cache-maintenance.sh

# Schedule monthly execution
echo "0 1 1 * * www-data /usr/local/bin/wp-cache-maintenance.sh" | sudo tee -a /etc/crontab
```

### 7. Caching Best Practices for Tonga National Portal

#### 7.1 Caching Strategy for Low-Bandwidth Areas

Tonga's archipelago includes remote islands with varying internet connectivity. Optimize for low-bandwidth areas:

1. **Aggressive Browser Caching**: Extend browser cache duration for remote areas
2. **Image Optimization**: Implement WebP images with proper caching
3. **Offline Capabilities**: Consider implementing Service Workers for critical content

```bash
# Install and configure WebP Express plugin
cd /var/www/tongaportal
sudo -u www-data wp plugin install webp-express --activate
```

#### 7.2 Cache Exclusions for Dynamic Content

Identify and exclude dynamic content from caching:

1. **Government Announcements**: Consider shorter cache times for critical updates
2. **Emergency Notifications**: Bypass cache for emergency content
3. **User-Specific Content**: Ensure personalized content is never cached

Add these exclusions to WP Super Cache:
- URLs containing "emergency"
- Pages with the template "announcement-template.php"
- Pages with the category "critical-updates"

#### 7.3 Cache Security Considerations

Implement security measures for cached content:

1. **Cache Poisoning Prevention**: Validate cache keys and sources
2. **Private Information**: Ensure personal data is never cached
3. **Cache Access Control**: Restrict direct access to cache directories

```bash
# Secure cache directories
sudo chmod 750 /var/cache/nginx/fastcgi_cache
sudo chown -R www-data:www-data /var/cache/nginx/fastcgi_cache

# Add protection to wp-content/cache
sudo nano /var/www/tongaportal/wp-content/cache/.htaccess
```

Add this content to the .htaccess file:

```
# Deny direct access to cache files
Order deny,allow
Deny from all
```

### 8. Troubleshooting Common Caching Issues

| Issue | Symptoms | Solution |
|-------|----------|----------|
| Stale cache | Outdated content displayed | Implement cache invalidation hooks |
| Cache not working | High server load, slow pages | Verify cache hit rates and configuration |
| Logged-in users experience issues | Functionality breaks for admins | Adjust cache exclusion rules |
| Mobile/desktop inconsistency | Different content on different devices | Configure device-specific cache rules |
| Plugin conflicts | Unexpected behavior after caching | Test plugins in isolation with caching |

#### 8.1 Debugging Redis Object Cache

If Redis object caching issues occur:

```bash
# Check Redis connection
redis-cli ping

# Verify Redis is running
sudo systemctl status redis-server

# Check Redis memory usage
redis-cli info memory

# Test WordPress Redis connection
cd /var/www/tongaportal
sudo -u www-data wp redis info
```

#### 8.2 Resolving Page Cache Issues

For page caching problems:

```bash
# Clear all page caches
cd /var/www/tongaportal
sudo -u www-data wp super-cache flush

# Verify cache directory permissions
sudo find /var/www/tongaportal/wp-content/cache -type d -exec chmod 755 {} \;
sudo find /var/www/tongaportal/wp-content/cache -type f -exec chmod 644 {} \;

# Check Nginx cache configuration
sudo nginx -t
```

### 9. Performance Benchmarking

Regularly benchmark caching performance to ensure optimal configuration:

```bash
# Install Apache Benchmark
sudo apt install -y apache2-utils

# Benchmark homepage with 100 requests, 10 concurrent
ab -n 100 -c 10 https://www.gov.to/

# Compare cached vs. non-cached performance
ab -n 100 -c 10 https://www.gov.to/
ab -n 100 -c 10 -H "Cache-Control: no-cache" https://www.gov.to/
```

Document baseline performance metrics and monitor changes over time:

| Metric | Target Value | Monitoring Method |
|--------|--------------|------------------|
| Time to First Byte | < 200ms | Chrome DevTools, WebPageTest |
| Full page load time | < 2 seconds | GTmetrix, PageSpeed Insights |
| Cache hit rate | > 80% | Redis stats, server logs |
| Server response time | < 300ms | New Relic, Query Monitor |
| Database query time | < 100ms | Query Monitor |

By implementing this comprehensive caching strategy, the Tonga National Portal will deliver optimal performance for citizens across all islands, regardless of their connectivity levels or device types. Regular monitoring and maintenance of these caching systems will ensure continued performance as the portal grows and evolves.

## Post-Deployment Verification

This section provides a comprehensive framework for verifying the successful deployment of the Tonga National Portal WordPress 6.5+ installation. Post-deployment verification is a critical phase that ensures the website functions correctly, performs optimally, and maintains security standards before being made available to the public.

### 1. System Health Verification

#### 1.1 Server Resource Monitoring

Verify that server resources are operating within expected parameters after deployment:

```bash
# Check CPU and memory usage
htop

# Monitor disk space usage
df -h

# Check for high I/O wait times
iostat -x 1 5

# Monitor system load
uptime

# Check for any system errors
sudo journalctl -p err -b
```

Expected results:
- CPU usage should remain under 70% during normal operation
- Memory usage should not exceed 80% of available RAM
- Disk space should have at least 20% free space
- Load average should be below the number of CPU cores

#### 1.2 Service Status Verification

Confirm all required services are running properly:

```bash
# Check Nginx status
sudo systemctl status nginx

# Verify PHP-FPM is running
sudo systemctl status php8.2-fpm

# Confirm MySQL is operational
sudo systemctl status mysql

# Check Redis status (if implemented)
sudo systemctl status redis-server

# Verify Fail2ban is active
sudo systemctl status fail2ban

# Check that automatic updates are enabled
sudo systemctl status unattended-upgrades
```

All services should show "active (running)" status with no errors.

#### 1.3 Log Analysis

Examine logs for any errors or warnings that might indicate problems:

```bash
# Check Nginx error logs
sudo tail -n 100 /var/log/nginx/error.log

# Review PHP error logs
sudo tail -n 100 /var/log/php8.2-fpm.log

# Examine MySQL logs
sudo tail -n 100 /var/log/mysql/error.log

# Check WordPress debug log (if enabled)
sudo tail -n 100 /var/www/tongaportal/wp-content/debug.log
```

Address any critical errors or recurring warnings before proceeding.

### 2. WordPress Core Verification

#### 2.1 WordPress Version and Updates

Verify WordPress core version and update status:

```bash
# Navigate to WordPress directory
cd /var/www/tongaportal

# Check WordPress version
sudo -u www-data wp core version

# Verify WordPress is up to date
sudo -u www-data wp core check-update

# Verify plugin and theme update status
sudo -u www-data wp plugin list --update=available
sudo -u www-data wp theme list --update=available
```

Ensure WordPress is running the latest version (6.5+) with all plugins and themes updated.

#### 2.2 WordPress Configuration Check

Verify critical WordPress configuration settings:

```bash
# Check WordPress configuration
cd /var/www/tongaportal
sudo -u www-data wp config list

# Verify important security settings
sudo -u www-data wp config get WP_DEBUG
sudo -u www-data wp config get DISALLOW_FILE_EDIT
sudo -u www-data wp config get WP_AUTO_UPDATE_CORE
```

Confirm these critical settings:
- `WP_DEBUG` should be set to `false` in production
- `DISALLOW_FILE_EDIT` should be set to `true`
- `WP_AUTO_UPDATE_CORE` should be set to `minor` or `true`

#### 2.3 Database Integrity Check

Verify database integrity and optimization:

```bash
# Check database tables
cd /var/www/tongaportal
sudo -u www-data wp db check

# Optimize database tables
sudo -u www-data wp db optimize

# Verify database prefix (should not be default 'wp_')
sudo -u www-data wp db prefix
```

All database tables should report "OK" status with no errors.

### 3. Comprehensive Functionality Testing

#### 3.1 Core WordPress Functionality

Systematically test all core WordPress functions:

| Function | Test Procedure | Expected Result |
|----------|---------------|-----------------|
| Admin Login | Navigate to `/wp-admin` and login with admin credentials | Successful login and redirect to dashboard |
| Content Creation | Create a test post and page | Content saves and displays correctly |
| Media Upload | Upload test images and documents | Media uploads successfully and displays in media library |
| Search Function | Perform searches with various keywords | Relevant results appear |
| User Management | Create a test user account | Account created with proper role and permissions |

Document any issues encountered during testing for resolution.

#### 3.2 Custom Functionality Testing

Test all custom features specific to the Tonga National Portal:

1. **Government Ministries and Services Directory**
   - Verify all ministries and services listings are accurate and accessible
   - Confirm contact information is correct

2. **Document Repository**
   - Test document uploads and downloads
   - Verify document categorization and search

3. **News and Announcements**
   - Test creation and publication of news items
   - Verify featured news appears in designated areas
   - Confirm news archive functionality

4. **MxChat AI Chatbox**
   - Verify chatbox loads and functions correctly
   - Test with common citizen queries
   - Confirm appropriate responses are provided


### 4. Security Verification

#### 4.1 WordPress Security Scan

Conduct a comprehensive security scan using WPScan:

```bash
# Install WPScan
sudo apt install -y ruby-full
sudo gem install wpscan

# Run a thorough security scan
wpscan --url https://www.gov.to/ --api-token YOUR_API_TOKEN --enumerate p,t,u,cb,dbe

# Scan for vulnerable plugins and themes
wpscan --url https://www.gov.to/ --api-token YOUR_API_TOKEN --enumerate vp,vt
```

Address any vulnerabilities identified before proceeding to production.

#### 4.2 SSL/TLS Configuration Verification

Verify SSL/TLS configuration meets modern security standards:

```bash
# Test SSL configuration using OpenSSL
openssl s_client -connect www.gov.to:443 -tls1_3

# Check SSL certificate validity
echo | openssl s_client -servername www.gov.to -connect www.gov.to:443 2>/dev/null | openssl x509 -noout -dates
```

Use online tools for comprehensive SSL testing:
- [SSL Labs Server Test](https://www.ssllabs.com/ssltest/analyze.html?d=www.gov.to)
- [ImmuniWeb SSL Security Test](https://www.immuniweb.com/ssl/)

Target SSL configuration:
- TLS 1.2 and 1.3 support only (no TLS 1.0/1.1)
- Strong cipher suites with forward secrecy
- HSTS implementation
- Proper certificate chain
- Valid certificate (not expired or self-signed)
- Minimum A grade on SSL Labs

#### 4.3 File and Directory Permissions Audit

Verify correct file permissions are set:

```bash
# Check WordPress core file permissions
find /var/www/tongaportal -type f -exec stat -c "%a %n" {} \; | grep -v "644"

# Check WordPress directory permissions
find /var/www/tongaportal -type d -exec stat -c "%a %n" {} \; | grep -v "755"

# Verify wp-config.php permissions
stat -c "%a %n" /var/www/tongaportal/wp-config.php

# Check uploads directory permissions
stat -c "%a %n" /var/www/tongaportal/wp-content/uploads
```

Correct any permission issues:

```bash
# Set correct file permissions
sudo find /var/www/tongaportal -type f -exec chmod 644 {} \;

# Set correct directory permissions
sudo find /var/www/tongaportal -type d -exec chmod 755 {} \;

# Set restricted permissions for wp-config.php
sudo chmod 600 /var/www/tongaportal/wp-config.php

# Set proper permissions for uploads directory
sudo chmod 775 /var/www/tongaportal/wp-content/uploads
sudo chown -R www-data:www-data /var/www/tongaportal/wp-content/uploads
```

#### 4.4 Security Headers Implementation

Verify and implement proper security headers:

```bash
# Check current security headers
curl -I https://www.gov.to/

# Test security headers using online tools
curl -s -I https://securityheaders.com/?q=www.gov.to
```

Implement these recommended security headers in Nginx:

```nginx
# Security headers
add_header X-Content-Type-Options "nosniff" always;
add_header X-Frame-Options "SAMEORIGIN" always;
add_header X-XSS-Protection "1; mode=block" always;
add_header Referrer-Policy "strict-origin-when-cross-origin" always;
add_header Permissions-Policy "camera=(), microphone=(), geolocation=()" always;
add_header Content-Security-Policy "default-src 'self'; script-src 'self' 'unsafe-inline' 'unsafe-eval' https://www.google-analytics.com https://www.googletagmanager.com; img-src 'self' data: https:; style-src 'self' 'unsafe-inline' https://fonts.googleapis.com; font-src 'self' https://fonts.gstatic.com; frame-src 'self'; connect-src 'self'" always;
add_header Strict-Transport-Security "max-age=31536000; includeSubDomains; preload" always;
```

> **NOTE**: A bad implementation of the Content-Security-Policy can lead into broken website display. Make sure you reference all the scripts loaded by the different plugins of the installation and other scripts that are needed to render the website succesffully.

#### 4.5 Firewall and Security Plugin Verification

Verify firewall and security plugin configurations:

```bash
# Check Fail2ban status and jail configuration
sudo fail2ban-client status
sudo fail2ban-client status nginx-http-auth

# Verify UFW firewall rules
sudo ufw status verbose

# Test WordPress security plugin settings (if installed)
cd /var/www/tongaportal
sudo -u www-data wp plugin list | grep -i security
```

Ensure these security measures are properly configured:
- Fail2ban has active jails for SSH and WordPress login attempts
- UFW allows only necessary ports (80, 443, 22)
- WordPress security plugins are properly configured
- Login attempt limiting is enabled
- Two-factor authentication for admin accounts

#### 4.6 Database Security Audit

Verify database security configuration:

```bash
# Check MySQL user privileges
sudo mysql -u root -p -e "SELECT user, host FROM mysql.user;"

# Verify database connection security
sudo mysql -u root -p -e "SHOW VARIABLES LIKE '%ssl%';"

# Check for public database access
sudo netstat -tulpn | grep mysql
```

Ensure:
- MySQL users have minimal necessary privileges
- Remote database access is disabled or strictly limited
- Database credentials are strong and unique
- Database prefix is not the default "wp_"

### 5. Backup and Recovery Verification

#### 5.1 Backup System Verification

Test the backup system to ensure it's functioning correctly:

```bash
# Test manual backup creation
cd /var/www/tongaportal
sudo -u www-data wp db export test-backup.sql

# If using UpdraftPlus or similar plugin
sudo -u www-data wp updraftplus backup

# Verify backup files exist
ls -la /path/to/backup/directory
```

Verify:
- Backups are being created successfully
- Backup files are not publicly accessible
- Backup schedule is properly configured
- Backup retention policy is implemented
- Off-site backup copies are configured

#### 5.2 Recovery Testing

Perform a test recovery to verify backup integrity:

```bash
# Create a test environment
sudo mkdir -p /var/www/test-recovery
sudo cp -a /var/www/tongaportal/. /var/www/test-recovery/

# Create test database
sudo mysql -u root -p -e "CREATE DATABASE test_recovery;"
sudo mysql -u root -p -e "GRANT ALL PRIVILEGES ON test_recovery.* TO 'tongaportal'@'localhost';"

# Restore database to test environment
cd /var/www/test-recovery
sudo -u www-data wp config set DB_NAME test_recovery
sudo -u www-data wp db import /path/to/backup/test-backup.sql

# Test the recovery site
sudo nano /etc/nginx/sites-available/test-recovery
# Configure test site with different port or domain
```

Verify:
- Database restores successfully
- Files restore correctly
- WordPress functions properly in the test environment
- No errors appear in logs

#### 5.3 Rollback Procedure Documentation

Document the complete rollback procedure for future reference:

1. **Database Rollback Procedure**

   ```bash
   # Stop web server
   sudo systemctl stop nginx
   
   # Restore database from backup
   mysql -u tongaportal -p tongaportal < /path/to/backup/tongaportal_db_YYYYMMDD.sql
   
   # Restart web server
   sudo systemctl start nginx
   ```

2. **File System Rollback Procedure**

   ```bash
   # Stop web server
   sudo systemctl stop nginx
   
   # Backup current files (optional)
   sudo tar -czf /backup/tongaportal_pre_rollback_$(date +%Y%m%d).tar.gz /var/www/tongaportal
   
   # Restore files from backup
   sudo rm -rf /var/www/tongaportal/*
   sudo tar -xzf /backup/tongaportal_files_YYYYMMDD.tar.gz -C /var/www/tongaportal/
   sudo chown -R www-data:www-data /var/www/tongaportal
   
   # Restart web server
   sudo systemctl start nginx
   ```

3. **Complete System Rollback Procedure**

   ```bash
   # Stop all services
   sudo systemctl stop nginx php8.2-fpm mysql redis-server
   
   # Restore database
   sudo mysql -u root -p tongaportal < /backup/tongaportal_db_YYYYMMDD.sql
   
   # Restore files
   sudo rm -rf /var/www/tongaportal/*
   sudo tar -xzf /backup/tongaportal_files_YYYYMMDD.tar.gz -C /var/www/tongaportal/
   sudo chown -R www-data:www-data /var/www/tongaportal
   
   # Restart all services
   sudo systemctl start mysql php8.2-fpm redis-server nginx
   
   # Verify restoration
   curl -I https://www.gov.to/
   ```

## Conclusion

This deployment guide provides comprehensive instructions for deploying the Tonga National Portal. Following these guidelines will ensure a secure and performant platform. Regular maintenance and monitoring are essential to maintain optimal performance and security.