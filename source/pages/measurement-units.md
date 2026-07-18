> This module was previously called **Measurement units**. It is now the **UCUM** module — the naming
> follows the [Unified Code for Units of Measure](https://ucum.org/) standard.
{.is-info}

TermX provides a full [UCUM](https://ucum.org/) (Unified Code for Units of Measure) service for validating, analysing and converting units of measure. It is used both interactively, from the **UCUM** menu, and by other resources (for example when an [observation definition](page:observation-definitions) declares the units it permits).

![UCUM menu](files/tutorial/ucum.png)

## Operations

The UCUM service exposes four operations:

- **Validate** — check that a unit string is a well-formed UCUM expression (for example `mg/dL`, `mm[Hg]`, `/min`).
- **Analyse** — break a unit down into its base dimensions and factors.
- **Convert** — convert a value from one unit to another compatible unit (for example `1 mg` → `0.001 g`).
- **Canonicalise** — reduce a unit to its canonical form so that two equivalent units can be compared.

## Catalog

The module also lets you browse the UCUM catalog loaded from the UCUM *essence* file:

- **Defined units** — named units such as `gram`, `litre`, `mole`.
- **Base units** — the seven SI base units the system is built on.
- **Prefixes** — SI prefixes such as `kilo`, `milli`, `micro`.

Each entry can be opened to inspect its code, symbol, definition and dimensions, and the catalog can be exported.

## FHIR and localisation

UCUM is exposed as a FHIR CodeSystem, including a `$translate` operation. National display names for units can be provided through a [CodeSystem supplement](page:code-system), so a deployment can show localised unit names without duplicating the UCUM catalog.
