Every [code system](page:code-system) and code system version in TermX has a **FHIR definition** — the resource rendered as a standard FHIR `CodeSystem`. The FHIR definition component gives you direct access to this representation from the code system list and the version summary.

## Viewing and downloading

From the **FHIR** link on a code system version you can:

- open the FHIR `CodeSystem` as **JSON** and download it as `CS-{id}.json`;
- download the same definition as **FSH** (FHIR Shorthand);
- inspect the definition before publishing or exchanging it.

The JSON is produced on demand from TermX's internal model, so it always reflects the current state of the selected version.

## GitHub synchronization

When a space is connected to a [GitHub](page:github) repository, the FHIR definitions are written into dedicated folders so they can be version-controlled and consumed by other tools:

- `codesystem-fhir-json` — the FHIR JSON representation;
- `codesystem-fhir-fsh` — the FHIR Shorthand (FSH) representation.

The same pattern applies to value sets and concept maps, giving a complete FHIR + FSH export of the terminology in the repository.
