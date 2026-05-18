# Portfolio Developer Skill — a-alonso/portfolio

A developer reference for building and maintaining pages in this portfolio. All decisions are grounded in the existing codebase. No new libraries, frameworks, or CSS patterns should be introduced unless explicitly agreed.

---

## Stack

| Layer | Technology | Version / Source |
|---|---|---|
| Markup | HTML5 | Static `.html` files |
| CSS framework | Bulma | `1.0.4` via jsDelivr CDN |
| Icons | Font Awesome | `4.6.3` via MaxCDN |
| Typography | Source Sans Pro | Google Fonts — weights 300, 400, 700 |
| Custom CSS | `assets/css/styles.css` | Repo-local |
| JavaScript | Vanilla ES5 | `controller.js` — navbar burger toggle only |
| Analytics | Mixpanel | `index.html` only — do not copy to work pages |

---

## File Structure

```
portfolio/
├── index.html               ← Home page (hero + work grid)
├── work_jpmc.html           ← Case study page
├── work_repay_1.html        ← Case study page
├── work_repay_2.html        ← Case study page
├── work_hogaru.html         ← Case study page
├── work_rappi.html          ← Case study page
├── work_endava.html         ← Case study page
├── work_nequi.html          ← Case study page
├── controller.js            ← Navbar burger toggle (shared across all pages)
└── assets/
    ├── css/styles.css       ← Custom overrides
    └── img/                 ← Project images (work_1.png … work_7.jpg, avatar.png, favicon.png)
```

Every page shares the same `<head>` block and `controller.js` reference. New pages must follow this exact pattern.

---

## Canonical `<head>` Block

Copy this verbatim for every new page. Replace only `<title>` and `<meta name="author">`.

```html
<head>
  <title>Work: [Project Name]</title>
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/font-awesome/4.6.3/css/font-awesome.min.css">
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bulma@1.0.4/css/bulma.min.css">
  <link rel="stylesheet" type="text/css" href="assets/css/styles.css">
  <link href="https://fonts.googleapis.com/css?family=Source+Sans+Pro:300,400,700" rel="stylesheet">
  <link rel="icon" type="image/x-icon" href="assets/img/favicon.png">
  <meta name="author" content="[Project Name]">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="theme-color" content="#E25D33">
</head>
```

**Do not** copy the Mixpanel snippet from `index.html` into work pages — it only belongs on the home page.

---

## JavaScript Rule

`controller.js` is the **only** JavaScript file. It does exactly one thing: toggle `is-active` on the `.navbar-burger` and its paired `.navbar-menu` when the burger is clicked.

```js
// controller.js — do not modify unless the navbar burger pattern changes
document.addEventListener('DOMContentLoaded', function () {
  var $navbarBurgers = Array.prototype.slice.call(
    document.querySelectorAll('.navbar-burger'), 0
  );
  if ($navbarBurgers.length > 0) {
    $navbarBurgers.forEach(function ($el) {
      $el.addEventListener('click', function () {
        var target = $el.dataset.target;
        var $target = document.getElementById(target);
        $el.classList.toggle('is-active');
        $target.classList.toggle('is-active');
      });
    });
  }
});
```

**Do not add new JS** for layout, animation, or styling. Bulma handles all visual states via CSS classes. Only introduce new JS if a Bulma interactive component (modal, dropdown) strictly requires a class toggle that cannot be achieved with CSS alone.

---

## Canonical Navbar

Every page uses this identical navbar. The only variation is which `navbar-item` links are relevant for that page's context. Do not alter structure, tag choices, or class names.

```html
<nav class="navbar is-transparent" role="navigation" aria-label="dropdown navigation">
  <div class="navbar-brand">
    <button class="button navbar-burger" data-target="navMenu">
      <span></span>
      <span></span>
      <span></span>
    </button>
  </div>

  <div class="navbar-menu" id="navMenu">
    <div class="navbar-start">
      <a class="navbar-item" href="https://a-alonso.github.io/portfolio/">
        Home
      </a>
      <div class="navbar-item has-dropdown is-hoverable">
        <a class="navbar-link" href="#about">Work</a>
        <div class="navbar-dropdown is-boxed">
          <a class="navbar-item" href="work_jpmc.html">JP Morgan services documentation</a>
          <hr class="navbar-divider">
          <a class="navbar-item" href="work_repay_2.html">Repay consumer-facing</a>
          <a class="navbar-item" href="work_repay_1.html">Repay admin-facing</a>
          <hr class="navbar-divider">
          <a class="navbar-item" href="work_hogaru.html">Hogaru.com</a>
          <hr class="navbar-divider">
          <a class="navbar-item" href="work_nequi.html">Nequi Neobank</a>
          <hr class="navbar-divider">
          <a class="navbar-item" href="work_rappi.html" target="_blank">Rappi</a>
          <hr class="navbar-divider">
          <a class="navbar-item" href="work_endava.html" target="_blank">Endava Ramp-Up Journal</a>
          <hr class="navbar-divider">
        </div>
      </div>
    </div>

    <div class="navbar-end">
      <div class="navbar-item">
        <div class="field is-grouped">
          <p class="control">
            <a class="button" href="https://www.linkedin.com/in/albertoalonsog/"
               target="_blank"
               data-social-network="Linkedin"
               data-social-target="https://www.linkedin.com/in/albertoalonsog/">
              <span class="icon"><i class="fa fa-linkedin"></i></span>
              <span>Linkedin</span>
            </a>
          </p>
          <p class="control">
            <a class="button" href="http://github.com/a-alonso" target="_blank">
              <span class="icon"><i class="fa fa-github"></i></span>
              <span>Github</span>
            </a>
          </p>
        </div>
      </div>
    </div>
  </div>
</nav>
```

**Rules:**
- `data-target="navMenu"` on the burger and `id="navMenu"` on the menu must always match — `controller.js` depends on this.
- Always `<button>` for the navbar-burger, never `<div>` or `<a>`.
- Always `target="_blank"` on external links (Rappi, Endava, LinkedIn, GitHub). Never `target:` — use `target=`.
- `<nav>` is the required tag for `.navbar`.

---

## Canonical Hero (Case Study Pages)

Used at the top of every `work_*.html` to display the project title. Replace title and subtitle only.

```html
<section class="hero is-medium is-link">
  <div class="hero-body">
    <div class="container">
      <h1 class="title">[Project Name]</h1>
      <h2 class="subtitle">[One-line description]</h2>
    </div>
  </div>
</section>
```

**Notes:**
- `is-bold` was used in earlier versions but is removed in Bulma v1.0. Do not add it.
- `is-medium` controls the hero height. Use `is-large` only if significantly more vertical space is needed.
- `is-link` is the standard colour for case study heroes in this portfolio.

---

## Work Card Pattern (index.html)

The canonical pattern for a project card in the home page work grid. Cards are equal-height within each row via `is-flex` on the column and `is-fullheight` on the card.

```html
<div class="column is-6 is-flex">
  <a href="work_[slug].html" target="_blank"
     class="is-flex is-flex-direction-column is-fullheight"
     style="width:100%">
    <div class="card is-flex is-flex-direction-column is-fullheight">
      <div class="card-image">
        <figure class="image is-350x150">
          <img src="assets/img/[image]" alt="[Alt text]">
        </figure>
      </div>
      <div class="card-content is-flex is-flex-direction-column is-flex-grow-1">
        <p class="title is-3">[Project Title]</p>
        <p class="subtitle is-6">[Short description]</p>
        <p class="mt-auto">
          <br>Read more
          <span class="icon">
            <i class="fa fa-long-arrow-right"></i>
          </span>
        </p>
      </div>
    </div>
  </a>
</div>
```

**Equal-height pattern explained:**
- `is-flex` on `.column` makes it a flex container
- `is-fullheight` + `is-flex is-flex-direction-column` on `.card` stretches it to fill the column
- `is-flex-grow-1` on `.card-content` pushes the footer area down
- `mt-auto` on the "Read more" paragraph pins it to the bottom

Cards are grouped in rows of two:

```html
<div class="columns is-desktop">
  <!-- card 1 -->
  <!-- card 2 -->
</div>
```

---

## Notification Blocks (Case Study Pages)

Used inside case study pages to highlight tools or methods used. Three per row is the established pattern.

```html
<div class="columns is-desktop is-vcentered">
  <div class="column is-flex">
    <article class="notification is-warning has-text-centered is-fullheight" style="width:100%">
      <p class="title">
        <span class="icon is-large">
          <i class="fa fa-[icon-name] fa-stack-1x"></i>
        </span>
      </p>
      <p class="subtitle has-text-centered">[Label]</p>
    </article>
  </div>
  <div class="column is-flex">
    <article class="notification is-info has-text-centered is-fullheight" style="width:100%">
      ...
    </article>
  </div>
  <div class="column is-flex">
    <article class="notification is-danger has-text-centered is-fullheight" style="width:100%">
      ...
    </article>
  </div>
</div>
```

**Do not use** `tile is-ancestor` / `tile is-parent` / `tile is-child` — the tile system is deprecated in Bulma v1.0. Always use `columns` / `column` instead.

Colour modifiers in use across the codebase: `is-warning`, `is-info`, `is-danger`, `is-primary`, `is-link`, `is-success`. Always include one — a bare `.notification` with no colour modifier is unstyled.

---

## Section and Content Pattern (Case Study Pages)

```html
<section id="[SectionId]" class="section section-2">
  <div class="container">
    <div class="columns is-desktop is-centered">
      <div class="column is-three-quarters is-narrow">
        <div class="has-text-left">
          <div class="content">

            <h2 class="title is-2">[Section Title]</h2>
            <p class="subtitle is-5">[Subtitle]</p>

            <p class="">
              <span class="subtitle is-5">The problem —</span>
              [Problem description]
            </p>
            <p class="">
              <span class="subtitle is-5">The solution —</span>
              [Solution description]
            </p>
            <p class="">
              <span class="subtitle is-5">The approach —</span>
              [Approach description]
            </p>

            <!-- Tools Used block (see Notification Blocks above) -->

            <p class="subtitle is-3">Insights<br></p>
            [Insights text]

            <!-- Embeds (InVision / Figma) -->

            <p class="subtitle is-3"><br>Deliverables<br></p>
            [Deliverables text]

          </div>
        </div>
      </div>
    </div>
  </div>
</section>
```

**Notes:**
- `section-2` is a custom class from `assets/css/styles.css` — keep it on all work sections.
- The `column is-three-quarters is-narrow` wrapper centres and constrains the reading width.
- Headings inside `.content` follow: `title is-2` for section title, `subtitle is-5` for subsection labels, `subtitle is-3` for major subheadings (Insights, Deliverables).

---

## Progress Bar Pattern

Used in `work_jpmc.html` to illustrate a stage-gate process.

```html
<div class="columns is-desktop is-centered">
  <div class="column is-centered is-half-desktop">
    <progress class="progress is-link" value="85" max="100">85%</progress>
    <p class="has-text-centered is-size-5 has-text-weight-light has-text-grey">
      <span class="icon-text">
        <span>DISCOVER</span>
        <span class="icon"><i class="fa fa-chevron-right"></i></span>
        <span>DEFINE</span>
        <span class="icon"><i class="fa fa-chevron-right"></i></span>
        <span>DEVELOP</span>
        <span class="icon"><i class="fa fa-chevron-right"></i></span>
        <span>DELIVER</span>
      </span>
    </p>
  </div>
</div>
```

---

## Embed Pattern (InVision / Figma)

Both embed types appear repeatedly in case study pages. Use these exact attributes.

**InVision:**
```html
<iframe src="https://[invision-share-url]"
        frameborder="0"
        width="100%"
        height="569"
        allowfullscreen="true"
        mozallowfullscreen="true"
        webkitallowfullscreen="true">
</iframe>
```

**Figma:**
```html
<iframe style="border: 1px solid rgba(0, 0, 0, 0.1);"
        width="100%"
        height="560"
        src="https://www.figma.com/embed?embed_host=share&url=[encoded-figma-url]"
        allowfullscreen>
</iframe>
```

---

## Media Block Pattern (index.html hero)

Used on `index.html` to display avatar + name + subtitle side by side.

```html
<article class="media">
  <figure class="media-left">
    <p class="image is-70x70">
      <img class="avatar" src="assets/img/avatar.png" alt="Alberto Alonso">
    </p>
  </figure>
  <div class="media-content">
    <h1 class="title">Alberto Alonso</h1>
    <h2 class="subtitle">Innovation Catalyst</h2>
  </div>
</article>
```

---

## Footer Pattern

Identical across all pages.

```html
<section class="section-4 has-text-centered container">
  <small>© Alberto Alonso Portfolio 2026 👨🏻‍💻</small>
</section>
```

`section-4` is a custom class. Do not replace it with a Bulma `footer` component — the visual output would differ.

---

## Script Reference

Every page closes with the same script tag before `</body>`.

```html
<script src="controller.js"></script>
```

No other scripts. No inline `<script>` blocks.

---

## Bulma v1.0 Constraints

These patterns are broken or removed in Bulma v1.0.4 — do not use them:

| Removed pattern | v1 replacement |
|---|---|
| `tile is-ancestor` / `is-parent` / `is-child` | `columns` / `column` |
| `is-bold` modifier on `.hero` | Remove — has no effect |
| `navbar is-transparent` + `is-bold` together | Use one or the other |

---

## Adding a New Case Study Page

1. Copy `work_repay_1.html` as the baseline — it is the most complete and correct reference page.
2. Replace `<head>` title and author meta.
3. Replace hero title and subtitle.
4. Replace section content (problem / solution / approach / tools / insights / deliverables).
5. Add the new page's `<a class="navbar-item">` entry to the navbar dropdown in **every** existing page.
6. Add a new card block to `index.html` in the appropriate row.
7. Run the Bulma linter (`bulma_lint.py`) before raising a PR.
