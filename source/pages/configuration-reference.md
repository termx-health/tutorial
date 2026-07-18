This page is a consolidated reference of the environment variables used to configure TermX. The server reads them via Micronaut relaxed binding (`UPPER_SNAKE_CASE` → dotted property, so `SNOWSTORM_URL` → `snowstorm.url`). For a step-by-step install see the [installation guide](page:installation-guide); for a runnable local stack see the [quickstart](page:developer-quickstart).

> Values shown are defaults. **Never ship the development defaults to production** — in particular the `test` DB passwords, the `minio`/`minio123` (or `bob`/`bobobobo`) object-store credentials, `AUTH_MOCK_ENABLED=true`, and the guest/`dummy` OAuth mode.
{.is-warning}

## Server — `server.env`

### Database
| Variable | Default | Purpose |
|---|---|---|
| `DB_URL` | `jdbc:postgresql://localhost:5432/termx` | JDBC URL of the Postgres database |
| `DB_APP_PASSWORD` | `test` | Password of the runtime user `tx_app` |
| `DB_ADMIN_PASSWORD` | `test` | Password of the admin/Liquibase user `tx_admin` |
| `DB_POOL_SIZE` | `10` | Connection pool size |

### Authentication
| Variable | Default | Purpose |
|---|---|---|
| `OAUTH_JWKS_URL` | (none) | JWKS endpoint used to validate access tokens |
| `OAUTH_JWKS_CACHE_TTL_SECONDS` | `3600` | JWKS cache TTL |
| `GUEST_DISABLED` | `false` | Disable the anonymous Guest account |
| `AUTH_MOCK_ENABLED` | `false` | Dev-only mock auth (see [authentication](page:authentication)) |

### Snowstorm (SNOMED CT)
| Variable | Default | Purpose |
|---|---|---|
| `SNOWSTORM_URL` | `https://snowstorm.termx.org/` | Snowstorm base URL |
| `SNOWSTORM_USER` / `SNOWSTORM_PASSWORD` | `termserver-app` / (empty) | Basic-auth (leave empty for public read-only servers) |
| `SNOWSTORM_BRANCH` | `MAIN` | Default edition branch (e.g. `MAIN/SNOMEDCT-EE`) |
| `SNOWSTORM_NAMESPACE` | `1000265` | Namespace id for minting SCTIDs |

### Object storage — MinIO / Bob
| Variable | Default | Purpose |
|---|---|---|
| `BOB_MINIO_URL` | `http://localhost:9000` | S3/MinIO endpoint (`bob.minio.url`) |
| `BOB_MINIO_ACCESS_KEY` | `minio` | Access key |
| `BOB_MINIO_SECRET_KEY` | `minio123` | Secret key |

> `BOB_MINIO_*` and `MINIO_URL`/`MINIO_ACCESS_KEY`/`MINIO_SECRET_KEY` bind to the same `bob.minio.*` properties — **prefer `BOB_MINIO_*`** (it disambiguates the app credentials from the MinIO server's `MINIO_ROOT_USER`/`MINIO_ROOT_PASSWORD`). If the URL is unset, object storage is disabled and uploads return `503`. See the [MinIO service](page:minio-service).
{.is-info}

### Large imports
| Variable | Default | Purpose |
|---|---|---|
| `micronaut.server.max-request-size` | `629145600` (600 MB) | Max request size — raise for RF2/LOINC archives (also raise the reverse-proxy body limit) |
| `micronaut.server.multipart.max-file-size` | `629145600` (600 MB) | Per-file multipart cap |

The IHTSDO delta-generator tool is bundled in the image (no env var).

### Email (SMTP, optional)
`SMTP_ENABLED` (`false`), `SMTP_HOST`, `SMTP_PORT` (`587`), `SMTP_USERNAME`, `SMTP_PASSWORD`, `SMTP_FROM` (`noreply@termx.dev`), `SMTP_AUTH` (`true`), `SMTP_STARTTLS` (`true`), `SMTP_TO_IMPORT` (import-notification recipients). Check status at `GET /management/email/status`.

### FHIR ecosystem & search
| Variable | Default | Purpose |
|---|---|---|
| `TERMINOLOGY_ECOSYSTEM_URL` | `http://tx.fhir.org/tx-reg` | Coordination server for the [FHIR terminology ecosystem](page:fhir-terminology-ecosystem) |
| `TERMX_FHIR_CS_SEARCH_DEFAULT_SUMMARY` | `true` | Default `_summary` for `GET /fhir/CodeSystem` |
| `TERMX_FHIR_VS_SEARCH_DEFAULT_SUMMARY` | `true` | Default `_summary` for `GET /fhir/ValueSet` |
| `TERMX_FHIR_CM_SEARCH_DEFAULT_SUMMARY` | `true` | Default `_summary` for `GET /fhir/ConceptMap` |

### GitHub, public URLs & CORS
`GITHUB_APP_NAME` / `GITHUB_APP_ID` / `GITHUB_CLIENT_ID` / `GITHUB_CLIENT_SECRET` (see the [GitHub App](page:github-app)); `TERMX_WEB_URL`, `TERMX_API_URL`; `MICRONAUT_SERVER_CORS_ENABLED` (`false`), `MICRONAUT_SERVER_CORS_CONFIGURATIONS_UI_ALLOWED_ORIGINS`.

### JVM & logging
`JAVA_OPTS` (e.g. `-Xmx1800m`), `LOGBACK_LOG_LEVEL` (`INFO`). The image sets OOM fail-fast options that trigger a restart on out-of-memory — give the container a memory limit ≥ `-Xmx` plus headroom.

### 3.3+ optional integrations
| Variable | Purpose |
|---|---|
| `MS_DEVOPS_CLIENT_ID` / `MS_DEVOPS_CLIENT_SECRET` | Azure DevOps git sync (PAT-based; opt-in) |
| `TERMX_SERVER_SECRET_ENCRYPTION_KEY` | Encrypt stored external-server secrets at rest (AES/GCM; plaintext passthrough when unset) |
| `TERMX_CONFORMANCE_TX_URL` (default `http://localhost:8200/fhir`), `TERMX_CONFORMANCE_VALIDATOR_JAR` (baked in), `TERMX_CONFORMANCE_TEST_PACKAGE_DIR`, `TERMX_CONFORMANCE_SETUP_AUTH_TOKEN` | FHIR tx-ecosystem conformance runner (see the [capability statement](page:capability-statement)) |

## Frontend — `web.env`

| Variable | Default | Purpose |
|---|---|---|
| `TERMX_API` | `/api` | Base URL of the TermX server API |
| `BASE_HREF` | `/` | App base path (use `/termx/` on a sub-path) |
| `OAUTH_ISSUER` / `OAUTH_CLIENT_ID` | (required) | OIDC issuer and client (`dummy` enables guest mode) |
| `OAUTH_SCOPE` | `openid profile offline_access` | OAuth scopes |
| `SWAGGER_URL` / `CHEF_URL` / `CHEF_FHIR_VERSION` / `PLANT_UML_URL` / `FML_EDITOR` | `/swagger/`, `/chef`, `5.0.0`, `/plantuml`, `/fml-editor` | External tool URLs |
| `SNOWSTORM_URL` / `SNOWSTORM_DAILY_BUILD_URL` / `SNOMED_BROWSER_URL` | — | SNOMED expansion & browse links |
| `DEFAULT_LANGUAGE` / `UI_LANGUAGES` / `CONTENT_LANGUAGES` / `EXTRA_LANGUAGES` | `en` / `["en","et","lt","de","fr","nl","cs"]` / — / — | Language configuration (see [UI translations](page:ui-translations)) |
| `GUEST_DISABLED` / `EMBEDDED` / `SKIN` / `SKIN_URL` / `BRANDING` | — | Access, embedded mode and branding |

> JSON-typed web variables (`UI_LANGUAGES`, `EXTRA_LANGUAGES`, …) must be valid JSON. `env.js` is regenerated on container **restart** — no rebuild is needed to change these.
{.is-info}

## Database bootstrap — `pg.env`

`POSTGRES_USER` / `POSTGRES_PASSWORD` / `POSTGRES_DB` (all `postgres`) — the superuser used only to create the `termx` database and the `tx_admin` / `tx_app` / `tx_viewer` roles.
