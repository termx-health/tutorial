TermX publishes an interactive **Swagger / OpenAPI** description of its REST API. It is generated from the server code, so it always matches the running version, and it is served without authentication.

## What is exposed

Two API surfaces are documented:

- **termx** — the native TermX API (terminology, wiki, modeler, observation definitions, …), available at `/api/swagger/termx.yml`.
- **termx-fhir** — the FHIR terminology API, available at `/api/fhir-swagger`.

The Swagger UI itself is served under `/swagger/` and can switch between the two definitions from its top bar.

## Deployment

In the reference deployment the Swagger UI runs as a separate `swaggerapi/swagger-ui` container and is proxied at `/swagger` (see the [installation guide](page:installation-guide)). It is configured with `swagger-config.json`, which lists the two definition URLs, and with `swagger.env` for the OAuth client used to authorize requests from the UI.

> The Swagger UI is an optional add-on. If you don't need it, you can omit the `termx-swagger` service; the API definitions are still available directly from the server.
{.is-info}
