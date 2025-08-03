# Drupal CI/CD Project

A Drupal 11 project with Docker-based development environment and CI/CD pipeline setup.

## 🚀 Project Overview

This is a Drupal 11 project built using the `drupal/recommended-project` template with a relocated document root (`web/` directory). The project includes a complete Docker development environment for easy local development and deployment.

<!-- Trigger deploy workflow -->

## 📋 Prerequisites

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)
- [Composer](https://getcomposer.org/download/)

## 🛠️ Project Structure

```
cicd/
├── composer.json              # Drupal project dependencies
├── docker-compose.yml         # Docker services configuration
├── etc/local/                 # Local configuration files
├── oauth/                     # OAuth configuration
├── recipes/                   # Drupal recipes
└── web/                       # Drupal document root
    ├── core/                  # Drupal core files
    ├── modules/               # Custom and contributed modules
    ├── themes/                # Custom and contributed themes
    ├── sites/                 # Site-specific configuration
    └── profiles/              # Installation profiles
```

## 🚀 Quick Start

### 1. Clone the Repository

```bash
git clone <repository-url>
cd cicd
```

### 2. Set Up Environment Variables

Create a `.env` file in the project root with the following variables:

```env
PROJECT_NAME=cicd
PROJECT_BASE_URL=cicd.localhost
DB_ROOT_PASSWORD=your_root_password
DB_NAME=drupal
DB_USER=drupal
DB_PASSWORD=your_db_password
DB_HOST=mariadb
DB_PORT=3306
DB_DRIVER=mysql
PHP_TAG=8.2-dev
NGINX_TAG=1.25
MARIADB_TAG=10.11
NGINX_VHOST_PRESET=drupal9
```

### 3. Install Dependencies

```bash
composer install
```

### 4. Start Docker Services

```bash
docker-compose up -d
```

### 5. Install Drupal

Access your site at `http://localhost:8000` and follow the Drupal installation wizard.

## 🐳 Docker Services

The project includes the following Docker services:

- **MariaDB**: Database server
- **PHP**: PHP-FPM application server
- **Nginx**: Web server
- **Traefik**: Reverse proxy and load balancer

### Service Access

- **Website**: http://localhost:8000
- **Traefik Dashboard**: http://localhost:8080 (uncomment in docker-compose.yml)

## 🔧 Development

### Running Commands

Execute commands inside the PHP container:

```bash
# Drush commands
docker-compose exec php drush status

# Composer commands
docker-compose exec php composer require drupal/module_name

# PHP CLI
docker-compose exec php php -v
```

### Database Management

```bash
# Access MariaDB
docker-compose exec mariadb mysql -u root -p

# Backup database
docker-compose exec mariadb mysqldump -u root -p drupal > backup.sql

# Restore database
docker-compose exec -T mariadb mysql -u root -p drupal < backup.sql
```

### File Permissions

Ensure proper file permissions for Drupal:

```bash
docker-compose exec php chown -R wodby:wodby /var/www/html/web/sites/default/files
docker-compose exec php chmod -R 755 /var/www/html/web/sites/default/files
```

## 📁 Configuration

### Drupal Settings

- Site-specific settings: `web/sites/default/settings.php`
- Local development settings: `web/sites/development.services.yml`
- Example local settings: `web/sites/example.settings.local.php`

### Docker Configuration

- Main configuration: `docker-compose.yml`
- PHP configuration: Environment variables in docker-compose.yml
- Nginx configuration: Uses Drupal 9 preset

## 🔍 Debugging

### Enable Xdebug

Uncomment the Xdebug configuration in `docker-compose.yml`:

```yaml
PHP_XDEBUG: 1
PHP_XDEBUG_MODE: debug
PHP_XDEBUG_START_WITH_REQUEST: "yes"
PHP_IDE_CONFIG: serverName=lw
PHP_XDEBUG_IDEKEY: "lw"
PHP_XDEBUG_CLIENT_HOST: host.docker.internal
```

### View Logs

```bash
# All services
docker-compose logs

# Specific service
docker-compose logs php
docker-compose logs nginx
docker-compose logs mariadb
```

## 🚀 Deployment

### Production Considerations

1. **Security**: Update `.env` file with strong passwords
2. **Performance**: Enable Nginx caching and PHP OPcache
3. **Monitoring**: Set up logging and monitoring
4. **Backup**: Configure automated database and file backups

### Environment Variables

Create environment-specific `.env` files:
- `.env.local` for local development
- `.env.staging` for staging environment
- `.env.production` for production environment

## 📚 Useful Commands

```bash
# Start services
docker-compose up -d

# Stop services
docker-compose down

# Rebuild containers
docker-compose build --no-cache

# View running containers
docker-compose ps

# Access PHP container shell
docker-compose exec php bash

# Clear Drupal cache
docker-compose exec php drush cr

# Update Drupal core
docker-compose exec php composer update drupal/core --with-dependencies
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## 📄 License

This project is licensed under the GPL-2.0-or-later License - see the [LICENSE](LICENSE) file for details.

## 🆘 Support

- [Drupal Documentation](https://www.drupal.org/docs)
- [Drupal Community](https://www.drupal.org/community)
- [Docker Documentation](https://docs.docker.com/)

## 🔗 Links

- [Drupal.org](https://www.drupal.org)
- [Docker Hub](https://hub.docker.com/)
- [Wodby Stack Documentation](https://wodby.com/docs/stacks/drupal/local/)

---

**Note**: This is a development environment. For production deployment, ensure proper security configurations and follow Drupal security best practices. 