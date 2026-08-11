# ezznasr.dev

Personal site — home page, resume, and teaching portfolio. Static HTML/CSS/JS,
no build step, no framework. Hosted on GitHub Pages via `CNAME` → `ezznasr.dev`.

## Skeleton

```
.
├── index.html              # home — hero, about, stack, work, coursework tools, history, contact
├── resume.html              # inline PDF viewer + download links
├── teaching.html             # teaching portfolio — programming / math / english
├── 404.html                  # not-found page
├── CNAME                      # ezznasr.dev
├── robots.txt                  # points crawlers at sitemap.xml
├── sitemap.xml                  # crawl targets, see below
├── llms.txt                      # plain-text identity/summary for LLM crawlers
├── icons/                         # favicons + site.webmanifest
├── images/
│   └── ezz-nasr.jpg                # portrait, used by og:image + JSON-LD across all pages
└── public/
    ├── resume/
    │   ├── Ezz_Nasr_Resume.pdf       # source of truth for resume.html's inline viewer
    │   └── Ezz_Nasr_Resume.docx       # downloadable source file
    └── assets/
        └── banner.svg                   # unused on-site; leftover from GitHub profile README, not linked anywhere here
```

Every page is self-contained — CSS and JS inline, no shared stylesheet or
bundle. Duplication across pages (header, footer, design tokens) is
intentional, not an oversight: keeps each page a single file to read top to
bottom.

## Pages

| Page | Path | Sections / anchors |
|---|---|---|
| Home | `/` | `#about` `#stack` `#work` `#tools` `#history` `#contact` |
| Resume | `/resume.html` | inline PDF viewer, PDF/.docx download |
| Teaching | `/teaching.html` | `#programming` `#math` `#english` |
| 404 | any unmatched path | GitHub Pages serves this automatically |

Nav is identical across `index.html` / `resume.html` / `teaching.html`
(About / Stack / Work / Teaching / Resume / Contact), with the current page
marked via `.current`. When adding a page, update nav in all three.

## SEO / discovery

- `sitemap.xml` — currently lists `/` and `/resume.html`. **`teaching.html`
  still needs to be added** (see TODO).
- `robots.txt` — allow-all, points to `sitemap.xml`.
- `llms.txt` — plain-text identity summary + project list, for LLM-based
  crawlers/answer engines rather than traditional search.
- Every page carries its own `<link rel="canonical">`, Open Graph / Twitter
  card tags, and a `Person` JSON-LD block (`sameAs`: GitHub + LinkedIn) —
  ties all pages back to the same entity for name search.
- `resume.html` also keeps a visually-hidden plain-text resume summary in
  the DOM (`.sr-resume-text`), since crawlers index HTML text far more
  reliably than an embedded PDF.

## Design system

Blueprint / technical-drawing aesthetic, shared across every page:

- Palette: deep green background (`--bg: #0B2318`), copper accent
  (`--copper: #C9974C` / `--copper-bright: #E3B168`)
- Type: IBM Plex Mono (labels, meta, UI) + Source Serif 4 (body copy)
- Motif: corner registration ticks (`.frame`), title-block metadata strip
  (`.tb-meta` — Dwg No. / Rev / Sheet / Status), BOM-style tables
- Motion: scroll-reveal via `IntersectionObserver`, hover lift on cards —
  all off under `prefers-reduced-motion`

New pages should reuse these tokens rather than introduce new ones —
`teaching.html` is the reference for how to extend the system into new
content without changing how it looks.

## Deploy

GitHub Pages, built straight from this repo (no Actions/build step) —
push to the default branch and it's live. `CNAME` handles the custom
domain.

## TODO

- [ ] Add `teaching.html` to `sitemap.xml`
- [ ] `images/ezz-nasr.jpg` — confirm it's the real photo, not a placeholder
- [ ] Teaching page: Functions video embed, Math + English samples and
      stats (tracked inline as `<!-- TODO -->` comments in `teaching.html`)
- [ ] Decide whether `public/assets/banner.svg` is still needed — currently
      dead weight, not referenced by any page
