The **termx-fhir** definition describes the FHIR terminology API — the standard conformance surface of the TermX terminology server. It is published at `/api/fhir-swagger` and rendered here directly from that document, so it always matches the deployed server.

These endpoints are read-only FHIR operations and need no authentication, so the **Send** button on each operation works without signing in.

> The definition declares `https://demo.termx.org/api/fhir` as its server, which is not always reachable. Set the *server* field to a running deployment — for example `https://dev.termx.org/api/fhir` — before sending.
{.is-info}

## CodeSystem

{% openapi src="termx-fhir" tag="CodeSystem" %}

## ValueSet

{% openapi src="termx-fhir" tag="ValueSet" %}

## ConceptMap

{% openapi src="termx-fhir" tag="ConceptMap" %}

## StructureMap

{% openapi src="termx-fhir" tag="StructureMap" %}

## System level

{% openapi src="termx-fhir" tag="root" %}
