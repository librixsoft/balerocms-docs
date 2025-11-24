# BaleroCMS Documentation

## Clone the repository

```bash
git clone git@github.com:librixsoft/balerocms.git
cd balerocms
```

## Install

Set write permissions to config file:

```bash
chmod 755 ./resources/config/balero.config.json
```

---

## Starting the environment

- First time or after changing Dockerfile/images:

```bash
docker-compose up -d --build
```

- `-d` → run in background
- `--build` → rebuild images before starting containers

- Just start existing containers:

```bash
docker-compose up -d
```

## Quick start

- BaleroCMS: http://localhost:8080
- Dashboard: http://localhost:8080/login

That's all — if you open http://localhost:8080 you will see the Balero CMS Setup Wizard!

## Auto-generated resources

When you start the application for the first time, it will generate some resources automatically:

### Cache Folder

The "/cache/" folder stores **auto-generated cache files** used by the application to improve routing performance.

Auto-generated DTOs and routes are cached in this folder.





