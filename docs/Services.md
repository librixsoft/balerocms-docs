# Services

## Accessing Services

### BaleroCMS
- URL: http://localhost:8080

### MySQL
- **Host:** localhost
- **Port:** 3307
- **User:** root
- **Database:** balero_cms
- **Password:** root

### phpMyAdmin
- URL: http://localhost:8081
- **User:** root
- **Password:** root

### SonarQube
- URL: http://localhost:9000
- **User:** admin
- **Password:** admin

## MySQL Storage Modes

You can configure MySQL in two ways:

### 1. In-memory (RAM)

- Data is lost when the container stops.
- Ideal for development and quick tests.

### 2. Persistent with volume

- Data is stored in a Docker volume.
- Recommended for production use.

In `docker-compose.yml` you can switch between these two options by commenting/uncommenting the relevant section.
