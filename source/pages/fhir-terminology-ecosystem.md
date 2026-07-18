The **FHIR Terminology Ecosystem** answers a coordination question: *across the HL7 ecosystem, which terminology server is authoritative for a given code system, and where do I reach it?* TermX both **participates** in the ecosystem (it can describe the servers it knows about) and **consults** it (it can discover and resolve servers registered in the HL7 coordination catalog).

## Discovery and resolution

TermX exposes a lightweight, public proxy toward a FHIR terminology **coordination server** (the HL7 registry by default):

- `GET /tx-reg` — discovery: list the registered terminology servers.
- `GET /tx-reg/resolve?fhirVersion=R4&url=http://snomed.info/sct` — resolution: find the authoritative server(s) for a canonical URL and FHIR version.
- A no-login web UI at `/tx-ecosystem/` presents the same discovery/resolution over a browser page.

```s
curl "https://dev.termx.org/tx-reg/resolve?fhirVersion=R4&url=http://snomed.info/sct" | jq
```

The proxy is **stateless** (every request hits the coordination server) and **public** (no authentication). It is enabled by default and needs no configuration to run against the HL7 public server; if the coordination server is unreachable the API returns **HTTP 502**. Point it at a different registry with the `TERMINOLOGY_ECOSYSTEM_URL` environment variable (default `http://tx.fhir.org/tx-reg`).

## Describing your own servers

TermX can also publish an **ecosystem definition** that groups the terminology servers a deployment knows about, exposed as a machine-readable `ecosystem.json`:

- internal management API `/ecosystems` (requires Space privileges);
- public API `/public/ecosystems`, `/public/ecosystems/{code}`.

For each server, FHIR-ecosystem metadata can be recorded — `usage` (`code-generation` / `validation` / `publication`), `supportedOperations` (`$expand`, `$validate-code`, `$lookup`, `$translate`, `$subsumes`, `$closure`), `fhirVersions` (R3–R6), the `authoritative` URL patterns it owns, and `exclusions`. Only **active** servers are exported, and stored auth secrets are masked. The definition can be exported/imported at `GET /servers/export/ecosystem` and `POST /servers/import/ecosystem`.

## Relationship to HTX Router and Viewer

The TermX `tx-reg` proxy (coordination discovery) is complementary to the **HTX Router** and **HTX Viewer** (the gateway/browser layer that routes apps and users to a set of *configured* servers). The `tx-reg` proxy answers *"who is authoritative in the HL7 catalog?"*; the HTX layer answers *"how do our apps reach our configured servers?"*. Together they cover both the global-registry and the local-operations needs.
