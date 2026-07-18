TermX secures access in two layers: **authentication** (proving who a user is) and **authorization** (deciding what that user may do).

## Authentication

TermX does not manage users itself. It delegates authentication to an external OpenID Connect provider (such as Keycloak) and reads the user's roles from the SSO token. See [authentication](page:authentication) and the [Keycloak server](page:keycloak-server) page for setup.

## Authorization — attribute-based access control

Authorization is **attribute-based** (ABAC). Each role that arrives from the SSO server is matched to a TermX **privilege** with the same code, and a privilege grants a set of fine-grained permissions.

A permission is expressed as `resourceId.resourceType.action`:

- **resourceType** — for example `CodeSystem`, `ValueSet`, `MapSet`, `Wiki`, `ObservationDefinition`, or `Admin`.
- **resourceId** — a specific resource, or `*` for all resources of that type.
- **action** — one of `read`, `triage`, `write`, `maintain`.

For example `*.CodeSystem.write` allows writing to every code system, while `snomed-ct.CodeSystem.read` allows reading only the SNOMED CT code system. An `Admin` resource grants `*.*.*` — full access.

See [permissions](page:permissions) for how privileges are configured and mapped to user groups.

## Transport security

Deployments should terminate TLS in front of the application (see the reverse-proxy example in the [installation guide](page:installation-guide)); the `80 → 443` redirect is mandatory for any non-localhost installation.
