You can publish your TermX space as a static website (SSG) as part of the synchronization process with GitHub.
This manual describes configuring the build pipeline to generate the static site with **mdbook** — a generator that renders the TermX Wiki export the same way the wiki does and publishes it to GitHub Pages.

## TermX space and GitHub setup
1. Navigate to Spaces and select your desired space.
2. Go to Space configuration and set up "GitHub integration."
2.1. Please read more on the [GitHub](page:github) and [GitHub App](page:github-app) pages.
2.2. Insert your GitHub repository URL.
2.3. Set the `wiki-ssg` folder location to `source`.
2.4. Clear other folder locations (if they are not in use).
2.5. Save your settings.

On synchronization, TermX exports the space into the `source/` folder: `space.json`, `pages.json`, `pages/*.md`, `attachments/`, and the embedded FHIR `resources/`.

## Project configuration

mdbook is driven by a small `.mdbook/config.yml` file in the repository root:

+++ View *.mdbook/config.yml*
```yaml
site:
  title: TermX tutorial
  description: Guide to the TermX terminology, wiki and modelling platform.
  lang: en

source:
  format: termx            # TermX Wiki export
  meta: source             # space.json + pages.json + resources/ + attachments/
  pages: source/pages      # page markdown

# FHIR terminology server for expanding {{csc:}}/{{vsc:}} at build time.
tx-server: https://dev.termx.org/api/fhir

theme:
  skin: helex

search: true

# Optional site footer, shown on every page (inline HTML allowed).
footer:
  message: Guide to the TermX terminology, wiki & modelling platform
  copyright: © 2026 TermX
```
+++

## Setting up a GitHub repository

If you haven't set up a GitHub workflow, follow the steps below to create it.

Add a workflow that builds the site with the mdbook action and deploys it to GitHub Pages.

+++ View *.github/workflows/mdbook.yml* configuration file
```yaml
name: Publish site

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: true

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Build with mdbook
        id: mdbook
        uses: igorboss/mdbook@v1.1.1   # pin to a release tag for reproducible builds
        with:
          project: .

      - uses: actions/configure-pages@v5

      - uses: actions/upload-pages-artifact@v3
        with:
          path: ${{ steps.mdbook.outputs.site }}

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - id: deployment
        uses: actions/deploy-pages@v4
```
+++

Then enable GitHub Pages for the repository: open **Settings → Pages → Build and deployment** and set the **Source** to **GitHub Actions**.

## How to run

Open the Space view. You'll find an ellipsis icon in the top right corner of the Space's context bar (*...*). Hover over this icon, and a dropdown menu will appear. From this menu, select *"Sync with GitHub."*

You'll be redirected to a GitHub preview page where, after a short delay, the changes between TermX and GitHub should become visible. Confirm that you've selected all the necessary items for synchronization, then click the *"Push changes to..."* button.

Every push to `main` triggers the workflow, which builds the site and deploys it to GitHub Pages. Navigate to your repository's *"Actions"* section to follow the running or completed workflow; once the *deploy* job finishes, the live site URL is shown there and under **Settings → Pages**.

## About the static site generator

mdbook builds on [VitePress](https://vitepress.dev) and uses the same Markdown engine as the TermX Wiki, so pages render consistently between the wiki and the generated site. During the build it:

* Rewrites `files/<id>/…` attachment references so images and files resolve on the static site.
* Expands `{{csc:}}` / `{{vsc:}}` code system and value set concept tables from the configured FHIR terminology server (`tx-server`).
* Renders Mermaid diagrams.
* Prepares StructureDefinition blocks for display using the `@termx-health/structure-definition-viewer` component.
  * Transforms FSH code blocks into JSON.
  * Replaces the `{{def:sd-code}}` block with content from the corresponding resource under `source/resources/structure-definition`.
* Builds a full-text search index and a per-language sidebar.
* Renders [card grids](page:extended-markdown-syntax#card-grids) (`{.card-grid}`) — bullet lists become responsive cards with cover images, titles, descriptions and action buttons.
* Shows an optional site **footer** on every page, configured via `footer:` in `.mdbook/config.yml`.
* Auto-detects the site base path — `/<repo>/` for a GitHub project page, or `/` for a custom domain (CNAME) or an `<owner>.github.io` page.

The result is a searchable, themeable static site published straight to GitHub Pages — no artifacts to download or extra hosting step.

## Differences from the TermX Wiki

The static site uses the same Markdown engine as the wiki, so almost all syntax renders identically — headings, styling, tables (including `{.dense}` and multiline tables), blockquote admonitions (`{.is-info}` …), content tabs, collapsibles, task lists, footnotes, sub/superscript, emoji, the `{{def:}}` / `{{csc:}}` / `{{vsc:}}` includes, and Draw.io, PlantUML and Mermaid diagrams.

A few things to keep in mind when authoring for the static site:

* **Internal links** — use the wiki link namespaces, which the generator resolves: `page:<slug>` (or `page:<space>/<slug>`), `cs:`, `vs:`, `ms:`, `concept:`, or a full external URL. Avoid application routes such as `/wiki/<space>/<slug>` or `/spaces` — those exist only in the running wiki and will not resolve on the static site.
* **Terminology expansion** — `{{csc:}}` / `{{vsc:}}` tables are expanded at **build time** against the configured `tx-server`, so the referenced code system / value set must be reachable there when the site is built.

## Page metadata & SEO

The generator produces standard SEO metadata from the export, with no extra authoring:

* **Title** — each page's `<title>` is the page **name** (from `pages.json`) — e.g. *"Import | TermX tutorial"*.
* **Description** — a `<meta name="description">` is derived from the page's first paragraph.
* **Sitemap, canonical & Open Graph** — a `sitemap.xml`, `<link rel="canonical">` and Open Graph / Twitter tags are generated per page when the site URL is known. That URL is auto-detected in CI (`https://<owner>.github.io/<repo>/`, or a `CNAME` domain); override it with `site.url` in `.mdbook/config.yml`.

Site-wide metadata — title, description, language, theme, terminology server and search — lives in the `.mdbook/config.yml` file described under [Project configuration](#project-configuration).

*The source code can be found [here](https://github.com/igorboss/mdbook).*
