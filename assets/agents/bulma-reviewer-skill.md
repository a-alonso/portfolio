# Bulma Front-End Code Reviewer — Agent Skill

A skill definition for an AI agent that reviews HTML files for correct, consistent, and idiomatic Bulma CSS implementation. The agent checks not just that the Bulma stylesheet is linked, but that every component's class hierarchy, required children, valid modifiers, and nesting rules are correctly followed.

---

## Scope

This skill covers:

- Bulma CDN version detection and compatibility gating
- Component structural validation (required/optional children per component)
- Modifier class validity per component
- Illegal or deprecated patterns (including v1 breaking changes)
- Cross-component consistency (e.g. columns/column pairing)
- Equal-height card normalisation patterns
- Output as annotated violations with line numbers and remediation suggestions

---

## Knowledge Base Schema

Each Bulma component is encoded as a JSON rule object. The agent loads this schema before analysing any file.

```json
{
  "columns": {
    "root": { "required": ["columns"], "tags": ["div"] },
    "modifiers": ["is-mobile", "is-desktop", "is-multiline", "is-centered",
                  "is-vcentered", "is-gapless", "is-variable"],
    "children": {
      "column": { "min": 1, "modifiers": ["is-narrow", "is-half", "is-one-third",
                  "is-two-thirds", "is-one-quarter", "is-three-quarters",
                  "is-full", "is-half-desktop", "is-one-third-desktop",
                  "is-flex"] }
    },
    "rules": [
      "Direct child of .columns must be .column — no other elements.",
      "Do not nest .columns inside .columns without an intermediate .column."
    ]
  },
  "card": {
    "root": { "required": ["card"], "tags": ["div", "article", "a"] },
    "modifiers": [],
    "children": {
      "card-header": {
        "optional": true,
        "children": {
          "card-header-title": { "min": 1 },
          "card-header-icon": { "optional": true }
        }
      },
      "card-image": { "optional": true },
      "card-content": { "optional": true },
      "card-footer": {
        "optional": true,
        "children": { "card-footer-item": { "min": 1 } }
      }
    },
    "equal_height_pattern": {
      "column_class": "is-flex",
      "card_class": "is-fullheight",
      "note": "To normalise card heights in a row, add is-flex to .column and is-fullheight to .card."
    }
  },
  "notification": {
    "root": { "required": ["notification"], "tags": ["div", "article", "p"] },
    "modifiers": ["is-primary", "is-link", "is-info", "is-success",
                  "is-warning", "is-danger", "is-light"],
    "rules": [
      "A colour modifier (is-primary, is-link, is-info, is-success, is-warning, is-danger) is strongly recommended."
    ]
  },
  "navbar": {
    "root": { "required": ["navbar"], "tags": ["nav"] },
    "modifiers": ["is-transparent", "is-fixed-top", "is-fixed-bottom",
                  "is-primary", "is-link", "is-info", "is-success",
                  "is-warning", "is-danger", "is-black", "is-dark",
                  "is-light", "is-white"],
    "children": {
      "navbar-brand": { "optional": true,
        "children": { "navbar-burger": { "optional": true } } },
      "navbar-menu": { "optional": true,
        "children": {
          "navbar-start": { "optional": true },
          "navbar-end": { "optional": true }
        }
      }
    },
    "rules": [
      "navbar-burger must be inside navbar-brand.",
      "navbar-menu requires an id matching the data-target on navbar-burger.",
      "navbar-item and navbar-link must be direct children of navbar-start, navbar-end, or navbar-brand.",
      "Use <button> or <a> for navbar-burger, not <div>."
    ]
  },
  "hero": {
    "root": { "required": ["hero"], "tags": ["section"] },
    "modifiers": ["is-small", "is-medium", "is-large", "is-halfheight",
                  "is-fullheight", "is-fullheight-with-navbar",
                  "is-primary", "is-link", "is-info", "is-success",
                  "is-warning", "is-danger", "is-black", "is-dark",
                  "is-light", "is-white", "is-bold"],
    "children": {
      "hero-body": { "min": 1 }
    }
  },
  "section": {
    "root": { "required": ["section"], "tags": ["section"] },
    "modifiers": ["is-small", "is-medium", "is-large"]
  },
  "button": {
    "root": { "required": ["button"], "tags": ["a", "button", "input"] },
    "modifiers": ["is-primary", "is-link", "is-info", "is-success",
                  "is-warning", "is-danger", "is-white", "is-light",
                  "is-dark", "is-black", "is-small", "is-normal",
                  "is-medium", "is-large", "is-fullwidth", "is-outlined",
                  "is-inverted", "is-rounded", "is-loading", "is-static",
                  "is-ghost"],
    "rules": [
      "Do not use <div> as the tag for a button.",
      "<input type='submit'> with .button must include a value attribute."
    ]
  },
  "tile": {
    "deprecated_in": "1.0.0",
    "rules": [
      "The tile system (is-ancestor, is-parent, is-child) is deprecated in Bulma v1.0+.",
      "Replace tile layouts with .columns / .column structure instead."
    ]
  },
  "modal": {
    "root": { "required": ["modal"], "tags": ["div"] },
    "children": {
      "modal-background": { "min": 1 },
      "modal-content": { "optional": true },
      "modal-card": { "optional": true,
        "children": {
          "modal-card-head": { "min": 1 },
          "modal-card-body": { "min": 1 },
          "modal-card-foot": { "optional": true }
        }
      },
      "modal-close": { "optional": true }
    },
    "rules": [
      "modal-content and modal-card are mutually exclusive — use one or the other.",
      "Add the 'is-active' class via JavaScript to open the modal."
    ]
  },
  "dropdown": {
    "root": { "required": ["dropdown"], "tags": ["div"] },
    "modifiers": ["is-active", "is-hoverable", "is-right", "is-up"],
    "children": {
      "dropdown-trigger": { "min": 1 },
      "dropdown-menu": { "min": 1,
        "children": { "dropdown-content": { "min": 1 } }
      }
    }
  },
  "tabs": {
    "root": { "required": ["tabs"], "tags": ["div"] },
    "modifiers": ["is-centered", "is-right", "is-small", "is-medium",
                  "is-large", "is-boxed", "is-toggle", "is-toggle-rounded",
                  "is-fullwidth"],
    "children": {
      "ul": { "min": 1,
        "children": { "li": { "min": 1 } }
      }
    }
  }
}
```

---

## Validation Rules

### 1. CDN Version Detection

Parse the `<link>` tag for the Bulma CDN URL and extract the semver string.

```
Pattern: /bulma@(\d+\.\d+\.\d+)/
```

- If version `>= 1.0.0`, enable **v1 deprecation checks** (tile system, `is-bold` on hero, etc.)
- If version string is malformed (e.g. `bulma@0.9.31.0.4css`), raise: `ERROR: Malformed Bulma CDN URL — no styles will load.`

### 2. Structural Checks

For each component root found in the HTML:

1. Confirm the element tag is in the component's allowed `tags` list
2. Walk direct children and verify:
   - Required child classes are present (`min: 1`)
   - No unknown classes are used as direct children where the schema defines an explicit child list
3. Check for orphaned modifiers — `is-*` classes on an element whose parent is not a recognised Bulma component root

### 3. Modifier Validity

For each `is-*` class found on an element:

- Look up the element's component (by its root class)
- Check whether the modifier appears in that component's `modifiers` list
- If not, raise: `WARNING: 'is-X' is not a valid modifier for .component-name`

### 4. Deprecated Pattern Detection (v1+)

| Pattern | Severity | Message |
|---|---|---|
| `.tile.is-ancestor` | ERROR | Tile layout deprecated in v1. Replace with `.columns`. |
| `.tile.is-parent` | ERROR | Tile layout deprecated in v1. Replace with `.column`. |
| `.tile.is-child` | ERROR | Tile layout deprecated in v1. Replace with content element inside `.column`. |
| `hero is-bold` | WARNING | `is-bold` gradient modifier removed in Bulma v1. |
| `columns` without any `.column` child | ERROR | `.columns` must contain at least one `.column` child. |

### 5. Consistency Checks

- **Mixed column sizing**: if any `.column` in a `.columns` block has an explicit size modifier (`is-half`, `is-one-third`, etc.) then all siblings should also have explicit sizes, or none should. Mixing implicit and explicit widths produces unpredictable layouts.
- **Equal-height cards**: if `.card` elements appear as siblings inside `.columns`, check whether `.is-flex` is present on each `.column` and `.is-fullheight` on each `.card`. If not, suggest the pattern.
- **navbar-burger / navbar-menu pairing**: `data-target` on the burger must match the `id` on the menu element.
- **`target` attribute syntax**: flag `target:` (colon) instead of `target=` (equals) on any anchor element.

### 6. Semantic Tag Checks

- `<nav>` must be used for `.navbar`, not `<div>`
- `<section>` must be used for `.hero` and `.section`
- `<button>` or `<a>` must be used for `.button`, not `<div>` or `<span>`
- `<article>` is preferred (not required) for `.card`

---

## Output Format

Violations are reported as structured objects, rendered as annotated output:

```
SEVERITY  LINE   COMPONENT    MESSAGE
──────────────────────────────────────────────────────────────────────────
ERROR      87     tile         .tile.is-ancestor detected with Bulma v1.0.4.
                               Tile layout is deprecated. Replace with .columns / .column.
                               → Remove .tile.is-ancestor wrapper; promote each .tile.is-parent
                                 to a .column.is-flex, and each .tile.is-child.notification
                                 to a .notification directly inside that .column.

WARNING    112    notification  .notification missing colour modifier.
                               → Add one of: is-primary, is-link, is-info,
                                 is-success, is-warning, is-danger.

WARNING     45    columns       Mixed column sizing detected: some .column children have
                               explicit size modifiers, others do not.
                               → Either add explicit size classes to all siblings,
                                 or remove them from all.

ERROR       23    —             Malformed Bulma CDN URL: bulma@0.9.31.0.4css.
                               No styles will load.
                               → Fix to: https://cdn.jsdelivr.net/npm/bulma@1.0.4/css/bulma.min.css
```

Severities:

| Level | Meaning |
|---|---|
| `ERROR` | Will cause visible breakage or no styles loading |
| `WARNING` | Incorrect usage that may cause subtle layout or style issues |
| `INFO` | Best-practice suggestion; not a bug |

---

## Extending the Knowledge Base

To add a new component:

1. Add an entry to the JSON schema following the structure above
2. Add a `check_<component>` function in the Python script
3. Register it in `run()`

The schema is intentionally separate from the checker logic so it can be maintained independently — or loaded from a remote URL pointing to a versioned copy of the rules as Bulma releases new versions.

---

## Version Compatibility Matrix

| Bulma version | Tile system | `is-bold` on hero | Smart Grid available |
|---|---|---|---|
| `< 0.9.0` | ✅ Supported | ✅ | ❌ |
| `0.9.x` | ✅ Supported | ✅ | ❌ |
| `>= 1.0.0` | ❌ Deprecated | ❌ Removed | ✅ |
