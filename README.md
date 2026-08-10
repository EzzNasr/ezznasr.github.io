# ezznasr.dev — Integration Notes (Rev 2026.08)

This README covers how to drop `teaching.html`, `index.html`, and `resume.html`
into the live site as edits to what's already deployed. It assumes the site is
static files served from a root (e.g. GitHub Pages, Vercel, Netlify, or plain
hosting) — adjust the deploy step for whatever you're actually using.

## What's in this drop

| File | Type | Action |
|---|---|---|
| `teaching.html` | New page | Add at site root: `/teaching.html` |
| `index.html` | Existing page | **Replace** — only change is the nav gained a `Teaching` link |
| `resume.html` | Existing page | **Replace** — only change is the nav gained a `Teaching` link |

No CSS/JS files were split out — each page is self-contained (styles and
scripts inline), so there's nothing else to copy for these three files to
render correctly.

## 1. Diff before you overwrite

Since `index.html` and `resume.html` already exist live, don't blind-copy over
them if you've made other edits since the last version I saw. Diff first:

```bash
diff old/index.html new/index.html
diff old/resume.html new/resume.html
```

The only intended difference in both files is one line in the `<nav>` block:

```html
<a href="teaching.html">Teaching</a>
```

added between the last section link and `Resume`/`Contact`. If your diff shows
more than that, you've likely diverged from this version — merge by hand
rather than overwriting.

## 2. Where things must line up

`teaching.html` assumes the same file layout as the rest of the site:

- `index.html`, `resume.html`, `teaching.html` all live together at site root
  (so relative links like `index.html#about` and `resume.html` resolve).
- `/icons/*` (favicons, manifest) — already present if `index.html` works today.
- Google Fonts are loaded from `fonts.googleapis.com` at request time — no
  local font files to sync.

Nothing in this drop needs a build step. It's plain HTML/CSS/JS — copy the
files in and they work.

## 3. Sitemap

`sitemap.xml` already includes `teaching.html` (added in the last pass, dated
`2026-08-10`). If your live sitemap predates that, add:

```xml
<url>
  <loc>https://ezznasr.dev/teaching.html</loc>
  <lastmod>2026-08-10</lastmod>
  <changefreq>monthly</changefreq>
  <priority>0.8</priority>
</url>
```

Bump `<lastmod>` on the `index.html` and `resume.html` entries too, since both
changed (nav only, but a crawler doesn't know that).

## 4. Deploy

Pick whichever matches your actual setup:

**Git-based (GitHub Pages / Vercel / Netlify via repo):**
```bash
cp teaching.html index.html resume.html /path/to/repo/
cd /path/to/repo
git add teaching.html index.html resume.html sitemap.xml
git commit -m "Add teaching portfolio page, link it from nav"
git push
```

**Plain static host (rsync/SFTP/manual upload):**
```bash
rsync -av teaching.html index.html resume.html user@host:/var/www/ezznasr.dev/
```

## 5. Post-deploy checklist

Not automated — check by hand after it's live:

- [ ] `ezznasr.dev/teaching.html` loads, nav highlights "Teaching" as current
- [ ] `ezznasr.dev/` and `ezznasr.dev/resume.html` — nav now shows "Teaching"
      and clicking it lands on the new page
- [ ] `ezznasr.dev/teaching.html#programming`, `#math`, `#english` anchors
      scroll to the right section (test the exact link you'll send to
      موقع الخطة التعليمية / the Bakalorya application, likely `#programming`)
- [ ] Resubmit `sitemap.xml` in Google Search Console so the new page and
      updated nav get recrawled sooner rather than waiting for the next
      organic crawl

## 6. Outstanding TODOs left in the pages

These are marked inline in `teaching.html` with `<!-- TODO -->` comments —
listed here so nothing gets missed once you have real content:

- Programming: swap the video placeholder for the actual `<iframe>` embed
  once the Functions demo is recorded and uploaded
- Math: topic title, video embed, and the three stat numbers (students
  tutored / avg. score lift / sessions run)
- English: topic title, video embed, and the three stat numbers (students
  taught / countries / sessions run)
- Top-of-page lede: replace the placeholder line with a real aggregate stat
  once you have one (e.g. "Taught 40+ students across these subjects since
  2023")
