# Maintenance

This section provides comprehensive guidance on maintaining the Government Administration Portal, including update procedures, backup strategies, troubleshooting common issues, monitoring recommendations, and long-term maintenance best practices.

## Update Procedures

Keeping the platform updated is essential for security, performance, and functionality. This section outlines the procedures for different types of updates.

### System Requirements

The Government Administration Portal requires the following system specifications:

| Component | Requirement |
|-----------|-------------|
| PHP | 8.3+ |
| Laravel | 11.x |
| FilamentPHP | 3.x |
| Livewire | 3.x |
| Node.js | 20.0+ |
| MariaDB | 10.5+ or MySQL 8.0+ |
| Redis | 6.0+ |
| Operating System | Ubuntu 24.04 LTS |

These requirements align with the deployment specifications and should be maintained during all updates to ensure system compatibility and optimal performance.

### Minor Updates

Minor updates include bug fixes, security patches, and small feature enhancements. These updates should be applied regularly to maintain system integrity.

#### Preparation

1. **Review Update Notes**:
   - Check the changelog for any breaking changes
   - Identify any database migrations that will be run
   - Note any configuration changes required

2. **Create a Backup**:
   - Back up the database
   - Back up the application files
   - Store backups in a secure location

3. **Schedule Maintenance Window**:
   - Notify users of planned maintenance
   - Choose a low-traffic period for the update
   - Prepare maintenance mode message

#### Update Process

1. **Enable Maintenance Mode**:
   ```bash
   cd /var/www/tongacp
   php artisan down --message="The system is being updated. Please check back in 30 minutes."
   ```

2. **Pull Latest Code**:
   ```bash
   git fetch origin
   git checkout main
   git pull origin main
   ```

3. **Update Dependencies**:
   ```bash
   composer install --no-dev --optimize-autoloader
   # Ensure Node.js 20.0+ is being used
   node -v
   npm ci
   npm run build
   ```

4. **Run Migrations**:
   ```bash
   php artisan migrate --force
   ```

5. **Clear and Rebuild Cache**:
   ```bash
   php artisan optimize:clear
   php artisan config:cache
   php artisan route:cache
   php artisan view:cache
   ```

6. **Update Permissions**:
   ```bash
   sudo chown -R www-data:www-data /var/www/tongacp
   sudo chmod -R 755 /var/www/tongacp
   sudo chmod -R 775 /var/www/tongacp/storage /var/www/tongacp/bootstrap/cache
   ```

7. **Restart Services**:
    ```bash
    sudo systemctl restart php8.3-fpm
    sudo supervisorctl restart "tongacp-worker:*"
    ```

8. **Disable Maintenance Mode**:
   ```bash
   php artisan up
   ```

9. **Verify Update**:
   - Check application version: `php artisan about`
   - Test key functionality
   - Monitor logs for errors: `tail -f storage/logs/laravel.log`

### Major Updates

Major updates involve significant changes to the platform, such as upgrading to a new Laravel 11.x or FilamentPHP 3.x version. These updates require more careful planning and testing.

#### Preparation

1. **Create a Test Environment**:
   - Clone the production environment to a staging server
   - Ensure staging environment mirrors production

2. **Comprehensive Backup**:
   - Full database backup
   - Complete application backup
   - Configuration files backup
   - Store backups in multiple secure locations

3. **Review Update Requirements**:
   - Check for PHP version requirements (minimum PHP 8.3+)
   - Identify dependency changes
   - Note breaking changes and required code modifications
   - Plan for extended maintenance window

#### Update Process

1. **Test in Staging Environment**:
   - Apply the update to the staging environment first
   - Follow the update steps documented for the specific version
   - Test all critical functionality
   - Document any issues and their solutions

2. **Update Production Environment**:
   - Schedule extended maintenance window
   - Notify users well in advance
   - Enable maintenance mode
   - Follow the same update steps as tested in staging
   - Apply any additional fixes identified during staging testing

3. **Post-Update Verification**:
   - Comprehensive testing of all modules
   - Monitor system performance
   - Check logs for errors
   - Verify all integrations are functioning

4. **Rollback Plan**:
   If critical issues are encountered:
   - Restore from backup
   - Revert to previous code version
   - Notify users of rollback and timeline for retry

### Plugin and Extension Updates

The platform uses various plugins and extensions that may require separate update procedures.

1. **Identify Plugin Updates**:
   - Check for updates to FilamentPHP plugins
   - Review plugin changelogs for breaking changes
   - Prioritize security-related updates

2. **Update Process**:
   - Update one plugin at a time
   - Test functionality after each update
   - Document any configuration changes

3. **Custom Plugin Compatibility**:
   - For custom-developed plugins, ensure compatibility with core updates
   - Update custom plugins as needed
   - Test integration points thoroughly

## Backup Strategies

A robust backup strategy is essential for data security and disaster recovery.

### Database Backups

1. **Automated Daily Backups**:
   ```bash
   #!/bin/bash
   TIMESTAMP=$(date +"%Y%m%d%H%M%S")
   BACKUP_DIR="/var/backups/tongacp/database"
   mkdir -p $BACKUP_DIR
   
   # Database backup
   mariadb-dump -u tongacp_user -p'secure_password' tongacp | gzip > $BACKUP_DIR/tongacp_db_$TIMESTAMP.sql.gz
   
   # Rotate backups (keep last 30 days)
   find $BACKUP_DIR -type f -name "tongacp_db_*.sql.gz" -mtime +30 -delete
   
   # Copy to offsite storage
   rsync -avz $BACKUP_DIR/tongacp_db_$TIMESTAMP.sql.gz backup-server:/backups/tongacp/database/
   ```

2. **Backup Before Updates**:
   Always create a manual backup before applying updates:
   ```bash
   php artisan db:backup --filename=pre_update_$(date +"%Y%m%d%H%M%S").sql
   ```

3. **Backup Verification**:
   Regularly test database restore process:
   ```bash
   # Create test database
   mariadb -e "CREATE DATABASE tongacp_test;"
   
   # Restore backup to test database
   gunzip < /var/backups/tongacp/database/tongacp_db_20250501120000.sql.gz | mariadb tongacp_test
   
   # Verify data integrity
   mariadb -e "SELECT COUNT(*) FROM users;" tongacp_test
   ```

### File Backups

1. **Automated File Backups**:
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
   
   # Copy to offsite storage
   rsync -avz $BACKUP_DIR/tongacp_files_$TIMESTAMP.tar.gz backup-server:/backups/tongacp/files/
   ```

2. **Media Files Backup**:
   ```bash
   #!/bin/bash
   TIMESTAMP=$(date +"%Y%m%d%H%M%S")
   BACKUP_DIR="/var/backups/tongacp/media"
   mkdir -p $BACKUP_DIR
   
   # Media files backup
   tar -czf $BACKUP_DIR/tongacp_media_$TIMESTAMP.tar.gz -C /var/www/tongacp/storage/app .
   
   # Rotate backups (keep last 14 days)
   find $BACKUP_DIR -type f -name "tongacp_media_*.tar.gz" -mtime +14 -delete
   
   # Copy to offsite storage
   rsync -avz $BACKUP_DIR/tongacp_media_$TIMESTAMP.tar.gz backup-server:/backups/tongacp/media/
   ```

### Backup Storage and Retention

1. **Local Storage**:
   - Store backups on a separate disk from the application
   - Implement appropriate permissions to prevent unauthorized access
   - Monitor available storage space

2. **Offsite Storage**:
   - Replicate backups to a remote server or cloud storage
   - Encrypt backups before transferring to offsite storage
   - Verify successful transfer of backups

3. **Retention Policy**:
   - Daily backups: Retain for 30 days
   - Weekly backups: Retain for 3 months
   - Monthly backups: Retain for 1 year
   - Pre-update backups: Retain until next major update is confirmed stable

### Disaster Recovery Plan

1. **Document Recovery Procedures**:
   - Step-by-step instructions for database restoration
   - File recovery procedures
   - Service restart sequence
   - Contact information for key personnel

2. **Regular Recovery Testing**:
   - Conduct quarterly recovery drills
   - Test restoration to a separate environment
   - Verify application functionality after recovery
   - Document recovery time and any issues encountered

3. **High Availability Considerations**:
   - For critical deployments, implement database replication
   - Consider standby application servers
   - Automate failover procedures where possible

## Troubleshooting Common Issues

This section provides guidance on diagnosing and resolving common issues that may arise during operation of the Government Administration Portal.

### Database Connection Issues

1. **Symptoms**:
   - "Could not connect to database" errors
   - Slow page loads followed by database errors
   - Intermittent database connection failures

2. **Diagnosis**:
   ```bash
   # Check database service status
   sudo systemctl status mariadb
   
   # Check database connection from command line
   mariadb -u tongacp_user -p -h localhost tongacp -e "SELECT 1"
   
   # Check database connection settings in .env
   cat .env | grep DB_
   ```

3. **Solutions**:
   - Restart database service: `sudo systemctl restart mariadb`
   - Verify database credentials in `.env` file
   - Check database server load and resources
   - Increase database connection timeout in `config/database.php`
   - Check for database connection leaks in custom code

### Performance Degradation

1. **Symptoms**:
   - Slow page loads
   - Timeout errors
   - High server resource usage

2. **Diagnosis**:
   ```bash
   # Check server load
   top
   
   # Check PHP-FPM status
   sudo systemctl status php8.3-fpm
   
   # Check for slow queries
   sudo tail -f /var/log/mariadb/slow-query.log
   
   # Check application logs
   tail -f storage/logs/laravel.log
   ```

3. **Solutions**:
   - Identify and optimize slow database queries
   - Implement or adjust caching strategies
   - Check for memory leaks in custom code
   - Optimize PHP configuration (memory_limit, max_execution_time)
   - Scale server resources if necessary
   - Implement database query caching

### File Upload Issues

1. **Symptoms**:
   - File upload failures
   - "Maximum file size exceeded" errors
   - Incomplete file uploads

2. **Diagnosis**:
   ```bash
   # Check PHP upload settings
   php8.3 -i | grep upload
   
   # Check storage directory permissions
   ls -la storage/app/public
   
   # Check available disk space
   df -h
   ```

3. **Solutions**:
   - Adjust PHP settings in `php.ini`:
     ```
     upload_max_filesize = 20M
     post_max_size = 20M
     max_execution_time = 300
     ```
   - Correct storage directory permissions:
     ```bash
     sudo chown -R www-data:www-data storage
     sudo chmod -R 775 storage
     ```
   - Free up disk space or expand storage
   - Check and adjust Nginx client_max_body_size

### Authentication Problems

1. **Symptoms**:
   - Users unable to log in
   - Session expiration issues
   - "Unauthenticated" errors after login

2. **Diagnosis**:
   ```bash
   # Check session configuration
   cat config/session.php
   
   # Check for cookie issues
   cat .env | grep SESSION
   
   # Check for failed login attempts
   tail -f storage/logs/laravel.log | grep auth
   ```

3. **Solutions**:
   - Clear session data: `php artisan session:clear`
   - Verify session driver configuration
   - Check for cookie domain/path issues
   - Ensure secure and HTTP-only flags are set for cookies
   - Verify that session storage is properly configured

### Queue Processing Issues

1. **Symptoms**:
   - Background jobs not processing
   - Email notifications not being sent
   - Queue backlog growing

2. **Diagnosis**:
    ```bash
    # Check Supervisor status
    sudo supervisorctl status
    
    # Check specific queue worker status
    sudo supervisorctl status "tongacp-worker:*"
    
    # Check Supervisor logs
    sudo tail -f /var/log/supervisor/supervisord.log
    
    # Check worker-specific logs
    sudo tail -f /var/www/tongacp/storage/logs/worker.log
    
    # Check queue backlog
    php artisan queue:size
    
    # Check failed jobs
    php artisan queue:failed
    ```

3. **Solutions**:
    - Restart queue workers: `sudo supervisorctl restart "tongacp-worker:*"`
    - Restart individual worker: `sudo supervisorctl restart "tongacp-worker:tongacp-worker_00"`
    - Restart Supervisor service: `sudo systemctl restart supervisor`
    - Increase number of queue workers by editing `/etc/supervisor/conf.d/tongacp-worker.conf` and changing the `numprocs` value
    - Check for errors in failed jobs
    - Verify Redis connection for queue driver
    - Retry failed jobs: `php artisan queue:retry all`
    - Update Supervisor configuration after changes: `sudo supervisorctl reread && sudo supervisorctl update`

### Common Error Codes and Solutions

| Error Code | Description | Possible Solutions |
|------------|-------------|-------------------|
| 500 | Internal Server Error | Check logs for detailed error messages, fix application code issues |
| 503 | Service Unavailable | Check if application is in maintenance mode, verify server resources |
| 404 | Not Found | Check routes configuration, verify URL paths |
| 403 | Forbidden | Check user permissions, verify policy implementation |
| 419 | Page Expired | CSRF token expired, refresh the page or check session configuration |
| 429 | Too Many Requests | Rate limiting triggered, adjust throttling settings or optimize client requests |

## Monitoring Recommendations

Effective monitoring is essential for maintaining system health and quickly identifying issues.

### Server Monitoring

1. **Resource Monitoring**:
   - CPU usage
   - Memory usage
   - Disk space and I/O
   - Network traffic

   Recommended tools:
   - Prometheus with Node Exporter
   - Grafana for visualization
   - Netdata for real-time monitoring

   Example Prometheus configuration:
   ```yaml
   global:
     scrape_interval: 15s
   
   scrape_configs:
     - job_name: 'node'
       static_configs:
         - targets: ['localhost:9100']
   ```

2. **Service Monitoring**:
   - Web server status
   - PHP-FPM status
   - Database server status
   - Redis status
   - Queue worker status

   Example Nginx status configuration:
   ```nginx
   location /nginx_status {
       stub_status on;
       allow 127.0.0.1;
       deny all;
   }
   ```

### Application Monitoring

1. **Error Tracking**:
   - Monitor application logs
   - Track exception rates
   - Alert on critical errors

   Implementation with Laravel Telescope:
   ```bash
   composer require laravel/telescope --dev
   php artisan telescope:install
   php artisan migrate
   ```

   Configure in `config/telescope.php`:
   ```php
   'enabled' => env('TELESCOPE_ENABLED', true),
   'middleware' => [
       'web',
       Authorize::class,
   ],
   ```

2. **Performance Monitoring**:
   - Track response times
   - Monitor database query performance
   - Identify slow endpoints

   Implementation with Laravel Debugbar:
   ```bash
   composer require barryvdh/laravel-debugbar --dev
   ```

   Configure in `.env`:
   ```
   DEBUGBAR_ENABLED=true
   ```

3. **User Activity Monitoring**:
   - Track login attempts
   - Monitor critical operations
   - Identify unusual activity patterns

   Using Laravel's built-in logging:
   ```php
   Log::channel('activity')->info('User logged in', [
       'user_id' => $user->id,
       'ip' => request()->ip(),
   ]);
   ```

### Alerting System

1. **Alert Thresholds**:
   - CPU usage > 80% for 5 minutes
   - Memory usage > 90% for 5 minutes
   - Disk space < 10% free
   - Error rate > 10 errors per minute
   - Response time > 2 seconds for critical endpoints

2. **Alert Channels**:
   - Email notifications
   - SMS alerts for critical issues
   - Integration with messaging platforms (Slack, Teams)
   - On-call rotation system

3. **Alert Management**:
   - Implement alert grouping to prevent alert fatigue
   - Define escalation paths for unresolved issues
   - Document response procedures for common alerts

### Logging Strategy

1. **Centralized Logging**:
   - Aggregate logs from all system components
   - Implement structured logging format
   - Ensure proper log rotation

   Example ELK Stack setup:
   - Filebeat to collect logs
   - Elasticsearch for storage and indexing
   - Kibana for visualization and search

2. **Log Levels**:
   - DEBUG: Detailed debugging information
   - INFO: General operational information
   - WARNING: Non-critical issues
   - ERROR: Runtime errors
   - CRITICAL: System failures

   Configure in `.env`:
   ```
   LOG_LEVEL=warning
   ```

3. **Log Retention**:
   - Error logs: 90 days
   - Access logs: 30 days
   - Debug logs: 7 days

   Example log rotation configuration:
   ```
   /var/www/tongacp/storage/logs/*.log {
       daily
       missingok
       rotate 14
       compress
       delaycompress
       notifempty
       create 0640 www-data www-data
   }
   ```

## Long-Term Maintenance Best Practices

Ensuring the long-term health and sustainability of the Government Administration Portal requires consistent maintenance practices.

### Code Quality Maintenance

1. **Regular Code Reviews**:
   - Review all code changes before deployment
   - Enforce coding standards
   - Use automated code quality tools

   Example PHP_CodeSniffer configuration:
   ```xml
   <?xml version="1.0"?>
   <ruleset name="TongaCP">
       <description>TongaCP coding standard</description>
       <rule ref="PSR12"/>
       <file>app</file>
       <file>config</file>
       <file>database</file>
       <file>routes</file>
       <exclude-pattern>*/vendor/*</exclude-pattern>
   </ruleset>
   ```

2. **Automated Testing**:
   - Maintain comprehensive test suite
   - Run tests before each deployment
   - Track test coverage

   Example PHPUnit configuration:
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <phpunit xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:noNamespaceSchemaLocation="./vendor/phpunit/phpunit/phpunit.xsd"
            bootstrap="vendor/autoload.php"
            colors="true">
       <testsuites>
           <testsuite name="Unit">
               <directory suffix="Test.php">./tests/Unit</directory>
           </testsuite>
           <testsuite name="Feature">
               <directory suffix="Test.php">./tests/Feature</directory>
           </testsuite>
       </testsuites>
       <coverage processUncoveredFiles="true">
           <include>
               <directory suffix=".php">./app</directory>
           </include>
       </coverage>
   </phpunit>
   ```

3. **Documentation Updates**:
   - Keep documentation in sync with code changes
   - Document API changes
   - Maintain clear changelog

### Security Maintenance

1. **Regular Security Audits**:
   - Conduct quarterly security reviews
   - Perform penetration testing annually
   - Review access controls and permissions

2. **Dependency Management**:
   - Regularly update dependencies
   - Monitor for security vulnerabilities
   - Use Composer security checks
   - Ensure all required PHP extensions are installed:
     - BCMath, Ctype, Fileinfo, JSON, Mbstring, OpenSSL, PDO, Tokenizer, XML, DOM
     - GD or Imagick (for image manipulation)
     - Zip, Curl, Intl, OPcache, SQLite, PCRE

   ```bash
   composer audit
   ```

3. **Security Patch Management**:
   - Apply security patches promptly
   - Maintain security update schedule
   - Document security-related changes

### Database Maintenance

1. **Regular Optimization**:
   - Analyze and optimize tables monthly
   - Monitor index performance
   - Clean up unnecessary data

   ```sql
   -- Analyze tables
   ANALYZE TABLE users, entities, tickets, appointments;
   
   -- Optimize tables
   OPTIMIZE TABLE users, entities, tickets, appointments;
   ```

2. **Data Archiving**:
   - Implement data archiving strategy for old records
   - Maintain data retention policies
   - Ensure archived data remains accessible when needed

3. **Schema Management**:
   - Document database schema changes
   - Use migrations for all schema modifications
   - Test migrations thoroughly before deployment

### Performance Tuning

1. **Regular Performance Reviews**:
   - Conduct monthly performance assessments
   - Identify and address performance bottlenecks
   - Monitor user experience metrics

2. **Caching Strategy Updates**:
   - Review and optimize caching configuration
   - Adjust cache TTL based on data volatility
   - Monitor cache hit/miss rates

3. **Resource Scaling**:
   - Plan for resource needs based on usage trends
   - Implement proactive scaling
   - Optimize resource allocation

### Queue System Management with Supervisor

1. **Supervisor Configuration**:
   - The system uses Supervisor to manage queue workers
   - Default configuration file location: `/etc/supervisor/conf.d/tongacp-worker.conf`
   - Default configuration:
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

2. **Queue Prioritization**:
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
   ```

3. **Supervisor Maintenance**:
   - Regularly check Supervisor logs for issues
   - Monitor worker memory usage and restart if needed
   - Update Supervisor configuration after changes:
     ```bash
     sudo supervisorctl reread
     sudo supervisorctl update
     sudo supervisorctl restart "tongacp-worker:*"
     ```

### Knowledge Management

1. **Maintenance Documentation**:
   - Document all maintenance procedures
   - Keep troubleshooting guides updated
   - Maintain system architecture documentation

2. **Knowledge Transfer**:
   - Train multiple team members on maintenance procedures
   - Document tribal knowledge
   - Implement pair maintenance sessions

3. **Incident Management**:
   - Document all incidents and resolutions
   - Conduct post-incident reviews
   - Implement preventive measures based on incident patterns

### Continuous Improvement

1. **User Feedback Collection**:
   - Gather feedback from government staff and citizens
   - Prioritize improvements based on feedback
   - Implement regular enhancement cycles

2. **Technology Stack Evaluation**:
   - Annually review technology stack
   - Plan for major version upgrades
   - Evaluate new technologies for potential benefits

3. **Process Refinement**:
   - Regularly review and improve maintenance processes
   - Automate repetitive maintenance tasks
   - Measure and optimize maintenance efficiency

By following these comprehensive maintenance practices, the Government Administration Portal can remain secure, performant, and reliable throughout its lifecycle, providing consistent value to both government entities and citizens.
