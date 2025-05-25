# Deployment

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

**Performance Considerations for Tonga:**

- **Geographic Distribution**: With 36 inhabited islands, network latency may vary across locations
- **Internet Penetration**: Consider varying levels of connectivity across islands
- **Usage Patterns**: Expect higher traffic during business hours and after government announcements
- **Seasonal Factors**: Plan for increased usage during national events or information campaigns

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

```bash
# Update system packages
sudo apt update
sudo apt upgrade -y

# Install required packages
sudo apt install -y software-properties-common
sudo add-apt-repository ppa:ondrej/php
sudo apt update
sudo apt install -y nginx mysql-server php8.1-fpm php8.1-mysql php8.1-curl php8.1-gd php8.1-mbstring php8.1-xml php8.1-zip php8.1-bcmath php8.1-intl php8.1-soap

# Configure firewall
sudo ufw allow 'Nginx Full'
sudo ufw allow ssh
sudo ufw enable
```

#### Server Hardening Recommendations

- Disable root SSH access
- Use SSH key authentication instead of password
- Configure automatic security updates
- Install and configure fail2ban to prevent brute force attacks

### 2. Database Setup

```bash
# Secure MySQL installation
sudo mysql_secure_installation

# Create database and user
sudo mysql -u root -p
```

Once logged into MySQL, execute the following SQL commands:

```sql
CREATE DATABASE tongaportal CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'tongaportal'@'localhost' IDENTIFIED BY 'strong_password_here';
GRANT ALL PRIVILEGES ON tongaportal.* TO 'tongaportal'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

### 3. WordPress Installation

```bash
# Create web directory
sudo mkdir -p /var/www/tongaportal

# Download WordPress
cd /tmp
wget https://wordpress.org/latest.tar.gz
tar -xzf latest.tar.gz
sudo cp -a /tmp/wordpress/. /var/www/tongaportal/

# Set permissions
sudo chown -R www-data:www-data /var/www/tongaportal
sudo chmod -R 755 /var/www/tongaportal
```

#### WordPress Configuration

Create and configure the wp-config.php file:

```bash
cd /var/www/tongaportal
sudo cp wp-config-sample.php wp-config.php
sudo nano wp-config.php
```

Update the following values in the wp-config.php file:

```php
// Database settings
define('DB_NAME', 'tongaportal');
define('DB_USER', 'tongaportal');
define('DB_PASSWORD', 'strong_password_here');
define('DB_HOST', 'localhost');

// Security keys (generate from https://api.wordpress.org/secret-key/1.1/salt/)
define('AUTH_KEY',         'unique_value_here');
define('SECURE_AUTH_KEY',  'unique_value_here');
define('LOGGED_IN_KEY',    'unique_value_here');
define('NONCE_KEY',        'unique_value_here');
define('AUTH_SALT',        'unique_value_here');
define('SECURE_AUTH_SALT', 'unique_value_here');
define('LOGGED_IN_SALT',   'unique_value_here');
define('NONCE_SALT',       'unique_value_here');

// Performance optimizations
define('WP_CACHE', true);
define('WP_MEMORY_LIMIT', '256M');

// Debug settings (disable in production)
define('WP_DEBUG', false);
```

### 4. Web Server Configuration

#### Nginx Configuration

Create a new Nginx server block:

```bash
sudo nano /etc/nginx/sites-available/tongaportal
```

Add the following configuration:

```nginx
server {
    listen 80;
    server_name tongaportal.gov.to www.tongaportal.gov.to;
    root /var/www/tongaportal;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ /index.php?$args;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.ht {
        deny all;
    }

    location = /favicon.ico {
        log_not_found off;
        access_log off;
    }

    location = /robots.txt {
        log_not_found off;
        access_log off;
        allow all;
    }

    location ~* \.(css|gif|ico|jpeg|jpg|js|png)$ {
        expires max;
        log_not_found off;
    }
}
```

Enable the site and restart Nginx:

```bash
sudo ln -s /etc/nginx/sites-available/tongaportal /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

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

### 6. Plugin Installation and Configuration

After completing the WordPress installation through the web interface, install and configure the required plugins:

#### Essential Plugins

1. **WP Super Cache** - For performance optimization
2. **Wordfence Security** - For enhanced security
3. **UpdraftPlus** - For regular backups
4. **Yoast SEO** - For SEO optimization
5. **Contact Form 7** - For contact forms
6. **Custom Post Type UI** - For custom content types
7. **Advanced Custom Fields** - For custom fields

```bash
# Install WP-CLI for easier plugin management
cd /tmp
curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar
chmod +x wp-cli.phar
sudo mv wp-cli.phar /usr/local/bin/wp

# Install and activate plugins
cd /var/www/tongaportal
sudo -u www-data wp plugin install wp-super-cache wordfence updraftplus wordpress-seo contact-form-7 custom-post-type-ui advanced-custom-fields --activate
```

### 7. Theme Installation and Configuration

```bash
# Install and activate the Tonga National Portal theme
cd /var/www/tongaportal
sudo -u www-data wp theme install [theme-zip-file-url] --activate

# Import theme settings (if available)
sudo -u www-data wp theme mods import [theme-settings-file]
```

### 8. Content Migration

If migrating from an existing site:

```bash
# Export content from existing site
wp export --path=/path/to/existing/wordpress

# Import content to new site
cd /var/www/tongaportal
sudo -u www-data wp import /path/to/export/file.xml --authors=create
```

For large migrations, consider using a database-level migration:

```bash
# On source server
mysqldump -u root -p --opt [source_db_name] > tongaportal_backup.sql

# Transfer file to new server
scp tongaportal_backup.sql user@new_server:/tmp/

# On destination server
mysql -u root -p tongaportal < /tmp/tongaportal_backup.sql

# Update URLs in database
cd /var/www/tongaportal
sudo -u www-data wp search-replace 'old-domain.com' 'tongaportal.gov.to' --all-tables
```

### 9. Domain and DNS Configuration

1. Configure DNS records with your domain registrar:
   - A record: `tongaportal.gov.to` → [Server IP]
   - A record: `www.tongaportal.gov.to` → [Server IP]
   - MX records for email functionality

2. Wait for DNS propagation (can take up to 48 hours)

3. Verify DNS configuration:
   ```bash
   dig tongaportal.gov.to
   dig www.tongaportal.gov.to
   ```

## Configuration for Scalability

### Load Balancing

For high-traffic scenarios, implement load balancing using Nginx or HAProxy:

#### Nginx Load Balancer Configuration

```nginx
http {
    upstream tongaportal {
        server web1.tongaportal.gov.to;
        server web2.tongaportal.gov.to;
        server web3.tongaportal.gov.to;
    }

    server {
        listen 80;
        server_name tongaportal.gov.to www.tongaportal.gov.to;

        location / {
            proxy_pass http://tongaportal;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
        }
    }
}
```

### Caching Strategies

#### Object Caching with Redis

```bash
# Install Redis
sudo apt install -y redis-server php8.1-redis

# Configure Redis
sudo nano /etc/redis/redis.conf
```

Update the Redis configuration:
```
maxmemory 512mb
maxmemory-policy allkeys-lru
```

Restart Redis:
```bash
sudo systemctl restart redis
```

Install and configure the Redis Object Cache plugin:
```bash
cd /var/www/tongaportal
sudo -u www-data wp plugin install redis-cache --activate
sudo -u www-data wp redis enable
```

#### Page Caching

Configure WP Super Cache with the following settings:

1. Enable caching
2. Use mod_rewrite to serve cache files
3. Set cache timeout to 3600 seconds (1 hour)
4. Enable compression
5. Exclude admin pages and logged-in users

### Database Optimization

1. **MySQL Configuration**

   Edit the MySQL configuration file:
   ```bash
   sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
   ```

   Add or modify the following settings:
   ```
   # InnoDB settings
   innodb_buffer_pool_size = 4G
   innodb_log_file_size = 512M
   innodb_flush_method = O_DIRECT
   innodb_flush_log_at_trx_commit = 2

   # Query cache settings
   query_cache_type = 1
   query_cache_size = 128M
   query_cache_limit = 2M

   # Connection settings
   max_connections = 500
   ```

   Restart MySQL:
   ```bash
   sudo systemctl restart mysql
   ```

2. **Database Maintenance**

   Schedule regular database optimization:
   ```bash
   cd /var/www/tongaportal
   sudo -u www-data wp db optimize
   ```

   Create a cron job for weekly optimization:
   ```bash
   echo "0 2 * * 0 www-data cd /var/www/tongaportal && wp db optimize" | sudo tee -a /etc/crontab
   ```

### Content Delivery Network (CDN) Setup

1. **Configure Cloudflare CDN**

   - Create a Cloudflare account
   - Add the domain tongaportal.gov.to
   - Update nameservers with your domain registrar
   - Enable Cloudflare CDN features:
     - Auto Minify (HTML, CSS, JS)
     - Brotli compression
     - Rocket Loader
     - Cache Level: Standard

2. **WordPress CDN Integration**

   Install and configure the Cloudflare plugin:
   ```bash
   cd /var/www/tongaportal
   sudo -u www-data wp plugin install cloudflare --activate
   ```

## Post-Deployment Verification

### Testing Procedures

1. **Functionality Testing**

   - Verify all pages load correctly
   - Test user registration and login
   - Test form submissions
   - Verify admin functionality
   - Test search functionality
   - Verify media uploads

2. **Cross-Browser Testing**

   Test the site in multiple browsers:
   - Chrome
   - Firefox
   - Safari
   - Edge
   - Mobile browsers (iOS Safari, Android Chrome)

3. **Responsive Design Testing**

   Verify the site displays correctly on:
   - Desktop (1920×1080, 1366×768)
   - Tablet (iPad 768×1024)
   - Mobile (iPhone 375×667, Android 360×640)

### Performance Benchmarking

1. **Page Speed Testing**

   ```bash
   # Install Apache Benchmark
   sudo apt install -y apache2-utils

   # Test homepage response time
   ab -n 100 -c 10 https://tongaportal.gov.to/
   ```

2. **Online Performance Testing Tools**

   - Google PageSpeed Insights
   - GTmetrix
   - WebPageTest

   Document baseline performance metrics:
   - Time to First Byte (TTFB)
   - First Contentful Paint (FCP)
   - Largest Contentful Paint (LCP)
   - Total Page Size
   - Number of Requests

3. **Load Testing**

   ```bash
   # Install k6 load testing tool
   sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys C5AD17C747E3415A3642D57D77C6C491D6AC1D69
   echo "deb https://dl.k6.io/deb stable main" | sudo tee /etc/apt/sources.list.d/k6.list
   sudo apt update
   sudo apt install k6

   # Create a load test script
   cat > loadtest.js << EOF
   import http from 'k6/http';
   import { sleep } from 'k6';

   export default function() {
     http.get('https://tongaportal.gov.to/');
     sleep(1);
   }
   EOF

   # Run load test with 50 virtual users for 1 minute
   k6 run --vus 50 --duration 1m loadtest.js
   ```

### Security Verification

1. **WordPress Security Scan**

   ```bash
   # Install WPScan
   sudo apt install -y ruby-full
   sudo gem install wpscan

   # Run security scan
   wpscan --url https://tongaportal.gov.to/ --api-token YOUR_API_TOKEN
   ```

2. **SSL Configuration Check**

   ```bash
   # Test SSL configuration
   curl -s https://www.ssllabs.com/ssltest/analyze.html?d=tongaportal.gov.to
   ```

3. **File Permissions Check**

   ```bash
   # Verify correct file permissions
   find /var/www/tongaportal -type f -exec chmod 644 {} \;
   find /var/www/tongaportal -type d -exec chmod 755 {} \;
   chmod 600 /var/www/tongaportal/wp-config.php
   ```

4. **Security Headers Check**

   ```bash
   # Check security headers
   curl -I https://tongaportal.gov.to/
   ```

   Implement recommended security headers in Nginx:

   ```nginx
   add_header X-Content-Type-Options "nosniff" always;
   add_header X-Frame-Options "SAMEORIGIN" always;
   add_header X-XSS-Protection "1; mode=block" always;
   add_header Referrer-Policy "strict-origin-when-cross-origin" always;
   add_header Content-Security-Policy "default-src 'self'; script-src 'self' 'unsafe-inline' 'unsafe-eval' https://www.google-analytics.com; img-src 'self' data: https:; style-src 'self' 'unsafe-inline'; font-src 'self'; frame-src 'self'; connect-src 'self';" always;
   ```

## Rollback Procedures

### Database Rollback

1. **Regular Backups**

   Configure UpdraftPlus for automated backups:
   - Daily database backups
   - Weekly full site backups
   - Retain backups for 30 days

2. **Manual Database Backup Before Major Changes**

   ```bash
   # Create a database backup
   cd /var/www/tongaportal
   sudo -u www-data wp db export pre_change_backup.sql
   ```

3. **Database Restoration Process**

   ```bash
   # Restore from backup
   cd /var/www/tongaportal
   sudo -u www-data wp db import backup_file.sql
   ```

### File System Rollback

1. **Version Control**

   Maintain a Git repository for theme and custom plugin code:

   ```bash
   # Initialize Git repository
   cd /var/www/tongaportal/wp-content/themes/tongaportal-theme
   git init
   git add .
   git commit -m "Initial commit"

   # Create a tag before major changes
   git tag v1.0.0
   ```

2. **File System Backup**

   ```bash
   # Create a full site backup
   sudo tar -czf /backup/tongaportal_$(date +%Y%m%d).tar.gz /var/www/tongaportal
   ```

3. **File Restoration Process**

   ```bash
   # Restore from backup
   sudo rm -rf /var/www/tongaportal/*
   sudo tar -xzf /backup/tongaportal_YYYYMMDD.tar.gz -C /
   sudo chown -R www-data:www-data /var/www/tongaportal
   ```

### Complete Rollback Procedure

In case of critical failure requiring a full rollback:

1. **Stop web server**
   ```bash
   sudo systemctl stop nginx
   ```

2. **Restore database**
   ```bash
   mysql -u root -p tongaportal < /backup/tongaportal_db_YYYYMMDD.sql
   ```

3. **Restore files**
   ```bash
   sudo rm -rf /var/www/tongaportal/*
   sudo tar -xzf /backup/tongaportal_files_YYYYMMDD.tar.gz -C /var/www/tongaportal/
   sudo chown -R www-data:www-data /var/www/tongaportal
   ```

4. **Restart web server**
   ```bash
   sudo systemctl start nginx
   ```

5. **Verify restoration**
   - Check website functionality
   - Verify admin access
   - Check recent content

### Emergency Contacts

Maintain a list of emergency contacts for critical issues:

| Role | Name | Contact |
|------|------|---------|
| System Administrator | [Name] | [Phone/Email] |
| Database Administrator | [Name] | [Phone/Email] |
| WordPress Developer | [Name] | [Phone/Email] |
| Hosting Provider Support | [Provider] | [Support Contact] |

## Conclusion

This deployment guide provides comprehensive instructions for deploying and maintaining the Tonga National Portal. Following these guidelines will ensure a secure, performant, and scalable platform. Regular maintenance and monitoring are essential to maintain optimal performance and security.

For additional support or questions regarding deployment, please contact the technical support team at [support@tongaportal.gov.to].