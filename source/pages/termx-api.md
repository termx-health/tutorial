The **termx** definition describes the native TermX API — terminology, wiki, modeler, task management and administration. It is published at `/api/swagger/termx.yml` and rendered here directly from that document, so it always matches the deployed server.

Unlike the FHIR surface, most of these endpoints are protected: the definition declares an OAuth2 flow against `sso.termx.org`. No client is configured for this site, so the console sends unauthenticated requests and protected endpoints answer `401`. Use it to explore shapes and parameters rather than to change data.

> The definition declares no `servers`, so the *server* field starts empty — enter your own deployment (for example `https://dev.termx.org/api`) before sending.
{.is-info}

The definition is large (313 operations). The sections below cover the self-contained areas; the high-volume areas — Terminology (123), SNOMED (55), Modeler (45) and Wiki (30) — are best browsed in the [Swagger UI](https://dev.termx.org/swagger/?urls.primaryName=termx), or embedded per tag on a page of their own.

## Observation definitions

{% openapi src="termx" tag="Observation definition" %}

## Task management

{% openapi src="termx" tag="Task management" %}

## Spaces

{% openapi src="termx" tag="Space" %}

## UCUM

{% openapi src="termx" tag="UCUM" %}

## Editions

{% openapi src="termx" tag="Editions" %}
