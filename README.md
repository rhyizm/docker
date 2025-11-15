# Docker Images

This repository contains Dockerfiles for various Python images.

## py313-slim-bookworm

A slim Python 3.13 image based on Debian Bookworm.

To build this image, run:

```bash
docker build -t py313-slim-bookworm:python3.13-slim-bookworm -f py313-slim-bookworm/Dockerfile py313-slim-bookworm
```

## py313-alpine

A Python 3.13 image based on Alpine Linux.

To build this image, run:

```bash
docker build -t py313-alpine:python3.13-alpine -f py313-alpine/Dockerfile py313-alpine
```

## mysql-84

An Apple Silicon–ready MySQL 8.4 service configured for Drizzle.

To start it, run:

```bash
docker compose -f mysql-84/docker-compose.yml up -d
```

- Connection string: `mysql://root:QazxSw@127.0.0.1:3306/mydb`
- Create a new database (replace `newdb` as needed):

```bash
mysql -h 127.0.0.1 -P 3306 -u root -pQazxSw -e "CREATE DATABASE newdb;"
```
