A **concept map** defines mappings between concepts in a source terminology and concepts in a target terminology. In TermX a concept map is stored as a **map set** — a versioned resource that holds a collection of associations (mappings), just like a [code system](page:code-system) holds concepts and a [value set](page:value-set) holds rules. It corresponds to the FHIR `ConceptMap` resource.

Concept maps answer questions such as *"which SNOMED CT concept corresponds to this local diagnosis code?"* or *"how does an old classification map to its replacement?"*. They are used at runtime through the FHIR `$translate` operation, and they can also be used to compute transitive closures with `$closure`.

## Structure

- A **map set** identifies the mapping, carries the canonical `url`, publisher and multilingual names, and owns one or more versions.
- A **map set version** fixes the source and target scope and holds the list of associations. Versions move through the draft → active → retired lifecycle (see [authoring](page:authoring)).
- An **association** links one source concept to one target concept with an equivalence relationship (for example *equivalent*, *wider*, *narrower*, *unmatched*).

## Working with concept maps

- [Concept map list](page:concept-map-list) — browse, search, add and delete concept maps.
- [Concept map summary](page:concept-map-summary) — the dashboard for a single concept map and its versions.
- [Concept map mapping](page:concept-map-mapping) — create and review the associations inside a version, including automap and export.
