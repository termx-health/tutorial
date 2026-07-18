## Docker-compose <i class="mdi mdi-docker"></i>

Docker-compose installation contains 3 components: backend server, frontend application and database.


In case you have your own DB instance you may omit the **termx-postgres** service from the YAML file and configure the DB user/password for the terminology server application in the **server.env** file.




### Reserved ports & paths
|         Service | Port | Path             |
|----------------:|------|------------------|
|        Postgres | 5432 | &nbsp;           |
|       TermX web | 9000 | `/`              |
|    TermX server | 8200 | `/api`           |
|         Swagger | 8000 | `/swagger`       |
|            Chef | 8500 | `/chef`          |
| PlantUML Server | 8501 | `/plantuml`      |
|      FML Editor | 8502 | `/fml-editor`    |
|           MinIO | 9100 | &nbsp;           |
|   MinIO console | 9101 | `/minio-console` |
{.dense}

### Example docker-compose file


+++*docker-compose.yml*

```yaml
version: '3.9'
services:
  termx-server:
    restart: unless-stopped
    image: ghcr.io/termx-health/termx-server:latest
    container_name: termx-server
    depends_on:
      - termx-postgres
    env_file:
      - server.env
    environment:
      - DB_URL=jdbc:postgresql://termx-postgres:5432/termx
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
    env_file:
      - web.env
    ports:
      - 9000:80

  termx-postgres:
    restart: unless-stopped
    image: postgres:18
    shm_size: 1g
    container_name: termx-postgres
    volumes:
      - ./pgdata:/var/lib/postgresql/data
    env_file:
      - pg.env
    ports:
      - 5432:5432

  termx-swagger:
    restart: unless-stopped
    image: swaggerapi/swagger-ui
    container_name: termx-swagger
    volumes:
      - ./swagger-config.json:/usr/share/nginx/html/swagger-config.json
    env_file:
      - swagger.env
    ports:
      - 8000:8080

  termx-minio:
    restart: unless-stopped
    image: quay.io/minio/minio
    container_name: termx-minio
    command: server /data --console-address ":9001"
    volumes:
      - ./minio-data:/data
    environment:
      - MINIO_ROOT_USER=minio-user
      - MINIO_ROOT_PASSWORD=minio-password
      - MINIO_CONSOLE_SUBPATH=/minio/
      - MINIO_BROWSER_REDIRECT_URL=http://localhost/minio
    ports:
      - 9100:9000
      - 9101:9001
  
  fsh-chef:
    restart: unless-stopped
    image: ghcr.io/termx-health/termx-chef:latest
    container_name: fsh-chef
    ports:
      - 8500:3000

  plantuml-server:
    restart: unless-stopped
    image: plantuml/plantuml-server:jetty
    container_name: plantuml-server
    environment:
      - BASE_URL=plantuml
    ports:
      - 8501:8080

  termx-fml-editor:
    restart: unless-stopped
    image: ghcr.io/termx-health/termx-fml:latest
    container_name: termx-fml-editor
    environment:
      - BASE_HREF=/fml-editor/
    ports:
      - 8502:80
```

**termx-server** and **termx-web** docker tags match the platform version from the [release notes](page:release-notes). The `latest` tag always points at the newest release.

+++

## Configuration files
## {.tabset}

### server.env
Server environment variables.

```plaintext
DB_URL=jdbc:postgresql://termx-postgres:5432/termx #should be changed when postgres database is not defined in docker-compose.yml
DB_APP_PASSWORD=test #change to whatever you like
DB_ADMIN_PASSWORD=test #change to whatever you like
DB_POOL_SIZE=10 #default pool size
JAVA_OPTS=-Xmx1800m #default max heap size
OAUTH_JWKS_URL=https://sso.termx.dev/realms/terminology/protocol/openid-connect/certs #JWKS url of your oauth SSO server
MICRONAUT_SERVER_CORS_ENABLED=true# for development only
MICRONAUT_SERVER_CORS_CONFIGURATIONS_UI_ALLOWED_ORIGINS=https://dev.termx.org

SNOWSTORM_URL=https://snowstorm.termx.org/ #base url of Snowstorm server
SNOWSTORM_BRANCH=MAIN/SNOMEDCT-EE

GITHUB_APP_NAME=xxxx

TERMX_WEB_URL=http://localhost:4200
CHEF_URL=http://fsh-chef:3000 #internal URL of the Chef (SUSHI/GoFSH) service

# Object storage (Minio) — backs the Binary Object Bank (Bob):
# Wiki attachments and SNOMED / LOINC archive uploads.
BOB_MINIO_URL=http://termx-minio:9000
BOB_MINIO_ACCESS_KEY=xxxx
BOB_MINIO_SECRET_KEY=xxxx

# SMTP email is optional; when unset, notifications are silently skipped.
#SMTP_ENABLED=true
#SMTP_HOST=smtp.example.org
#SMTP_PORT=587
#SMTP_USERNAME=noreply@example.org
#SMTP_PASSWORD=xxxx
#SMTP_FROM=noreply@termx.org
#SMTP_TO_IMPORT=admin@example.org
```

#### Database
- DB_URL. The JDBC address of the DB server (host `termx-postgres` is the Postgres service name inside the compose network).
- DB_APP_PASSWORD. The password of the application user (`tx_app`) in the DB.
- DB_ADMIN_PASSWORD. The password of the admin user (`tx_admin`) used to create the schema and run Liquibase migrations.

#### SNOMED
- SNOWSTORM_URL. The root URL of the [Snowstorm server](page:snowstorm-server).
- SNOWSTORM_BRANCH. The SNOMED edition on the Snowstorm server used by default. For example `SNOWSTORM_BRANCH=MAIN/SNOMEDCT-EE` for the Estonian Edition, `SNOWSTORM_BRANCH=MAIN` for the International Edition.

#### Github
- GITHUB_APP_NAME. The GitHub App used for content publishing. Read more on the [GitHub application page](page:github-app). Register the App following the GitHub [manual](https://docs.github.com/en/apps/creating-github-apps/registering-a-github-app/registering-a-github-app); the client ID and secret are configured for that App.

#### Object storage
- BOB_MINIO_URL, BOB_MINIO_ACCESS_KEY, BOB_MINIO_SECRET_KEY. Connection to the S3-compatible [MinIO service](page:minio-service) that stores Wiki attachments and terminology archive uploads (the "Binary Object Bank", Bob). If the object store URL is not set, object storage is disabled and uploads return `503`. The equivalent `MINIO_URL` / `MINIO_ACCESS_KEY` / `MINIO_SECRET_KEY` names bind to the same properties.

#### Email
- SMTP_*. Optional SMTP configuration for import notifications. Check status at `GET /management/email/status`.

#### Redirect to OAuth SSO
- OAUTH_JWKS_URL. The OAuth JWKS endpoint used to validate JWT access tokens against your SSO server.

#### CORS
- MICRONAUT_SERVER_CORS_CONFIGURATIONS_UI_ALLOWED_ORIGINS. When serving the web app from a different origin, specify the root path to your web URL. 


### web.env
Frontend environment variables.

```plaintext
BASE_HREF=/
OAUTH_ISSUER=https://sso.termx.dev/realms/terminology
OAUTH_CLIENT_ID=term-client
OAUTH_SCOPE=openid

UI_LANGUAGES=["en","fr"]
DEFAULT_LANGUAGE=fr
CONTENT_LANGUAGES=["en","fr","pl"]
EXTRA_LANGUAGES={"pl":{"en":"Polish","fr":"Polonais"}}

SNOWSTORM_URL=https://snowstorm.termx.org/
SNOWSTORM_DAILY_BUILD_URL=https://snowstorm.termx.org/
SNOMED_BROWSER_URL=https://snomed.termx.org/

GUEST_DISABLED=false
SKIN=helex
BRANDING=TermX
```

#### Deployment
`BASE_HREF`
- Specify when your application has a relative URL that differs from the domain URL. For example, set it to `/termx` if your application run on `https://site.org/termx`.
- **Default:** The default value is `/` that match to the domain root, for example `https://site.org`.

#### Security
`OAUTH_ISSUER`
- The address (realm) of the OpenId Connect server. The special keyword `dummy` enables the mode without an SSO server. 

`OAUTH_CLIENT_ID`
- OAuth2 client identifier in `$OAUTH_ISSUER`. The special keyword `dummy` may be used for enabling a Guest account with all possible privileges.

#### API
`TERMX_API`
- The address of server API.
- **Default:** `/api`.

`SWAGGER_URL`
- **Default:** `/swagger/`.

`CHEF_URL`
- **Default:** `/chef/`.

`PLANT_UML_URL`
- **Default:** `/plantuml`.

`FML_EDITOR`
- **Default:** `/fml-editor`.

#### Languages

`DEFAULT_LANGUAGE`
- Default language will be used if your browser locale does not belong to the list of UI languages.
- **Default:** `/en`.

`UI_LANGUAGES`
- Languages supported in the user interface. The list of supported languages is listed [here](https://github.com/termx-health/termx-web/tree/main/app/src/assets/i18n). You should add a translation file if you want to have UI in your language.
- **Default:** `'en', 'et', 'lt', 'de', 'fr', 'nl', 'cs'`

`CONTENT_LANGUAGES`
- List of languages used in the multilingual inputs. If a language is missing from the list of supported languages, specify it as an additional language.
- **Default:** `$UI_LANGUAGES`

`EXTRA_LANGUAGES`
- Languages may be used in the multilingual inputs and inputs. It should be presented as JSON where language code is key and pairs language+translation for every UI language should be added.

`OAUTH_SCOPE`
- OAuth2 scopes requested during login. **Default:** `openid`.

`GUEST_DISABLED`
- When `true`, disables the anonymous Guest account so that only authenticated users may access the application. **Default:** `false`.

#### Branding
`SKIN`
- Named UI skin. **Default:** `helex`.

`SKIN_URL`
- URL of an external skin stylesheet, used instead of a bundled `SKIN`.

`BRANDING`
- Product name shown in the UI header.

#### Snowstorm
`SNOWSTORM_URL`
- The address of the [Snowstorm server](page:snowstorm-server) used for SNOMED CT expansion and lookup.

`SNOWSTORM_DAILY_BUILD_URL`
- The address of the Snowstorm server's daily build.

`SNOMED_BROWSER_URL` / `SNOMED_BROWSER_DAILY_BUILD_URL`
- The address of the SNOMED CT browser (and its daily build) linked from the [SNOMED CT browser](page:snomed-ct-browser).


You can validate configured parameters locally in [env.js](http://localhost:4200/assets/env.js).
*Some fields may be empty; if so, the default value will be used.*


### pg.env
Postgres-related configuration for initial setup. You a free to change those values.

```plaintext
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
POSTGRES_DB=postgres
```

- POSTGRES_DB. The main database. Do not change it.
- POSTGRES_USER. The name of the superuser.
- POSTGRES_PASSWORD. The password of the superuser.


### swagger.env
```plaintext
CONFIG_URL=https://dev.termx.org/swagger/swagger-config.json
OAUTH_CLIENT_ID=term-client
OAUTH_REALM=terminology
OAUTH_USE_PKCE=true
PERSIST_AUTHORIZATION=true
BASE_URL=/swagger
```


### swagger-config.json
```json
{
  "urls": [
    {
      "url": "https://dev.termx.org/api/swagger/termx.yml",
      "name": "termx"
    },
    {
      "url": "https://dev.termx.org/api/fhir-swagger",
      "name": "termx-fhir"
    }
  ],
  "plugins": [
    "topbar"
  ]
}
```



## Running docker-compose.yml

Run docker-compose with `docker-compose up -d`

It's expected that only Postgres service will be up and running since the terminology server has no connection to the database (it's not created). Let's connect to it and create database\\users using the following command. Be sure to change admin and app user passwords according to **server.env** properties. 

```plaintext
docker exec -i termx-postgres psql -U postgres <<-EOSQL
CREATE ROLE tx_admin LOGIN PASSWORD 'test' NOSUPERUSER INHERIT NOCREATEDB CREATEROLE NOREPLICATION;
CREATE ROLE tx_app   LOGIN PASSWORD 'test' NOSUPERUSER INHERIT NOCREATEDB CREATEROLE NOREPLICATION;
CREATE ROLE tx_viewer NOLOGIN NOSUPERUSER INHERIT NOCREATEDB NOCREATEROLE NOREPLICATION;
CREATE DATABASE termx WITH OWNER = tx_admin ENCODING = 'UTF8' TABLESPACE = pg_default CONNECTION LIMIT = -1;
grant temp on database termx to tx_app;
grant connect on database termx to tx_app;
EOSQL
```

In case you are using an existing database, run SQL commands between **EOSQL** via sql console.

##  **Checking everything is ok**

After the DB is created, run `docker-compose restart` and check the application server logs via `docker logs -f termx-server`. There should not be any errors or java stack traces. If you see a log line similar to this

```plaintext
13:08:57.472 [main] INFO  io.micronaut.runtime.Micronaut - Startup completed in 7037ms. Server Running: http://2048db663c4b:8200
```

then the application is ready to receive requests from the browser.

## Serving the application

+++NGINX reverse proxy config example (for example, /etc/nginx/conf.d/default.conf_) 

```plaintext
server {
    server_name dev.termx.org;

    location / {
        add_header Content-Security-Policy "default-src 'self' 'unsafe-inline' 'unsafe-eval'; img-src *; style-src 'self' 'unsafe-inline'; connect-src sso.termx.dev tx.termx.org dev.termx.org" always;
        proxy_pass http://localhost:9000/;
    }

    location /api/ {
        client_max_body_size 900M;
        proxy_pass http://localhost:8200/;
    }

    location /swagger {
        proxy_pass http://localhost:8000;
    }
   
    location /chef/ {
        client_max_body_size 100M;
        proxy_pass http://localhost:8500/;
    }
    
    location /plantuml {
        proxy_pass http://localhost:8501;
    }

    location /fml-editor/ {
        proxy_pass http://localhost:8502/;
    }

    location /minio-console/ {
        rewrite   ^/minio-console/(.*) /$1 break;
        proxy_pass http://localhost:9101;
        proxy_set_header Host $http_host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-NginX-Proxy true;

        # This is necessary to pass the correct IP to be hashed
        real_ip_header X-Real-IP;

        proxy_connect_timeout 300;

        # To support websockets in MinIO versions released after January 2023
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";

        chunked_transfer_encoding off;
    }

    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/dev.termx.org/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/dev.termx.org/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
}

server {
    if ($host = dev.termx.org) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


    server_name dev.termx.org;
    listen 80;
    return 404; # managed by Certbot
}
```

`server_name` value is the application domain where the application is running.
+++

Please make sure you are using SSL. Please change **\dev.termx.org** domain configurations with your own domain. **80 → 443** redirect is a must.
 ***sso.termx.dev** in this example is an SSO server domain.* {.is-success}

SSL certificates are managed by [Certbot](https://certbot.eff.org/).


