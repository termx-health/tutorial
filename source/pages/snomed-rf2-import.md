Before pushing a SNOMED CT RF2 release into [Snowstorm](page:snowstorm), TermX lets you **dry-run scan** the release to see exactly what it changes, and check which TermX artefacts reference the concepts it inactivates. Once you're satisfied, the **same uploaded archive** is pushed to Snowstorm in one click — no second upload of the (~549 MB) file.

This complements the edition/branch management on the [SNOMED CT management](page:snomed-ct-management) page; be sure to review the Snowstorm version requirements there before importing.

## Privileges

- `snomed-ct.CodeSystem.read` — run a dry-run scan, view the result, run a concept-usage lookup.
- `snomed-ct.CodeSystem.write` — proceed with the import into Snowstorm.

## Dry-run scan

1. Open the SNOMED CT code system **edit** page and open the **Import from RF2** dialog.
2. Choose the RF2 zip, tick **Dry run**, and (optionally) **Full analysis** — see modes below. Click **Confirm**.
3. A three-phase progress bar shows *Uploading file…* → *Scanning RF2 file…* → (later) *Importing to Snowstorm…*. The scan runs in the background straight from the RF2 rows — it does **not** call Snowstorm, so even a full International edition scans in seconds.
4. You are routed to the **scan result** page. Both a **JSON and a Markdown** report download automatically, and three tables list the **NEW**, **MODIFIED** and **INVALIDATED** concepts with counts (for example 3,525 new / 1,133 modified / 576 invalidated), each with their designations.

### Scan modes

| Mode | Reads | Extra detail | Speed |
|---|---|---|---|
| **Summary** (default) | Concepts + descriptions + text definitions | — | ~3–4× faster |
| **Full** | + relationships + language refset | attributes on new concepts, acceptability on designations | baseline |

In summary mode, concepts whose only change was a relationship are not listed as *modified* (that change isn't visible from the parsed files alone).

## Check what will be affected

From the scan result, click **Check usage** to open the concept-usage lookup with the invalidated codes pre-filled (you can also paste plain codes, a JSON array, or a whole scan-result JSON). **Search** returns every TermX artefact still referencing those concepts — **CodeSystem supplements**, **ValueSet** rules, and stored **ValueSet expansions** — each linking to the affected resource so you can fix or re-expand it before the import lands.

## Proceed with the import

On the scan result page, click **Proceed with import**. The cached archive is submitted to Snowstorm without re-uploading; the dialog switches to *Importing to Snowstorm…* until the Snowstorm job completes. If Snowstorm rejects the import (for example an auth error), a clear notification is shown and the dialog closes.

## Notes

- **Uploaded archives are cached for 7 days** and then cleaned up automatically — long enough to scan, review and proceed.
- **Offline / CI use.** Standalone Python scripts (`snomed_rf2_scan.py`, `snomed_concept_usage.py`) under `termx-server/scripts/` mirror the in-app scan and usage lookup for batch or offline runs.
- **Size.** The International edition RF2 (~549 MB) is within the default 600 MB upload limit; raise it (and the reverse-proxy body limit) for larger archives — see the [configuration reference](page:configuration-reference).
- When SMTP is configured, SNOMED imports also send an [email notification](page:configuration-reference) on completion.
