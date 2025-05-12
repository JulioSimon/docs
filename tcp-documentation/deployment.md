# Deployment

This section provides comprehensive guidance on deploying the Government Administration Portal to production environments for Tonga's government services, including server requirements, capacity planning, step-by-step deployment processes, environment setup, and performance optimization. The recommended configuration supports hundreds of concurrent users on a single server, which is more than sufficient for Tonga's population of approximately 110,000 citizens, eliminating the need for complex scaling strategies.

## Server Requirements

The Government Administration Portal requires specific server configurations to ensure optimal performance, security, and reliability.

### Hardware Requirements

| Component | Requirement |
|-----------|-------------|
| CPU | Minimum 4 cores |
| RAM | Minimum 16GB |
| Storage | Minimum 100GB SSD |
| Network | High-speed internet connection with 1Gbps bandwidth |

While the recommended specifications are more than sufficient for Tonga's government needs, larger government entities or unusually high-traffic scenarios might consider scaling up these specifications. For Tonga's population of approximately 110,000 citizens, the requirements listed above provide optimal performance of the Government Administration Portal with substantial capacity to spare.

### Capacity Planning

The recommended hardware specifications are designed to handle the needs of Tonga's citizens without requiring horizontal scaling or complex infrastructure. With a population of approximately 110,000 people across 36 inhabited islands, the Government Administration Portal is well-equipped to serve the entire nation from a single server deployment.

| Metric | Estimated Capacity | Tonga Context |
|--------|-------------------|---------------|
| Concurrent active users | 500-800 | Exceeds peak demand for a population of 110,000 |
| Total registered users | 50,000+ | Sufficient for nearly half of Tonga's population |
| Database size | Up to 50GB | Ample for all citizen records and transactions |
| File storage | Up to 80GB | Adequate for document storage needs |

With the minimum recommended configuration (4 cores, 16GB RAM, 100GB SSD), a single server deployment can comfortably support:

- Up to 800 concurrent users actively using the system (submitting forms, processing requests)
- Several thousand concurrent passive sessions (logged in but not actively performing operations)
- Daily peaks of up to 10,000 unique visitors (approximately 9% of Tonga's population)
- Processing of 100+ background jobs per minute

This capacity significantly exceeds the expected requirements for Tonga's government services, even during peak usage periods. The system is designed to be efficient with resources, leveraging:

- Effective database query caching
- Optimized PHP-FPM worker processes
- Redis for session and cache storage
- Efficient background job processing with Supervisor

> **Note:** These estimates assume typical usage patterns for Tonga's government administrative portal, including form submissions, document processing, and reporting. Given Tonga's population size and internet usage patterns, the recommended hardware specifications provide substantial headroom for future growth and peak usage scenarios, such as during tax filing periods or national registration events.

### Software Requirements

| Software | Version | Notes |
|----------|---------|-------|
| PHP | 8.3+ | With required extensions |
| MariaDB | 10.5+ | Recommended database engine |
| MySQL | 8.0+ | Alternative database engine |
| Nginx | Latest stable | Preferred web server |
| Redis | 6.0+ | For caching and queues |
| Node.js | 20.0+ | For asset compilation with Vite |
| Composer | 2.0+ | PHP dependency manager |
| Git | Latest stable | Version control |
| Laravel | 11.x | Framework version |
| FilamentPHP | 3.x | Admin panel framework |
| Livewire | 3.x | Interactive components |

#### Required PHP Extensions

- BCMath
- Ctype
- Fileinfo
- JSON
- Mbstring
- OpenSSL
- PDO
- Tokenizer
- XML
- DOM
- GD or Imagick
- Zip
- Curl
- Intl (for internationalization)
- OPcache (for performance)
- SQLite (for testing)
- PCRE (for regular expressions)

> **Note:** The application uses Spatie Media Library for file management, which requires GD or Imagick PHP extension for image manipulation.

### Operating System

The platform must be deployed on:

- **Linux**:
  - Ubuntu 24.04 LTS

Ubuntu 24.04 LTS is the target deployment operating system for this application due to its long-term support, stability, security features, and optimal compatibility with the required software stack.

## Step-by-Step Deployment Process

This section outlines the complete process for deploying the Government Administration Portal to a production environment.

### 1. Server Preparation for Ubuntu 24.04 LTS

The following steps are specific to Ubuntu 24.04 LTS, which is the target deployment operating system for the Government Administration Portal.

1. **Update System Packages**:
   ```bash
   # For Ubuntu 24.04 LTS
   sudo apt update
   sudo apt upgrade -y
   ```

2. **Install Required Dependencies**:
   ```bash
   # For Ubuntu 24.04 LTS
   sudo apt install -y nginx mariadb-server redis-server git unzip curl
   ```

3. **Install PHP and Required Extensions**:
   ```bash
   # For Ubuntu 24.04 LTS
   sudo apt install -y php8.3-fpm php8.3-cli php8.3-common php8.3-mysql \
   php8.3-zip php8.3-gd php8.3-mbstring php8.3-curl php8.3-xml php8.3-bcmath \
   php8.3-intl php8.3-opcache php8.3-sqlite3 php8.3-tokenizer php8.3-fileinfo
   ```

4. **Install Composer**:
   ```bash
   curl -sS https://getcomposer.org/installer | php
   sudo mv composer.phar /usr/local/bin/composer
   ```

5. **Install Node.js and npm**:
   ```bash
   # Using NVM (recommended)
   curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
   source ~/.bashrc
   nvm install 20
   nvm use 20
   ```

6. **Configure MariaDB**:
   ```bash
   sudo mariadb-secure-installation
   ```
   
   Create a database and user:
   ```sql
   CREATE DATABASE tongacp;
   CREATE USER 'tongacp_user'@'localhost' IDENTIFIED BY 'secure_password';
   GRANT ALL PRIVILEGES ON tongacp.* TO 'tongacp_user'@'localhost';
   FLUSH PRIVILEGES;
   ```

7. **Configure Nginx**:
   Create a new site configuration:
   ```bash
   sudo nano /etc/nginx/sites-available/tongacp
   ```
   
   Add the following configuration:
   ```nginx
   server {
       listen 80;
       server_name your-domain.com;
       root /var/www/tongacp/public;
   
       add_header X-Frame-Options "SAMEORIGIN";
       add_header X-Content-Type-Options "nosniff";
   
       index index.php;
   
       charset utf-8;
   
       location / {
           try_files $uri $uri/ /index.php?$query_string;
       }
   
       location = /favicon.ico { access_log off; log_not_found off; }
       location = /robots.txt  { access_log off; log_not_found off; }
   
       error_page 404 /index.php;
   
       location ~ \.php$ {
           fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
           fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
           include fastcgi_params;
           fastcgi_read_timeout 300;
       }
   
       location ~ /\.(?!well-known).* {
           deny all;
       }
   }
   ```
   
   Enable the site:
   ```bash
   sudo ln -s /etc/nginx/sites-available/tongacp /etc/nginx/sites-enabled/
   sudo nginx -t
   sudo systemctl restart nginx
   ```

8. **Set Up SSL with Let's Encrypt**:
   ```bash
   sudo apt install -y certbot python3-certbot-nginx
   sudo certbot --nginx -d your-domain.com
   ```

### 2. Application Deployment

1. **Clone the Repository**:
   ```bash
   cd /var/www
   git clone https://github.com/your-organization/tongacp.git
   cd tongacp
   ```

2. **Install PHP Dependencies**:
   ```bash
   composer install --no-dev --optimize-autoloader
   ```

3. **Install Node.js Dependencies and Build Assets**:
   ```bash
   npm ci
   npm run build
   ```

4. **Set Proper Permissions**:
   ```bash
   sudo chown -R www-data:www-data /var/www/tongacp
   sudo chmod -R 755 /var/www/tongacp
   sudo chmod -R 775 /var/www/tongacp/storage /var/www/tongacp/bootstrap/cache
   ```

5. **Set Up Environment Configuration**:
   ```bash
   cp .env.example .env
   php artisan key:generate
   ```
   
   Edit the `.env` file with your production settings:
   ```bash
   nano .env
   ```
   
   Update the following values:
   ```
   APP_ENV=production
   APP_DEBUG=false
   APP_URL=https://your-domain.com
   
   DB_CONNECTION=mysql
   DB_HOST=127.0.0.1
   DB_PORT=3306
   DB_DATABASE=tongacp
   DB_USERNAME=tongacp_user
   DB_PASSWORD=secure_password
   
   CACHE_DRIVER=redis
   SESSION_DRIVER=redis
   QUEUE_CONNECTION=redis
   
   REDIS_HOST=127.0.0.1
   REDIS_PASSWORD=null
   REDIS_PORT=6379
   
   MAIL_MAILER=smtp
   MAIL_HOST=your-mail-server.com
   MAIL_PORT=587
   MAIL_USERNAME=your-username
   MAIL_PASSWORD=your-password
   MAIL_ENCRYPTION=tls
   MAIL_FROM_ADDRESS=no-reply@your-domain.com
   MAIL_FROM_NAME="${APP_NAME}"
   ```

6. **Run Database Migrations and Seeders**:
   ```bash
   php artisan migrate --force
   php artisan db:seed --force
   ```

7. **Generate Application Cache**:
   ```bash
   php artisan config:cache
   php artisan route:cache
   php artisan view:cache
   ```

8. **Set Up Symbolic Link for Storage**:
   ```bash
   php artisan storage:link
   ```

9. **Set Up Cron Job for Scheduled Tasks**:
   ```bash
   sudo crontab -e
   ```
   
   Add the following line:
   ```
   * * * * * cd /var/www/tongacp && php artisan schedule:run >> /dev/null 2>&1
   ```

10. **Install and Configure Supervisor for Queue Workers**:
     ```bash
     sudo apt install -y supervisor
     ```
     
     Create a configuration file for the TongaCP queue workers:
     ```bash
     sudo nano /etc/supervisor/conf.d/tongacp-worker.conf
     ```
     
     Add the following configuration:
     ```ini
     [program:tongacp-worker]
     process_name=%(program_name)s_%(process_num)02d
     command=php /var/www/tongacp/artisan queue:work redis --sleep=3 --tries=3 --max-time=3600
     autostart=true
     autorestart=true
     stopasgroup=true
     killasgroup=true
     user=www-data
     numprocs=8
     redirect_stderr=true
     stdout_logfile=/var/www/tongacp/storage/logs/worker.log
     stopwaitsecs=3600
     ```
     
     Reload the supervisor configuration and start the workers:
     ```bash
     sudo supervisorctl reread
     sudo supervisorctl update
     sudo supervisorctl start "tongacp-worker:*"
     ```
     
     > **Note:** Supervisor is the recommended process monitor for Laravel queue workers. It will automatically restart the workers if they fail and provides better process management than systemd for this specific use case.

### 3. Post-Deployment Verification

1. **Check Application Status**:
   ```bash
   php artisan about
   ```

2. **Verify Web Server Configuration**:
   ```bash
   sudo nginx -t
   ```

3. **Test Application Functionality**:
   - Navigate to your domain in a web browser
   - Verify that you can access the login page
   - Test login functionality with admin credentials
   - Check that all modules are functioning correctly

4. **Monitor Logs for Errors**:
   ```bash
   tail -f /var/www/tongacp/storage/logs/laravel.log
   ```

## Configuration for Scaling

> **Note:** With the recommended hardware specifications (4+ cores, 16GB+ RAM, 100GB+ SSD), a single server deployment can handle 500-800 concurrent active users and up to 50,000+ registered users. For Tonga's population of approximately 110,000 citizens, this capacity is more than sufficient, and the government will not need to implement the scaling strategies outlined below. These are provided for reference only in case of exceptional requirements or significant future growth beyond the capacity of a single server.

The following scaling strategies are only necessary if your usage significantly exceeds the capacity outlined in the [Capacity Planning](#capacity-planning) section. For Tonga's government services, this scenario is highly unlikely given the population size of approximately 110,000 citizens. These strategies are included primarily for documentation completeness.

### Horizontal Scaling

Horizontal scaling involves adding more servers to distribute the load:

1. **Load Balancer Setup**:
   - Deploy multiple application servers
   - Configure a load balancer (e.g., Nginx, HAProxy, AWS ELB)
   - Ensure session persistence (sticky sessions or shared sessions)

   Example Nginx load balancer configuration:
   ```nginx
   upstream tongacp {
       server app1.example.com;
       server app2.example.com;
       server app3.example.com;
   }
   
   server {
       listen 80;
       server_name your-domain.com;
       
       location / {
           proxy_pass http://tongacp;
           proxy_set_header Host $host;
           proxy_set_header X-Real-IP $remote_addr;
           proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
           proxy_set_header X-Forwarded-Proto $scheme;
       }
   }
   ```

2. **Session Management**:
   - Configure Redis for session storage
   - Update `.env` file:
     ```
     SESSION_DRIVER=redis
     ```

3. **Shared File Storage**:
   - Use a distributed file system or cloud storage (S3, GCS)
   - Update filesystem configuration in `config/filesystems.php`
   - For cloud storage, install and configure the appropriate package:
     ```bash
     composer require league/flysystem-aws-s3-v3
     ```

### Vertical Scaling

Vertical scaling involves increasing the resources of existing servers:

1. **Database Optimization**:
   - Increase server resources (CPU, RAM)
   - Optimize MariaDB configuration for larger workloads (unlikely to be needed for Tonga's scale)
   - Consider database replication for read-heavy workloads (unnecessary for Tonga's population size)

   Example MariaDB optimization settings in `50-server.cnf`:
   ```
   [mariadb]
   innodb_buffer_pool_size = 8G
   innodb_log_file_size = 1G
   innodb_flush_method = O_DIRECT
   innodb_flush_log_at_trx_commit = 2
   max_connections = 1000
   innodb_read_io_threads = 8
   innodb_write_io_threads = 8
   ```

   > **Note:** These settings are optimized for the minimum hardware requirements (16GB RAM, 4 cores). Adjust values proportionally if using higher-spec hardware.

2. **Web Server Tuning**:
   - Increase worker processes and connections
   - Optimize PHP-FPM configuration

   Example PHP-FPM configuration for 4+ cores and 16GB+ RAM:
   ```
   pm = dynamic
   pm.max_children = 100
   pm.start_servers = 20
   pm.min_spare_servers = 10
   pm.max_spare_servers = 30
   pm.max_requests = 1000
   pm.process_idle_timeout = 10s
   request_terminate_timeout = 300
   ```

   > **Note:** These settings are optimized for the minimum hardware requirements, which are more than sufficient for Tonga's government portal. Even during peak usage periods across all of Tonga's islands, these settings provide excellent performance without requiring further tuning.

3. **Application Server Scaling**:
   - Increase server resources (CPU, RAM)
   - Optimize PHP memory limits
   - Consider using OpCache for improved performance

### Database Scaling

As data volume grows, database scaling becomes essential:

1. **Read Replicas**:
   - Set up MariaDB read replicas
   - Configure Laravel to use different connections for reads and writes

   Example database configuration in `config/database.php`:
   ```php
   'mysql' => [
       'read' => [
           'host' => [
               'replica1.example.com',
               'replica2.example.com',
           ],
       ],
       'write' => [
           'host' => 'master.example.com',
       ],
       // Other configuration...
   ],
   ```

2. **Database Sharding**:
   - For very large deployments, consider database sharding (not applicable for Tonga's government portal with 110,000 citizens)
   - Implement tenant-based sharding using the entity ID (unnecessary given Tonga's scale)

3. **Query Optimization**:
   - Regularly analyze and optimize slow queries
   - Ensure proper indexing of frequently queried columns
   - Consider using query caching for repetitive queries

### Queue System Scaling

For handling background processes efficiently:

1. **Multiple Queue Workers with Supervisor**:
   - Configure supervisor to run multiple queue worker processes
   - Distribute workers across servers for high availability

   Example enhanced supervisor configuration:
   ```ini
   [program:tongacp-workers]
   process_name=%(program_name)s_%(process_num)02d
   command=php /var/www/tongacp/artisan queue:work redis --sleep=3 --tries=3 --max-time=3600
   autostart=true
   autorestart=true
   stopasgroup=true
   killasgroup=true
   user=www-data
   numprocs=16
   redirect_stderr=true
   stdout_logfile=/var/www/tongacp/storage/logs/worker.log
   stopwaitsecs=3600
   
   ; Supervisor configuration to increase file descriptor limits
   [supervisord]
   minfds=10000
   ```
   
   > **Note:** For Tonga's government portal with its population of approximately 110,000 citizens, the default `numprocs` value is more than sufficient. The configuration shown here already provides substantial capacity beyond what would be required for the entire nation's usage.

2. **Queue Prioritization with Supervisor**:
   - Set up different queues for different types of jobs
   - Prioritize critical jobs by processing them first
   - Configure separate supervisor processes for different queue priorities

   Example job class configuration:
   ```php
   // In job classes
   public function queue()
   {
       return 'high';
   }
   ```
   
   Example supervisor configuration for prioritized queues:
   ```ini
   [program:tongacp-high-priority]
   process_name=%(program_name)s_%(process_num)02d
   command=php /var/www/tongacp/artisan queue:work redis --queue=high --sleep=3 --tries=3 --max-time=3600
   autostart=true
   autorestart=true
   user=www-data
   numprocs=4
   redirect_stderr=true
   stdout_logfile=/var/www/tongacp/storage/logs/worker-high.log
   
   [program:tongacp-default-priority]
   process_name=%(program_name)s_%(process_num)02d
   command=php /var/www/tongacp/artisan queue:work redis --queue=default --sleep=3 --tries=3 --max-time=3600
   autostart=true
   autorestart=true
   user=www-data
   numprocs=8
   redirect_stderr=true
   stdout_logfile=/var/www/tongacp/storage/logs/worker-default.log
   
   [program:tongacp-low-priority]
   process_name=%(program_name)s_%(process_num)02d
   command=php /var/www/tongacp/artisan queue:work redis --queue=low --sleep=3 --tries=3 --max-time=3600
   autostart=true
   autorestart=true
   user=www-data
   numprocs=2
   redirect_stderr=true
   stdout_logfile=/var/www/tongacp/storage/logs/worker-low.log
   ```
   
   > **Note:** Allocate more worker processes to queues that handle critical or frequently executed jobs. This approach provides better control over resource allocation than using a single worker process with the `--queue=high,default,low` parameter.

## Environment Setup

Proper environment configuration is crucial for security, performance, and functionality.

### Production Environment Variables

Key environment variables for production:

```
# Application Settings
APP_ENV=production
APP_DEBUG=false
APP_URL=https://your-domain.com

# Logging
LOG_CHANNEL=daily
LOG_LEVEL=warning

# Drivers
BROADCAST_DRIVER=redis
CACHE_DRIVER=redis
FILESYSTEM_DISK=local
QUEUE_CONNECTION=redis
SESSION_DRIVER=redis
SESSION_LIFETIME=120

# Redis
REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379

# Mail
MAIL_MAILER=smtp
MAIL_HOST=your-mail-server.com
MAIL_PORT=587
MAIL_USERNAME=your-username
MAIL_PASSWORD=your-password
MAIL_ENCRYPTION=tls
MAIL_FROM_ADDRESS=no-reply@your-domain.com
MAIL_FROM_NAME="${APP_NAME}"

# Session & Authentication
SANCTUM_STATEFUL_DOMAINS=your-domain.com
SESSION_DOMAIN=.your-domain.com

# Filament Settings
FILAMENT_PATH=admin
FILAMENT_PANEL_ID=admin
FILAMENT_LANGUAGE_SWITCH_ENABLED=true
FILAMENT_PANEL_SWITCH_ENABLED=true
FILAMENT_SHIELD_ENABLED=true
FILAMENT_SOCIALITE_ENABLED=true

# Keycloak Integration (if using)
KEYCLOAK_BASE_URL=https://your-keycloak-server.com
KEYCLOAK_REALM=your-realm
KEYCLOAK_CLIENT_ID=your-client-id
KEYCLOAK_CLIENT_SECRET=your-client-secret
KEYCLOAK_REDIRECT_URI="${APP_URL}/auth/callback/keycloak"
```

> **Note:** Laravel 11 and FilamentPHP v3 require specific environment variables for proper functionality. The above configuration includes settings for multi-panel support, language switching, role management, and social login capabilities based on the installed packages.

### Security Configurations

1. **Web Server Security Headers**:
   ```nginx
   add_header X-Frame-Options "SAMEORIGIN";
   add_header X-Content-Type-Options "nosniff";
   add_header X-XSS-Protection "1; mode=block";
   add_header Referrer-Policy "strict-origin-when-cross-origin";
   add_header Content-Security-Policy "default-src 'self'; script-src 'self' 'unsafe-inline' 'unsafe-eval'; style-src 'self' 'unsafe-inline'; img-src 'self' data:; font-src 'self' data:; connect-src 'self'";
   ```

2. **PHP Security Settings**:
   ```
   expose_php = Off
   display_errors = Off
   log_errors = On
   error_log = /var/log/php/error.log
   allow_url_fopen = Off
   session.cookie_secure = On
   session.cookie_httponly = On
   session.use_strict_mode = On
   ```

3. **Database Connection Security**:
   - Use TLS for database connections
   - Implement least privilege for database users
   - Regularly rotate database credentials

### Backup Configuration

Set up automated backups:

1. **Database Backups**:
   ```bash
   # Create backup script
   nano /usr/local/bin/backup-tongacp-db.sh
   ```
   
   Script content:
   ```bash
   #!/bin/bash
   TIMESTAMP=$(date +"%Y%m%d%H%M%S")
   BACKUP_DIR="/var/backups/tongacp/database"
   mkdir -p $BACKUP_DIR
   
   # Database backup
   mariadb-dump -u tongacp_user -p'secure_password' tongacp | gzip > $BACKUP_DIR/tongacp_db_$TIMESTAMP.sql.gz
   
   # Rotate backups (keep last 30 days)
   find $BACKUP_DIR -type f -name "tongacp_db_*.sql.gz" -mtime +30 -delete
   ```
   
   Make executable and schedule:
   ```bash
   chmod +x /usr/local/bin/backup-tongacp-db.sh
   sudo crontab -e
   ```
   
   Add to crontab:
   ```
   0 2 * * * /usr/local/bin/backup-tongacp-db.sh
   ```

2. **File Backups**:
   ```bash
   # Create backup script
   nano /usr/local/bin/backup-tongacp-files.sh
   ```
   
   Script content:
   ```bash
   #!/bin/bash
   TIMESTAMP=$(date +"%Y%m%d%H%M%S")
   BACKUP_DIR="/var/backups/tongacp/files"
   mkdir -p $BACKUP_DIR
   
   # Files backup (excluding vendor, node_modules, etc.)
   tar -czf $BACKUP_DIR/tongacp_files_$TIMESTAMP.tar.gz \
       --exclude="vendor" \
       --exclude="node_modules" \
       --exclude="storage/logs" \
       --exclude="storage/framework/cache" \
       -C /var/www tongacp
   
   # Rotate backups (keep last 7 days)
   find $BACKUP_DIR -type f -name "tongacp_files_*.tar.gz" -mtime +7 -delete
   ```
   
   Make executable and schedule:
   ```bash
   chmod +x /usr/local/bin/backup-tongacp-files.sh
   sudo crontab -e
   ```
   
   Add to crontab:
   ```
   0 3 * * * /usr/local/bin/backup-tongacp-files.sh
   ```

## Performance Optimization

Optimizing performance ensures a responsive and efficient platform for both Tonga's government staff and citizens, providing a smooth user experience even during peak usage periods across all 36 inhabited islands.

### Caching Strategies

1. **Application Caching**:
   ```bash
   php artisan config:cache
   php artisan route:cache
   php artisan view:cache
   ```

2. **Data Caching**:
   - Implement Redis caching for frequently accessed data
   - Use Laravel's cache facade for query results

   Example implementation:
   ```php
   $value = Cache::remember('users', 3600, function () {
       return DB::table('users')->get();
   });
   ```

3. **Static Asset Caching**:
   - Configure browser caching for static assets
   - Use versioning for CSS and JavaScript files

   Example Nginx configuration:
   ```nginx
   location ~* \.(css|js|jpg|jpeg|png|gif|ico|svg|woff|woff2|ttf|eot)$ {
       expires 30d;
       add_header Cache-Control "public, no-transform";
   }
   ```

### Content Delivery

1. **Content Delivery Network (CDN)**:
   - Use a CDN for static assets
   - Configure Vite to use the CDN URL

   Example configuration in `.env`:
   ```
   ASSET_URL=https://cdn.your-domain.com
   ```
   
   Example in `vite.config.js`:
   ```javascript
   import { defineConfig } from 'vite';
   import laravel from 'laravel-vite-plugin';

   export default defineConfig({
       plugins: [
           laravel({
               input: ['resources/css/app.css', 'resources/js/app.js'],
           }),
       ],
       // Vite will use ASSET_URL from .env automatically
   });
   ```

2. **Image Optimization**:
   - Implement image compression
   - Use responsive images with appropriate sizes
   - Consider lazy loading for images

   Example implementation with Laravel Intervention Image:
   ```php
   $image = Image::make($request->file('image'))
       ->resize(800, null, function ($constraint) {
           $constraint->aspectRatio();
           $constraint->upsize();
       })
       ->encode('jpg', 80);
   ```

### Database Optimization

1. **Query Optimization**:
   - Use eager loading to prevent N+1 query problems
   - Implement database indexing for frequently queried columns
   - Use query caching for repetitive queries

   Example eager loading:
   ```php
   $tickets = Ticket::with(['user', 'category', 'status'])->get();
   ```

2. **Database Configuration**:
   - Optimize MariaDB settings for your server resources
   - Implement query caching
   - Regular maintenance (ANALYZE TABLE, OPTIMIZE TABLE)

### Code Optimization

1. **Composer Optimization**:
   ```bash
   composer install --optimize-autoloader --no-dev
   ```

2. **PHP OpCache**:
   Enable and configure OpCache in `php.ini`:
   ```
   opcache.enable=1
   opcache.memory_consumption=128
   opcache.interned_strings_buffer=8
   opcache.max_accelerated_files=4000
   opcache.revalidate_freq=60
   opcache.fast_shutdown=1
   opcache.enable_cli=1
   ```

3. **JavaScript and CSS Minification**:
   Ensure production builds are minified:
   ```bash
   npm run build
   ```

### Monitoring and Profiling

1. **Performance Monitoring**:
   - Implement server monitoring (e.g., New Relic, Datadog)
   - Set up application performance monitoring
   - Monitor database query performance

2. **Log Management**:
   - Implement centralized logging
   - Set appropriate log levels for production
   - Regularly analyze logs for performance issues

3. **Regular Performance Audits**:
   - Conduct regular performance testing
   - Identify and address bottlenecks
   - Implement continuous performance improvements

By following these deployment guidelines and optimization strategies, the Government Administration Portal can be deployed efficiently on a single server to meet the needs of Tonga's government services. The recommended hardware specifications provide sufficient capacity for 500-800 concurrent active users and 50,000+ registered users, which significantly exceeds the requirements for Tonga's population of approximately 110,000 citizens. Complex scaling strategies are unnecessary for Tonga's deployment, as a single server configuration offers ample capacity for current needs and future growth.
