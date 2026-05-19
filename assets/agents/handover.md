# Portfolio Project — Handover Document

**Owner:** Alberto Alonso  
**Repo:** [a-alonso/portfolio](https://github.com/a-alonso/portfolio)  
**Live URL:** https://a-alonso.github.io/portfolio/  
**Date:** 2026-05-19  

---

## Project Overview

A personal portfolio website for Alberto Alonso — Innovation-driven Product Manager with 10+ years of global experience across fintech, insurtech, and legal tech. The site presents a curated set of professional case studies, a brief bio, and social links. It is a static HTML site hosted on GitHub Pages.

---

## Tech Stack

| Layer | Choice |
|-------|--------|
| Markup | Plain HTML5 |
| CSS framework | **Bulma v1.0.4** (CDN) |
| Icons | Font Awesome 4.6.3 (CDN) |
| Font | Source Sans Pro (Google Fonts) |
| Analytics | Mixpanel (initialised in `<head>`) |
| JS | Minimal — `controller.js` handles navbar burger toggle |
| Hosting | GitHub Pages off `master` |

---

## Repository Structure

```
portfolio/
├── index.html            # Main landing page (About + Work grid)
├── controller.js         # Navbar burger toggle logic
├── work_jpmc.html        # Case study: JP Morgan
├── work_repay_1.html     # Case study: REPAY Config Tool (admin-facing)
├── work_repay_2.html     # Case study: REPAY Direct Debit (consumer-facing)
├── work_rappi.html       # Case study: Rappi
├── work_hogaru.html      # Case study: Hogaru
├── work_nequi.html       # Case study: Nequi
├── work_endava.html      # Case study: Endava
├── assets/
│   ├── css/styles.css    # Custom styles on top of Bulma
│   └── img/             # All images (work_*.jpg/png, avatar, favicon)
├── .gitignore
├── LICENSE
└── README.md
```

---

## Pages & Navigation

### `index.html`
The single entry point. Contains:
- **Navbar** — Home link, Work dropdown (all 5 visible cases), LinkedIn + GitHub CTA buttons
- **Hero section** (`#about`) — Avatar, name, bio, credentials (LSE, Y Combinator, Python/SQL cert)
- **Work grid** (`#Work`) — 5 project cards in a 2-column Bulma grid, each linking to its case study page
- **Footer** — Copyright line

### Work Dropdown (navbar order)
1. JP Morgan services documentation → `work_jpmc.html`
2. Repay consumer-facing → `work_repay_2.html`
3. Repay admin-facing → `work_repay_1.html`
4. Rappi → `work_rappi.html`
5. Hogaru.com → `work_hogaru.html`

> **Note:** `work_nequi.html` and `work_endava.html` exist in the repo but are **not currently linked** from the navbar or the work grid on `index.html`.

---

## Bulma Implementation Notes

The project recently migrated to **Bulma v1.0.4**. Key changes made during that upgrade:

- Removed deprecated `is-bold` modifier from hero sections
- Fixed navbar burger: removed extra 4th `<span>` (Bulma v1 requires exactly 3)
- Changed `<a class="navbar-burger">` to `<button class="button navbar-burger">` for accessibility
- Equal-height cards implemented with `is-flex` on `.column` and `is-flex is-flex-direction-column is-fullheight` on `.card`, with `mt-auto` on the "Read more" line

---

## Active Branches

| Branch | Purpose |
|--------|---------|
| `master` | Production — live on GitHub Pages |
| `chore/session-handover` | Housekeeping PR with previous session notes (safe to merge/close) |

---

## Outstanding / Next Steps

### Immediate
- **Add `work_nequi.html` and `work_endava.html` to the navbar and work grid** — both pages exist and have content but are unreachable from the UI
- **Close or merge PR #8** (`chore/session-handover`) — it contains only a now-outdated incident note and can be merged and the branch deleted

### Planned Feature (was in progress)
- **Add PromptBunny / SmartRisk entry** — a new work case study for an AI-powered screening tool. Needs:
  1. A new `work_smartrisk.html` (or similar) case study page with text and media
  2. A navbar entry under the Work dropdown
  3. A card added to the work grid on `index.html`
  4. A card image added to `assets/img/`

---

## Key Asset Reference

| Image file | Used by |
|------------|---------|
| `assets/img/work_7.jpg` | JP Morgan card |
| `assets/img/work_5.jpg` | REPAY Config Tool card |
| `assets/img/work_6.jpg` | REPAY Direct Debit card |
| `assets/img/work_3.jpg` | Rappi card |
| `assets/img/work_1.png` | Hogaru card |
| `assets/img/avatar.png` | Hero bio section |
| `assets/img/favicon.png` | Browser tab icon |

