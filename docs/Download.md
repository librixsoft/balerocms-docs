# Guide: Build, Pull, and Run the BaleroCMS Docker Image

This document summarizes the steps to **build the production image of BaleroCMS**, push it to Docker Hub, pull it from Docker Hub, and run it locally.

---

## 1. Build the image locally (optional)

```bash
# Stop and remove any previous container
docker rm -f balero

# Build the production image locally (optional if you want to modify before pushing)
docker build -t lastprophet/balerocms:latest -f Dockerfile.prod .
```

---

## 2. Push the image to Docker Hub

```bash
# Log in to Docker Hub (if not already logged in)
docker login

# Push the image
docker push lastprophet/balerocms:latest
```

> If you encounter an access error, tag explicitly:
>
> ```bash
> docker tag lastprophet/balerocms:latest docker.io/lastprophet/balerocms:latest
> docker push docker.io/lastprophet/balerocms:latest
> ```

---

## 3. Pull the image from Docker Hub (for other machines or users)

```bash
docker pull lastprophet/balerocms:latest
```

---

## 4. Run the image locally

```bash
# Stop and remove any previous container
docker rm -f balero

# Run the image
docker run -d --name balero -p 8080:80 lastprophet/balerocms:latest
```

Then open your browser at:

```
http://localhost:8080
```

---

## 5. Check container logs

```bash
docker logs -f balero
```

This helps identify any issues with permissions, Apache, or PHP.

---

## 6. Stop the container

```bash
docker stop balero
docker rm balero  # optional if you want to remove the container
```

This guide allows you to **build, push, pull, and run** the BaleroCMS production image, making it ready to share and run on any machine.
