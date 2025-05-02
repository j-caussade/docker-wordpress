# Docker Setup for WordPress with MySQL and phpMyAdmin

This project sets up a WordPress application using Docker with MySQL as the database and phpMyAdmin for database management.

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

## Project Structure

- **docker-compose.yml**: Defines the services for the WordPress application, MySQL database, and phpMyAdmin.
- **.env.example**: Example environment file to set up your environment variables.
- **db/**: Directory to store MySQL database files.
- **wordpress/**: Directory to store WordPress application files.

## Resources

- [Docker Documentation](https://docs.docker.com/reference/samples/wordpress/)
- [WordPress Docker Image](https://hub.docker.com/_/wordpress)

## License

This project is licensed under the MIT License.
