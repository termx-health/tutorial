> **WARNING!**
> This guide is intended for local quickstart and not suited for production environments.{.is-warning}

The quickest way to run TermX locally is to clone the [**termx-quick-start**](https://github.com/termx-health/termx-quick-start/) repository and start it:

```s
git clone https://github.com/termx-health/termx-quick-start.git
cd termx-quick-start
docker-compose pull && docker-compose up -d
```

The repository is the **authoritative, always-current** source for the local stack — its `docker-compose.yml`, `pginit.sql` and `*.env` files are maintained alongside the platform.

> The step-by-step walkthrough below reproduces a **simplified** version of that stack to explain how the pieces fit together. It is illustrative and may lag behind the repository — for an up-to-date, runnable setup always use the [termx-quick-start](https://github.com/termx-health/termx-quick-start/) repository.
{.is-info}

---

# {.tabset}
## Docker Compose

Create folder (for example `termx`) and move to this folder.

### 1. Create postgres init file
*pginit.sql*
```
CREATE ROLE tx_admin LOGIN PASSWORD 'test' NOSUPERUSER INHERIT NOCREATEDB CREATEROLE NOREPLICATION;
CREATE ROLE tx_app   LOGIN PASSWORD 'test' NOSUPERUSER INHERIT NOCREATEDB CREATEROLE NOREPLICATION;
CREATE ROLE tx_viewer NOLOGIN NOSUPERUSER INHERIT NOCREATEDB NOCREATEROLE NOREPLICATION;
CREATE DATABASE termx WITH OWNER = tx_admin ENCODING = 'UTF8' TABLESPACE = pg_default CONNECTION LIMIT = -1;
grant temp on database termx to tx_app;
grant connect on database termx to tx_app;

set core.env  = 'dev';
ALTER SYSTEM SET core.env = 'dev';
select pg_reload_conf();
```

### 2. Create docker compose file
*docker-compose.yml*
```
version: '3.9'
services:
  termx-postgres:
    restart: unless-stopped
    image: postgres:18
    shm_size: 1g
    container_name: termx-postgres
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
      - POSTGRES_DB=postgres
    volumes:
      - ./pginit.sql:/docker-entrypoint-initdb.d/init.sql
    ports:
      - 5432:5432

  termx-server:
    restart: unless-stopped
    image: ghcr.io/termx-health/termx-server:latest
    container_name: termx-server
    depends_on:
      - termx-postgres
    environment:
      - DB_URL=jdbc:postgresql://termx-postgres:5432/termx
      - DB_APP_PASSWORD=test
      - DB_ADMIN_PASSWORD=test
      - DB_POOL_SIZE=10
      - JAVA_OPTS=-Xmx1800m -Dauth.dev.allowed=true -Dio.netty.leakDetection.level=advanced
      - MICRONAUT_SERVER_CORS_ENABLED=true
      - TERMX_WEB_URL=http://localhost:4200
      - CHEF_URL=http://fsh-chef:3000
      - BOB_MINIO_URL=http://termx-minio:9000
      - BOB_MINIO_ACCESS_KEY=bob
      - BOB_MINIO_SECRET_KEY=bobobobo
      - SNOWSTORM_URL=https://snowstorm.termx.org/
      - SNOWSTORM_BRANCH=MAIN/SNOMEDCT-EE
    healthcheck:
      test: [ "CMD", "curl", "-f", "http://termx-server:8200/health" ]
      interval: 1s
      timeout: 5s
      retries: 60
    ports:
      - 8200:8200
    mem_reservation: 2g

  termx-web:
    restart: unless-stopped
    image: ghcr.io/termx-health/termx-web:latest
    container_name: termx-web
    depends_on:
      - termx-server
    environment:
      - TERMX_API=http://localhost:8200
      - CHEF_URL=http://localhost:8500
      - FML_EDITOR=http://localhost:8502
      - PLANT_UML_URL=http://localhost:8501
      - OAUTH_ISSUER=dummy
      - OAUTH_CLIENT_ID=dummy
      - DEFAULT_LANGUAGE=fr
      - UI_LANGUAGES=["en","fr"]
      - CONTENT_LANGUAGES=["en","fr","pl"]
      - EXTRA_LANGUAGES={"pl":{"en":"Polish","fr":"Polonais"}}
    ports:
      - 4200:80

  termx-minio:
    restart: unless-stopped
    image: quay.io/minio/minio:latest
    container_name: termx-minio
    command: server /data --console-address ":9001"
    environment:
      - MINIO_ROOT_USER=minio
      - MINIO_ROOT_PASSWORD=miniominio
    ports:
      - 9100:9000
      - 9101:9001

  termx-minio-init:
    container_name: termx-minio-init
    restart: "no"
    image: quay.io/minio/mc
    entrypoint: /bin/sh
    command: -c "mc config host add minio http://172.17.0.1:9100 minio miniominio && (mc admin user info minio bob || (mc admin user add minio bob bobobobo && mc admin policy attach minio readwrite --user bob))"
    depends_on:
      - termx-minio

  fsh-chef:
    restart: unless-stopped
    image: ghcr.io/termx-health/termx-chef:latest
    container_name: fsh-chef
    ports:
      - 8500:3000
    mem_reservation: 1g

  plantuml-server:
    restart: unless-stopped
    image: plantuml/plantuml-server:jetty
    container_name: plantuml-server
    ports:
      - 8501:8080

  termx-fml-editor:
    restart: unless-stopped
    image: ghcr.io/termx-health/termx-fml:latest
    container_name: termx-fml-editor
    ports:
      - 8502:80

```

Read more about the configuration of containers in the [installation guide](page:installation-guide).

### 3. Run it!

Install and run binaries
```s
docker-compose pull && docker-compose up -d
```

Check the status of installation process
```
docker ps && docker logs -f termx-server
```

First startup may take up to 1 minute. Wait until text `Startup completed in NNNNms. Server Running: http://XXXXX:8200` appears. 

Web application should be available on http://localhost:4200

### 4. Update it later (if needed)

Move to the folder with `docker-compose.yml` (in our example `termx`).

Get updates and restart updated containers.

```
docker-compose pull && docker-compose up -d
```

### 5. Stopping containers

Move to the folder with `docker-compose.yml` (in our example `termx`).
```
docker-compose stop
```








