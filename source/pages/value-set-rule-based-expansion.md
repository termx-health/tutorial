A [value set](page:value-set) version does not store its concepts directly — it stores a **rule-based definition** (the FHIR *compose*) that is **expanded** into the final list of concepts. This keeps a value set in sync with its source code systems and lets a single rule stand in for thousands of concepts.

## Rules

You define rules within the **draft** version of a value set. The definition consists of **include** and **exclude** rules that work additively (like `UNION` in SQL): everything the include rules add, minus everything the exclude rules remove.

Every rule is based on an exact code system and code system version. A rule can select concepts in three ways:

- **Enumerated concepts** — pick specific concepts by code.
- **Filters** — select concepts by a `property`, an `operator` and a `value` (for example *is-a* a SNOMED CT concept, or a property equals a value). Several FHIR filter operators are supported; for SNOMED CT the hierarchy operators are translated into ECL.
- **Referenced value sets** — include the concepts of another value set.

If a rule specifies no limitations, all concepts of the code system are included.

The version summary shows the rule and its resolved expansion — for example a `body-site` value set whose single rule includes *all SNOMED CT concepts where concept is-a 442083009*:

![Value set version with a rule-based definition](files/tutorial/value-set-summary.png)

> Use the "eye" icon next to a rule to **preview** the concepts it selects — including SNOMED CT rules — before you commit the version.
{.is-info}

## Expansion

When the version is activated, the rules are expanded and the result is stored as a snapshot. The same definition is also available at runtime through the FHIR `$expand` operation. Expansion is strict per version: a value set version always expands against fixed code system versions, so the result is reproducible.
