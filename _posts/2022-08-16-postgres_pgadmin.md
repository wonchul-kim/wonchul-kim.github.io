---
layout: post
title: postgres & pgadmin
category: Database
tag: DB, RMS
---

## Run postgresql & pgadmin with docker-compose

#### docker-compose.yml

```
version: "3"

services:
  postgres:
    image: postgres:latest
    container_name: aiv-postgres
    restart: always
    ports:
      - "5432:5432"
    environment:
      POSTGRES_USER: "${POSTGRES_USER_ID}"
      POSTGRES_PASSWORD: "${POSTGRES_USER_PASSWORD}"
    volumes:
      - ${POSTGRES_HOME_DIR}/data/:/var/lib/postgresql/data
    networks:
      - postgres

  pgadmin:
    user: root
    image: dpage/pgadmin4
    container_name: aiv-pgadmin
    environment:
      PGADMIN_DEFAULT_EMAIL: "${PGADMIN_DEFAULT_EMAIL}"
      PGADMIN_DEFAULT_PASSWORD: "${PGADMIN_DEFAULT_PASSWORD}"
      # PGADMIN_CONFIG_SERVER_MODE: 'False'
    volumes:
      - ${PGADMIN_HOME_DIR}/:/var/lib/pgadmin
    ports:
      - "5050:80"
    networks:
      - postgres
    restart: always
    depends_on:
      - postgres

networks:
  postgres:
    driver: bridge

```


#### .env

```
POSTGRES_USER_ID=<user name to register>
POSTGRES_USER_PASSWORD=<user password>
POSTGRES_HOME_DIR=<directory to mount>

PGADMIN_DEFAULT_EMAIL=<user email to register>
PGADMIN_DEFAULT_PASSWORD=<user password>
PGADMIN_HOME_DIR=<directory to mount>
```

`.env`가 잘 반영되는지 확인

```
docker-compose config
```

#### postgres 접속 확인

```
telnet 127.0.0.1 5432
```

```
psql -h 127.0.0.1 -U <user name>
```

#### pgadmin 접속 확인

`localhost:5050`으로 접속하여 앞선 로그인 정보로 로그인

`Servers`가 비어있을 것이며, 추가한다.

- `General` 탭
  - `Name`으로 하고 싶은거 기입
- `Connection` 탭
  - `Host name/address`: `docker-compose`에서 기입한 postgres container name 기입
  - `Username`: `docker-compose`에서 기입한 postgres user 기입
  - `Password`: `docker-compose`에서 기입한 postgres password 기입
