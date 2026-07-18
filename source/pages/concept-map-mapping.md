The mappings (associations) of a [concept map](page:concept-map) are edited within the **draft** version of a map set. Each association links one source concept to one target concept together with an equivalence relationship (for example *equivalent*, *wider*, *narrower* or *unmatched*).

## Creating associations

- **Manually** — pick a source concept and a target concept and choose the relationship.
- **In batches** — add or update many associations at once.
- **Automap** — TermX proposes candidate mappings automatically; you then review and confirm them.
- **Unmap** — mark source concepts that have no target.

Associations can be **verified** to flag inconsistencies before the version is activated.

## Scope and review

The version scope defines which source and target code systems (and versions) the mappings are drawn from. Within the version summary you can review the associations, the mapped concepts, and the property values that decorate them.

## Export

The full set of associations can be exported to **CSV** or **XLSX** for offline review or exchange. At runtime the mappings are served through the FHIR `$translate` operation.
