# Nav Standardization + Review Page — What Changed

All files below are ready to drop into your local clone of
`roofwithveterans.com` (overwrite in place), then review in VS Code
before committing. Nothing has been pushed — you're in full control.

## 1. New: `_includes/nav.html`
The single source of truth for the site nav going forward. Contains
the desktop nav (with the **More ▾** dropdown), the mobile slide-down
menu, and the toggle JS — all in one file. **Edit nav links only here
from now on.** Added a **"Leave a Review"** link to both the desktop
dropdown and the mobile menu, pointing to `/review/`.

## 2. Changed: `_layouts/default.html`
Added one line — `{% include nav.html %}` — right after `<body>`.
Every page that uses `layout: default` now automatically gets the
exact same nav, forever. This is what stops future drift.

## 3. Changed: `assets/css/site.css`
Removed the old nav-specific CSS rules (the site had a *second*,
independent nav implementation living here — that's part of why nav
had drifted). Everything else in this file (buttons, page-hero,
sections, footer, etc.) is untouched.

## 4. Changed: 14 pages using `layout: default`
`index.html`, `careers.html`, `employee-portal.html`, `blog.html`,
`contact.html`, `gallery.html`, `operation-roof.html`, `resources.html`,
`roof-maintenance.html`, `roof-repairs.html`, `roof-replacement.html`,
`services.html`, `storm-damage.html`, `values.html`

Each had its own duplicate `<nav>` HTML, nav CSS, and toggle JS
removed — they now all pick up the shared nav from the include
automatically. Nothing else on these pages was touched.

**One bug fixed along the way:** in `employee-portal.html`, the old
per-page hamburger JS declared `const hamburger`/`const mobileMenu` at
the top level — if left in place alongside the include's own script,
clicking the hamburger would have toggled the menu open *and* closed
in the same click (double-toggle, looks like it does nothing). Removed
the duplicate; the include's script now handles it exclusively.

## 5. Changed: 5 standalone pages (no Jekyll front matter)
`meet-the-team.html`, `meet-wes.html`, `wes-military.html`,
`wes-personal.html`, `wes-professional.html`

These aren't processed by Jekyll's layout system, so they can't use
`{% include %}`. I pasted the same Pattern A nav markup directly into
each one instead (kept in sync by hand — if you change
`_includes/nav.html` later, these 5 need the same edit copied over
manually; I'll help with that whenever you update the nav).

**⚠️ Judgment call worth double-checking:** `wes-military.html`,
`wes-personal.html`, and `wes-professional.html` (the three bio
deep-dive "chapters") previously had an intentionally minimal nav —
just a logo and a "← Back to Wes Miller" link — since they're meant to
read like a focused narrative, not a browsing page. I replaced that
with the full site nav (Careers, Portal, More ▾, etc.) for
consistency, per "standardize everything." If that feels like too much
chrome on a page meant to be read start-to-finish, say the word and
I'll put the minimal version back on just those three.

## 6. Deleted: `operation-roof (1).html`
Confirmed byte-for-byte identical to `operation-roof.html` — a stray
duplicate upload, not a real second page. Removed.

## 7. New: `review.html`
The customer review-harvester tool (admin link/QR generator +
customer review flow), added with front matter:
```
layout: null
title: Leave a Review
permalink: /review/
```
`layout: null` is intentional — this page is a fully self-contained
document with its own `<html>/<head>/<body>`, so it deliberately does
**not** go through `_layouts/default.html` (which would double up the
HTML shell and the Gunny widget). It also doesn't carry the main site
nav, since customers reach it via a personal link or QR code, not by
browsing the site — that's by design, not an oversight.

**Still needs your input before this is fully live** — three CONFIG
values are marked `TODO` at the top of the file:
- `GOOGLE_PLACE_ID`
- `FACEBOOK_PAGE_URL`
- `BBB_PROFILE_URL`

Until those are filled in, the corresponding post-to buttons just
won't render on the customer view — nothing breaks, they're just
inactive.

## 8. One unrelated thing I noticed, not touched
`employee-portal.html`'s password gate uses `CORRECT_PASSWORD =
'Veterans'` in the live file — flagging in case that's not the
password you meant to have live (mismatched from what you'd told me
previously). Didn't change it since I wasn't asked to.

---

## How to bring this in
1. Copy every file in this package into your local clone at the same
   relative path (overwrite existing files).
2. Delete `operation-roof (1).html` from your local clone if it's
   still there.
3. Serve locally and spot-check: homepage nav (More ▾ opens, Reviews
   link present), mobile hamburger menu, `careers.html`,
   `employee-portal.html` (password gate + hamburger), one Pattern-B
   page like `services.html`, and one bio page like
   `wes-military.html`.
4. Fill in the three CONFIG values in `review.html` when you have them.
5. `git add -A && git commit -m "Standardize site nav via shared include; add review page" && git push`
