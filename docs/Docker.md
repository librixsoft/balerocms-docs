# Docker

## Docker Services

### Start

```bash
docker-compose up -d
```

### Stop only containers (keep volumes and networks)

```bash
docker-compose stop
```

### Stop and remove containers and networks

```bash
docker-compose down
```

### Stop and remove everything including volumes

```bash
docker-compose down -v
```

> For MySQL in RAM (tmpfs), simply use `docker-compose down` and restart; the database is deleted when the container stops.

## Debug Container

Open shell inside container:

```bash
docker exec -it lamp-app bash
```

Get log in container:

```bash
tail -F /var/log/apache2/error.log
```

## Docker Hub Repository

https://hub.docker.com/r/lastprophet/balerocms
