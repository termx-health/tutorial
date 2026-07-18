Authoring in TermX means creating and maintaining terminology resources — [code systems](page:code-system), [value sets](page:value-set), [concept maps](page:concept-map) and [observation definitions](page:observation-definitions) — in a controlled, versioned way.

## Versions and lifecycle

All terminology resources are **versioned**. A resource (for example a code system) owns one or more versions, and each version moves through a lifecycle:

- **draft** — the version is being edited. Rules, concepts and mappings can be changed freely.
- **active** — the version is released. Its content is frozen and it becomes the version other resources resolve to.
- **retired** — the version is superseded and kept for history.

You change status explicitly from the version summary. A version can also be **duplicated** to start the next revision from the previous content.

![Resource summary with versions](files/tutorial/code-system-summary.png)

## Why versioning matters

Versioning lets you change a resource without breaking consumers that depend on an earlier release. A [value set](page:value-set-rule-based-expansion) always expands against fixed code system versions, and a [concept map](page:concept-map-mapping) always maps between fixed versions, so results stay reproducible over time.

## Provenance and tasks

Every change is recorded as **provenance**, viewable from the resource summary. Editorial work can be coordinated through [tasks](page:task-management) — for example a review or approval request raised on a version — and access to create or modify resources is controlled by [privileges](page:permissions).

When a version is ready for release, it can be published to GitHub or exposed through the FHIR API — see [publisher](page:publisher).
