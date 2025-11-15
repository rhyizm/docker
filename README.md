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

## MySQL 8.4

ホストに合わせて以下のどちらかを使ってください：

- Apple Silicon/arm64: `mysql-84-arm64v8/docker-compose.yml`
- Windows x86_64 (Linuxコンテナ): `mysql-84-windows-x86/docker-compose.yml`

起動コマンド例：

```bash
docker compose -f mysql-84-arm64v8/docker-compose.yml up -d
# または
docker compose -f mysql-84-windows-x86/docker-compose.yml up -d
```

接続例: `mysql://root:QazxSw@127.0.0.1:3306/drizzle`

DB作成例（`newdb`は任意名に置換）：

```bash
mysql -h 127.0.0.1 -P 3306 -u root -pQazxSw -e "CREATE DATABASE newdb;"
```
