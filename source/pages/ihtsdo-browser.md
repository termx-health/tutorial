TermX treats **SNOMED CT** as an external terminology, stored and served by a [Snowstorm](page:snowstorm) server. To explore SNOMED CT content it ships its own graphical browser — the same tool described in more detail on the [SNOMED CT browser](page:snomed-ct-browser) page.

## Browsing SNOMED CT

The browser presents the state of the SNOMED CT branch currently selected by TermX. With it you can:

- browse the concept taxonomy;
- look up reference sets (refsets) and their members;
- execute [ECL](https://confluence.ihtsdotools.org/display/DOCECL) (Expression Constraint Language) expressions.

For each concept it shows the descriptions, relationships/attributes, and associations with other terminologies.

## Translations

The browser includes a translation widget for proposing new designations for a concept. Each proposed translation is tied to an installed language module and to the working (or daily-build) branch, and it creates a **concept review** [task](page:task-management). When the task is accepted, the translation is written back to the connected SNOMED branch. Language modules are configured through the `snomed-module` code system — see [SNOMED CT configuration](page:snomed-ct-configuration).

## Official browser

The community also maintains the official SNOMED International browser at [browser.ihtsdotools.org](https://browser.ihtsdotools.org), which can be used to cross-check content against the International Edition.
