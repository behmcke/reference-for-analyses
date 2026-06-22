# Refactor Plan: reference-for-analyses → Quarto cookbook

Status: **base conversion (steps 0–5) and the discoverability & readability pass
are both done (2026-06-22), uncommitted; publish + cleanup (steps 6–7) pending.**
The enhancement pass as actually built — including where it diverged from the
original sketch — is recorded in *Discoverability & readability enhancements*
below.

## Goal

Convert this flat collection of R Markdown class files into a navigable,
searchable **Quarto website** published to GitHub Pages — without altering any
of the existing content (code, prose, or statistical answers move verbatim).

## Decisions locked

| Question | Decision |
|---|---|
| Scope of display refactor | **Full Quarto site** (top-navbar course dropdowns + built-in search; no left sidebar) |
| Site title | **"Betsy's Stats Cookbook"**; an *Organized using Claude Code* credit on the homepage and the data-provenance page |
| Rendered HTML in git | **Stop committing renders** — render to `_site/` (gitignored); publish with `quarto publish gh-pages` |
| Datasets | **Vendor a `data/` folder** so chunks are reproducible, with a `data/README.md` citing sources |
| Class tagging granularity | **Page per task** — each task is its own `.qmd`, tagged with its course **and** with cross-cutting concept tags (see *Discoverability & readability enhancements*) |
| Concept tags | **Add a shared concept-tag vocabulary** spanning courses; **drop the per-page singleton task-name tag** (it filtered to one page and added no value) |
| Per-page descriptions | **One-line `description:` on every page**, surfaced in the index listing and search index |
| Lead sentences | **Allowed (non-verbatim), but always inside a colored Quarto callout titled "Introduction"** so they're visually distinct from verbatim content (see Lead-sentence policy) |
| Navigation | **Courses are top-navbar dropdowns** (alphabetical), each listing its tasks in **workflow order** (fit → diagnose → select → special cases); the left sidebar was dropped. Homepage listing is a **table** (avoids the default layout's thumbnail indent). |
| 5 missing Regression datasets | **Fetch from a public Kutner ALSM mirror** into `data/` (mirror verified at execution; `eval: false` fallback) |
| Chunk execution on render | **`freeze: auto` site-wide; Time Series pages `eval: false`** (see Execution policy) |
| Model running this work | Opus 4.8, high/xhigh effort |

## Target structure

```
reference-for-analyses/
├── _quarto.yml                    # site config: navbar, sidebar, search, listing
├── index.qmd                      # landing page = filterable task listing + README intro
├── tasks/
│   ├── regression/                # 14 tasks
│   ├── experiments/               # 13 tasks
│   ├── time-series/               # promoted to real chunks (~6 tasks)
│   ├── multivariate/              # 3 tasks
│   └── statistical-computing/     # 14 tasks
├── data/                          # vendored datasets + README.md provenance, paths repointed to ../../data/
├── _site/                         # rendered site (gitignored; published via `quarto publish gh-pages`)
└── .gitignore                     # _site/, .Rproj.user, .quarto, *_cache, _freeze/
```

The original `*.Rmd` files are **kept in place until the rendered site is
verified** (end of step 7), then removed in a final commit — git history
retains them.

Each task `.qmd` carries front matter like:

```yaml
---
title: "Added Variable Plot"
description: "Isolate one predictor's contribution to a fitted model."
categories: [Regression, Diagnostics, Visualization]
---
```

`categories` now combines the **course** with one or more **concept tags** from
a controlled vocabulary (no more singleton task-name tags). Quarto renders these
as clickable filter buttons and auto-builds the `index.qmd` listing, so the same
content is genuinely browsable by course *and* by concept, fully searchable, and
every task individually linkable. The `description` shows in the listing and
feeds the search index, so a reader can tell pages apart without opening them.

## Content inventory (source → target)

| Source file | Tasks | Target folder |
|---|---|---|
| Regression Code.Rmd | 14 chunks | `tasks/regression/` |
| Experiments Code.Rmd | 13 chunks | `tasks/experiments/` |
| Statistical Computing Code.Rmd | 14 chunks | `tasks/statistical-computing/` |
| Multivariate Code.Rmd | 3 chunks | `tasks/multivariate/` |
| Time Series Code.Rmd | raw R, 0 chunks | `tasks/time-series/` (promote to real chunks) |

The three committed `.nb.html` renders (Regression, Experiments, Multivariate;
~2.5 MB total) are removed from the tracked tree via `git rm --cached` — history
keeps them.

## Execution policy

Quarto executes R chunks on render, so the plan needs an explicit policy:

- **Site-wide:** `freeze: auto` — chunks re-execute only when their source
  changes, so one broken page doesn't force a full re-run.
- **Time Series:** all pages get `eval: false` (display-only) with a short note.
  The promoted code has genuine bugs as written — `nrow()` called with no
  argument, undefined `table`, `x`, `x1`, `x3`, `confidence.level` — and fixing
  code logic is out of scope. The pages still render, searchable and tagged;
  they just don't execute.
- **Experiments — one allowed mechanical deviation:** the source file opens with
  a `Load Packages` chunk (`pacman::p_load(dplyr, ggplot2, lsmeans, pwr)`) that
  all 13 task chunks depend on. Since each task page renders in isolation, this
  setup chunk is **replicated at the top of every Experiments task page**
  (analogous to the already-allowed data-path repointing — no logic changes).
- **Missing-data fallback:** any task whose dataset can't be sourced (see Data
  vendoring) ships with `eval: false` and a "dataset unavailable" note rather
  than blocking the build.

## Data vendoring

**Fetchable from live URLs (download into `data/`):**
- Experiments — 6 files from `deanvossdraguljic.ietsandbox.net`
  (trout, reaction.time, bicycle, water.boiling, paper.towel.strength, weld.strength)
- Statistical Computing — 4 from `stat.osu.edu/~vqv/6730/data/`
  (fev.RData, lab_05.R, lab_05.RData, fish.txt)

**Found on local disk (copy into `data/`):**
- Multivariate — T1-8.DAT, T4-6.DAT
- Regression — 9 of 14: gpa, CH03PR03, CH03PR15, CH03PR17, CH06PR15,
  CH09PR10-1, CH14PR14 (and the two reused refs)

**Missing locally — fetch from public Kutner ALSM mirror:**
- CH01PR19-1, CH08PR16, CH06PR18-1, CH09PR22-1, CH14PR07
- A specific mirror URL must be identified and verified during step 2 (the ALSM
  data is widely mirrored on university course sites and in CRAN packages). If
  no working mirror is found, those 5 task pages ship `eval: false` with a
  "dataset unavailable" note instead of blocking the conversion.

**Provenance — `data/README.md`:** lists, per dataset, the textbook citation
(Kutner et al., *Applied Linear Statistical Models*; Dean, Voss & Draguljic,
*Design and Analysis of Experiments*; OSU Stat 6730 course materials), the
original URL, and the retrieval date. Raw numeric data is generally
uncopyrightable fact and these sets are already widely mirrored, so vendoring
plus citation is the standard, good-faith practice.

All `read.table(...)` / `load(url(...))` paths get repointed to `data/`
(relative `../../data/` from a task page). No code logic changes — only the path
string.

## Execution order

0. **Install Quarto** — `quarto --version` currently fails; install before
   anything that renders (steps 1–5 only build source files and don't need it).
1. **Foundation** — add `.gitignore`; `git rm --cached` the three `.nb.html`.
2. **Data** — create `data/`; download the 10 URL files, copy the 11 found
   local files, identify + verify a Kutner ALSM mirror and fetch the 5 missing;
   write `data/README.md` with per-dataset provenance.
3. **Scaffold** — `_quarto.yml` (navbar/sidebar/search/listing/`freeze: auto`)
   + `index.qmd`.
4. **Proof of concept** — convert **Regression** fully: split into 14 per-task
   `.qmd`, move code + prose verbatim, repoint data paths, stamp `categories`.
   **Checkpoint: you review the first converted file before I roll across the rest.**
5. **Remaining courses** — convert Experiments (setup chunk replicated per
   page), Statistical Computing, Multivariate, and promote Time Series into
   real chunks (`eval: false` per the Execution policy).
6. **Render + publish** — render locally to confirm, then `quarto publish
   gh-pages` (pushes the rendered site to a `gh-pages` branch; `main` stays
   render-free, no CI needed).
7. **Cleanup** — once the published site is verified, remove the original
   `*.Rmd` files in a final commit (history retains them). Also remove the
   `statistical-computing/` source-of-truth directory the user dropped into the
   project root (used to resolve 6730 compilation gaps; not part of the site).
   Note: the external source-of-truth repos `behmcke/regression-6450` and
   `behmcke/experiments-6410` live outside this project and are NOT deleted.

## Things to verify on your end

- ~~Quarto installed?~~ **Verified 2026-06-11: not installed** — added as
  step 0 of the execution order.
- R + needed packages (`pacman`, `dplyr`, `ggplot2`, `car`, `purrr`, etc.) if
  you want chunks to actually execute on render.
- ~~Whether the OSU URLs still resolve~~ **Verified 2026-06-11: they do**, via
  a 301 redirect to `asc.ohio-state.edu/vu.104/...` — a personal page, so
  fragile; vendor now while they're up.
- Comfort with the page-per-task split (~50 small files) vs. your current 5.

## Answer-block handling (approved 2026-06-11)

The source files bury each task's written answers/interpretation in a
`# ///// Answers /////` comment block inside the code chunk. **Approved
deviation:** lift these into the page body as a `## Answers` section with `###`
sub-labels, and **normalize math notation only** — bare notation like
`H_0: B_1=0` becomes `$H_0: \beta_1 = 0$` so it typesets via MathJax. Prose
wording (including any source typos) stays verbatim; only math symbols are
formatted. This makes the answers readable instead of inert grey comments.

## Discoverability & readability enhancements (revised)

**Status: done 2026-06-22 (uncommitted).** Applied as a pass over all 51 task
pages plus the site config. None of it touches statistical answers, values, or
code logic. Subsections below describe what was built; #5–#6 note where the
result diverged from the original sketch.

### 1. Concept tags (controlled vocabulary)

Every page keeps its single course tag and gains one or more **concept tags**
drawn from a fixed vocabulary. The old per-page task-name tag is dropped (it
only ever matched one page). Final vocabulary (9 tags; each aggregates ≥2 pages,
*Model Selection* the smallest at 3):

- **Model Fitting** · **Diagnostics** · **Model Selection**
  · **Inference & Prediction** · **Visualization** · **Resampling & Simulation**
  · **Data Wrangling** · **Experimental Design** · **Transformations**

This is what finally delivers the "browsable by task, not just by course"
promise: e.g. *Diagnostics* collects added-variable plots, assumption checks,
outlier analysis, and ACF/PACF from across four different courses. Time-series
work folds into the shared vocabulary rather than getting its own bucket:
detrending and deseasoning are **Transformations** (transforming a series toward
stationarity), forecasting is **Inference & Prediction**, ARIMA fitting is
**Model Fitting**, and ACF/PACF is **Diagnostics**.

### 2. Per-page descriptions

Each page gets a one-line `description:` in its front matter. Update the
`index.qmd` listing to surface it: `fields: [title, description, categories]`.
This makes the landing listing scannable and enriches the search index — a
reader can distinguish "Manual Smoothing" from "Smoothing with `geom_smooth()`"
without clicking.

### 3. Lead-sentence policy (the one new-prose exception)

Each task page may open with a **1–2 sentence "what / when to use this" lead**.
This prose is **non-verbatim and authored fresh**, so to keep it unmistakably
separate from the original content it **must live inside a colored Quarto
callout** — never as plain body text. Use a single, consistent callout type
site-wide:

```markdown
::: {.callout-note title="Introduction"}
Use an added-variable plot to judge whether one predictor still contributes
once the others are already in the model.
:::
```

Built as a blue `callout-note` titled **"Introduction"**, uniform across all 51
pages so the lead box reads as a recognizable convention. Everything below the
callout (code, results, interpretation, answers) stays verbatim per the rules
below.

### 4. Disambiguate duplicate titles

Several titles repeat across courses ("Check Assumptions", "Interaction Terms",
"Transformations"). The `description` and concept tags disambiguate them in
search and listings; no title rename required.

### 5. Navigation, cross-links & provenance

- **Navigation moved from a left sidebar to top-navbar course dropdowns**
  (alphabetical courses; tasks within each in workflow order fit → diagnose →
  select → special cases). The left sidebar was removed so pages render
  full-width. *(Diverges from the original "navbar + sidebar" decision.)*
- The homepage listing was switched from `type: default` to **`type: table`**:
  the default layout reserved an empty image gutter that showed as a large left
  indent.
- **"See also" footers** added to all 51 pages, linking related tasks — including
  cross-course links (the three *Check Assumptions* pages and the two
  *Transformations* pages point at each other).
- **Dataset → provenance links** added to the 36 data-using pages.
  `data/README.md` is now a **rendered site page** (`data/README.html`, added to
  the render list) with explicit per-course anchors (`#regression`,
  `#experiments`, `#multivariate`, `#time-series`, `#statistical-computing`);
  each page links its dataset(s) to the matching section.

### 6. Site identity

- Site renamed to **"Betsy's Stats Cookbook"** (navbar + homepage title).
- An *Organized using Claude Code* credit line sits on the homepage (after the
  intro paragraphs) and on the data-provenance page.

## Explicitly out of scope

- No edits to statistical prose, answers, code logic, or chunk content. Two
  approved exceptions, both additive and clearly delimited: (a) math-notation
  normalization in answer blocks (formatting only — no wording or value
  changes); (b) the authored lead sentence, which is confined to a colored
  callout per the Lead-sentence policy above.
- No renaming of variables or restructuring of analyses.
- This is a *display/structure* refactor only.
```
