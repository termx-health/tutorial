# TermX tutorial

Documentation for the TermX terminology, wiki and modelling platform, published
as a static site with [**mdbook**](https://github.com/helex-solutions/mdbook) and hosted
on GitHub Pages.

**Live site:** https://termx-health.github.io/tutorial/

## How it works

```
TermX Wiki  ──sync──▶  source/  ──mdbook (GitHub Action)──▶  GitHub Pages
```

1. Content is authored in **TermX Wiki** and synced to `source/` (the wiki-ssg export).
2. On every push to `main`, the workflow builds the site with mdbook and deploys it.

> **TermX sync:** in the TermX space's *GitHub integration*, set the **wiki-ssg folder
> location** to `source` (it defaults to `__source`) so the export lands in this folder.

## Structure

```
.
├── source/                   # TermX Wiki export — the single source of content
│   │                         #   (synced from TermX; do not hand-edit)
│   ├── space.json            # space metadata (name, web URL)
│   ├── pages.json            # page hierarchy
│   ├── pages/*.md            # page content
│   ├── attachments/          # images & files referenced as files/<id>/…
│   └── resources/            # embedded FHIR resources (StructureDefinitions, …)
│
├── .mdbook/
│   └── config.yml            # mdbook site config (skin, terminology server, …)
│
├── .github/workflows/
│   └── mdbook.yml            # build with mdbook + deploy to GitHub Pages
│
├── .gitignore
├── README.md
└── LICENSE
```

`source/` is the **only** content directory — everything the site needs (pages,
hierarchy, images, terminology resources) lives there. It is the authoritative
export from TermX Wiki, so edit content in TermX, not here.

## Local preview

```bash
npx github:helex-solutions/mdbook dev   --project .   # live-reload dev server
npx github:helex-solutions/mdbook build --project .   # build to .mdbook/dist
```

## Configuration

Site options live in [`.mdbook/config.yml`](.mdbook/config.yml) — the theme skin,
the FHIR `tx-server` used to expand `{{csc:}}`/`{{vsc:}}` concept tables, search,
and menu overrides. See the [mdbook docs](https://github.com/helex-solutions/mdbook) for
the full reference.
