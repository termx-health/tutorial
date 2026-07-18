An **observation definition** describes a template for a clinical or laboratory observation — what is measured, in which units, how results are interpreted, and how the observation maps to code systems. It corresponds to the FHIR `ObservationDefinition` resource.

Like other TermX resources, an observation definition is identified by a `code`, has a canonical `url`, a `publisher` and multilingual names, and moves through the draft → active → retired lifecycle described in [authoring](page:authoring).

![Observation definitions](files/tutorial/observation-definitions.png)

## What an observation definition captures

- **Value** — the permitted result: a data type, quantitative units (validated against [UCUM](page:measurement-units)), and permitted coded values.
- **Qualified intervals** — reference ranges and their interpretation (for example normal / high / low, optionally qualified by age, sex or condition).
- **Components** — for panel observations, the individual measurements that make up the panel.
- **Members** — related observation definitions grouped together.
- **Protocol** — the method or procedure used to obtain the observation.
- **Mappings** — links from the observation definition to concepts in code systems such as LOINC or SNOMED CT.

## Managing observation definitions

The **Observation definitions** menu lists all definitions and lets you add, edit, view and import them. The editor is organised into tabs — *value*, *member*, *component*, *interpretation*, *protocol* and *mapping* — so each aspect can be maintained independently. Editing requires the `ObservationDefinition` write privilege (see [security](page:security)).
