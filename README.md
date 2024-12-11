# README for Docker-Compose Setup

This README explains how to use the provided `docker-compose.yml` file to set up and run a WordPress environment with a MySQL database.

## Prerequisites

Before using this setup, ensure the following tools are installed on your system:

- Docker
- Docker Compose

## Overview

The `docker-compose.yml` file sets up two services:

1. **MySQL Database**
   - Stores data for the WordPress site.
   - Configurable with environment variables.
2. **WordPress**
   - The content management system.
   - Connects to the MySQL database.

## Services Configuration

### MySQL Service (`db`)

- **Image**: `mysql:5.7`
- **Volumes**:
  - `db_data:/var/lib/mysql`: Persistent storage for the database data.
- **Restart Policy**: Always restarts the container if it stops.
- **Environment Variables**:
  - `MYSQL_ROOT_PASSWORD`: Password for the root user.
  - `MYSQL_DATABASE`: Name of the initial database.
  - `MYSQL_USER`: Name of a new user for the database.
  - `MYSQL_PASSWORD`: Password for the new user.

### WordPress Service (`wordpress`)

- **Image**: `wordpress:latest`
- **Ports**:
  - `8080:80`: Maps port 80 in the container to port 8080 on the host.
- **Restart Policy**: Always restarts the container if it stops.
- **Environment Variables**:
  - `WORDPRESS_DB_HOST`: Hostname for the database (in this case, `db`).
  - `WORDPRESS_DB_USER`: MySQL username.
  - `WORDPRESS_DB_PASSWORD`: MySQL user password.
  - `WORDPRESS_DB_NAME`: MySQL database name.

## How to Use

1. Create a file named `docker-compose.yml` and paste the following content:

```yaml
db:
  image: mysql:5.7
  volumes:
    - db_data:/var/lib/mysql
  restart: always
  environment:
    MYSQL_ROOT_PASSWORD: your_mysql_root_password
    MYSQL_DATABASE: your_mysql_database_name
    MYSQL_USER: your_mysql_username
    MYSQL_PASSWORD: your_mysql_password

wordpress:
  depends_on:
    - db
  image: wordpress:latest
  ports:
    - "8080:80"
  restart: always
  environment:
    WORDPRESS_DB_HOST: db:3306
    WORDPRESS_DB_USER: your_mysql_username
    WORDPRESS_DB_PASSWORD: your_mysql_password
    WORDPRESS_DB_NAME: your_mysql_database_name

volumes:
  db_data:
```

2. Replace the placeholders (`your_mysql_*`) in the environment variables with your desired values.

3. Run the following command to start the services:

```bash
docker-compose up -d
```

4. Open a browser and navigate to `http://localhost:8080` to access the WordPress setup page.

## Volumes

The `db_data` volume ensures that the MySQL database data persists even if the container is stopped or removed.

## Stopping the Services

To stop the containers, run:

```bash
docker-compose down
```

This command stops and removes the containers but preserves the `db_data` volume.

## Additional Notes

- Ensure that the chosen ports (e.g., `8080`) are not in use by other services on your system.
- For a production environment, consider using secure passwords and configuring SSL.

## Troubleshooting

- **Issue**: Containers fail to start.
  - **Solution**: Check for typos in the `docker-compose.yml` file and ensure Docker is running.
- **Issue**: Unable to connect to the database.
  - **Solution**: Verify the environment variables for both services.
