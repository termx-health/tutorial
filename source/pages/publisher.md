Once a terminology resource has been [authored](page:authoring) and a version is **active**, it can be **published** — made available to consumers inside and outside TermX.

## Ways to publish

- **FHIR API** — every active resource is served through TermX's FHIR terminology endpoints (`CodeSystem`, `ValueSet`, `ConceptMap`) and operations such as `$expand`, `$validate-code`, `$lookup` and `$translate`. This is the primary way other systems consume TermX terminology at runtime. See the [capability statement](page:capability-statement).
- **GitHub** — a space can be synchronized with a [GitHub](page:github) repository, writing FHIR JSON and FSH representations of resources (see [code system FHIR definition](page:code-system-fhir-definition-component)) so they are version-controlled and reviewable. Publishing uses a [GitHub App](page:github-app).
- **Azure DevOps** — since release 3.3, TermX can synchronize content with Azure DevOps git repositories in the same way.
- **Implementation Guides** — resources can be assembled into a FHIR Implementation Guide for formal publication.
- **Files** — resources can be exported as FHIR/FSH, CSV or XLSX for offline exchange.

## Static site

Wiki content authored alongside the terminology can be published as a static site (see [static site generation](page:static-site-generation)) or exported to PDF/HTML, keeping documentation and terminology released together.
