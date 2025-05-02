# Docker Setup for WordPress with MySQL and phpMyAdmin

This project sets up a WordPress application using Docker with MySQL as the database and phpMyAdmin for database management.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
  - [1. Clone the Repository](#1-clone-the-repository)
  - [2. Set Up Environment Variables](#2-set-up-environment-variables)
  - [3. Run Docker Compose](#3-run-docker-compose)
  - [4. Access the Applications](#4-access-the-applications)
  - [5. Stopping the Services](#5-stopping-the-services)
- [Deploying on a VPS with Apache](#deploying-on-a-vps-with-apache)
  - [Prerequisites](#prerequisites-1)
  - [Steps](#steps)
- [Project Structure](#project-structure)
- [Resources](#resources)

## Prerequisites

- [Docker](https://www.docker.com/get-started) installed on your machine.
- [Docker Compose](https://docs.docker.com/compose/install/) installed and enabled.

## Getting Started

### 1. Clone the Repository

Clone this repository to your local machine:

```bash
git clone git@github.com:j-caussade/docker-wordpress.git
cd docker-wordpress
```

### 2. Set Up Environment Variables

Create a `.env` file by copying the `.env.example` file and updating the values as needed:

```bash
cp .env.example .env
```

Edit the `.env` file to set your environment variables:

```env
DB_ROOT_PASSWORD=your_root_password
DB_NAME=your_database_name
DB_USER=your_user_name
DB_PASSWORD=your_user_password
DB_HOST=db
```

### 3. Run Docker Compose

Start the services defined in the `docker-compose.yml` file:

```bash
docker compose up -d
```

This command will start the following services:

- **db**: MySQL database service.
- **wordpress**: WordPress application service.
- **phpmyadmin**: phpMyAdmin service for database management.

### 4. Access the Applications

- **WordPress**: Access the WordPress setup page at `http://localhost:8080`.
- **phpMyAdmin**: Access phpMyAdmin at `http://localhost:8081` to manage your MySQL database.

### 5. Stopping the Services

To stop and remove the containers, run:

```bash
docker compose down
```

## Deploying on a VPS with Apache

### Prerequisites

- A VPS with Ubuntu installed.
- Docker and Docker Compose installed on your VPS.
- SSH access to your VPS.
- Apache installed and configured on your VPS.
- SSH key added to your GitHub account for authentication.

### Steps

1. **Clone the Repository**

   Clone the repository on your VPS in the `~/var/www/` directory:

   ```bash
   git clone git@github.com:j-caussade/docker-wordpress.git
   cd docker-wordpress
   ```

2. **Set Up Environment Variables**

   Create and edit the `.env` file from the `.env.example` template:

   ```bash
   cp .env.example .env
   nano .env
   ```

3. **Run Docker Compose**

   Start the services:

   ```bash
   docker compose up -d
   ```

4. **Configure Apache as a Reverse Proxy**

   To ensure WordPress correctly handles HTTPS, configure Apache to set the `X-Forwarded-Proto` header.

   - **Enable Necessary Modules**:

     Ensure the necessary Apache modules are enabled:

     ```bash
     sudo a2enmod proxy
     sudo a2enmod proxy_http
     sudo a2enmod headers
     sudo a2enmod ssl
     sudo systemctl reload apache2
     ```

   - **Edit Your Virtual Host Configuration**:

     Edit your Apache virtual host configuration file (e.g., `/etc/apache2/sites-available/your_site.conf`):

     ```apache
     <VirtualHost *:80>
         ServerName your_domain.com
         DocumentRoot /var/www/docker-wordpress

         RewriteEngine on
         RewriteCond %{SERVER_NAME} =your_domain.com
         RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,NE,R=permanent]
     </VirtualHost>

     <VirtualHost *:443>
         ServerName your_domain.com
         DocumentRoot /var/www/docker-wordpress

         SSLEngine on
         SSLCertificateFile /etc/letsencrypt/live/your_domain.com/fullchain.pem
         SSLCertificateKeyFile /etc/letsencrypt/live/your_domain.com/privkey.pem

         ProxyPreserveHost On
         ProxyPass / http://localhost:8080/
         ProxyPassReverse / http://localhost:8080/

         RequestHeader set X-Forwarded-Proto "https"
     </VirtualHost>
     ```

   - **Reload Apache**:

     After editing the configuration, reload Apache to apply the changes:

     ```bash
     sudo systemctl reload apache2
     ```

## Project Structure

- **docker-compose.yml**: Defines the services for the WordPress application, MySQL database, and phpMyAdmin.
- **.env.example**: Example environment file to set up your environment variables.
- **db/**: Directory to store MySQL database files.
- **wordpress/**: Directory to store WordPress application files.

## Resources

- [Docker Documentation](https://docs.docker.com/reference/samples/wordpress/)
- [WordPress Docker Image](https://hub.docker.com/_/wordpress)
