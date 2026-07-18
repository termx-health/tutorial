TermX imports a [LOINC](page:loinc) release directly from the release zip published by Regenstrief. You **upload the zip once**; it is stored in [object storage](page:minio-service) and can be re-imported later — to retry a failed run, correct a single file, or keep several releases side by side — **without uploading the ~100 MB file again**.

A single LOINC release populates **three independent code systems** so consumers can subscribe to and version each separately:

- `loinc` — the core LOINC concept set;
- `loinc-part` — LOINC Parts;
- `loinc-answer-list` — LOINC answer lists.

## Privileges

- `loinc.CodeSystem.read` — open the import page and list stored archives.
- `loinc.CodeSystem.write` — upload or delete an archive.
- `*.CodeSystem.write` — fetch the file mapping and trigger the import.

See [security](page:security) for how privileges are configured.

## Importing a release

1. Open the **LOINC import** page and choose the translation **language** you want to load.
2. In the *Stored archives* card, choose the downloaded release zip (`Loinc_<version>.zip`) and click **Upload**. The release **version is detected automatically** from the filename (with a fallback that reads the version from the archive's difference report), and the zip streams straight to object storage tagged with its version and language.
3. The version selector refreshes and selects the new release. TermX reads the archive and **pre-selects the eight CSV slots** it needs (Parts, terminology, supplementary properties, panels and forms, answer list, answer-list link, order–observation, and the language-specific *LinguisticVariant* translation file).
4. Review the slot mapping. You can override any slot with a different file from the same zip, or leave it on the server default.
5. Click **Import**. Parsing and persistence run as a **background job**; when it finishes, the success notification links to the imported code system.

## Notes

- **Re-import is safe.** Importing again upserts concepts by code, so retries and re-runs don't create duplicates. To retry a failed import, just pick the version and click **Import** again — no re-upload.
- **Multiple releases coexist.** Upload 2.79, 2.81, 2.82… each is kept and listed; pick any version to import it. Archives are retained until you delete them from the *Stored archives* card.
- **Size.** LOINC release zips (~100 MB) are well within the default 600 MB upload limit (see the [configuration reference](page:configuration-reference)).
- **Only the selected language** translation file is ingested; other language files inside the same zip are listed but not loaded.
- A full release (e.g. 2.82) is large — on the order of 100 k concepts and over a million property values — so a first import typically takes several minutes, dominated by database writes.
