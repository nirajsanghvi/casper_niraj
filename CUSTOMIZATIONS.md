# Fork Customizations

This fork of [TryGhost/Casper](https://github.com/TryGhost/Casper) adds the following
personal customizations on top of the upstream theme.

---

## How this fork stays up-to-date

A GitHub Actions workflow (`.github/workflows/sync-upstream.yml`) checks upstream every
Monday, but only acts when upstream publishes a **new release version** (the
`package.json` version changes). Upstream's frequent dependency-only commits between
releases are ignored. When there is a new release:

- If it merges cleanly, the workflow opens a PR that merges that release's tag.
- If there are conflicts, it files a GitHub Issue instead.

Each run also re-enables the workflow, so GitHub's 60-day inactivity rule never
switches the schedule off.

## Getting the theme zip

`.github/workflows/release.yml` builds the theme on every merge to `main` (except
changes to only Markdown or `.github/` files) and attaches it to a GitHub release
tagged `v<upstream version>-niraj.<n>`, e.g. `v5.12.5-niraj.1`. Download the newest one
from the [latest release](https://github.com/nirajsanghvi/casper_niraj/releases/latest).

The zip is named after the release (e.g. `casper-v5.12.5-niraj.1.zip`), so each upload
appears in Ghost Admin as its own theme, e.g. `casper (casper-v5.12.5-niraj.1)`. Older
versions stay installed until deleted, so rolling back is just activating the previous
one.

### Why conflicts should be rare

Each customization below has been **extracted into a dedicated partial file**
(`partials/custom-*.hbs`).  Upstream never touches these files, so upstream changes
to `post.hbs` or `partials/post-card.hbs` will merge cleanly even when those templates
change.

---

## Active customizations

### 1 — Multi-tag display with separators

**File:** `partials/custom-post-tags.hbs`  
**Used in:** `post.hbs`, `partials/post-card.hbs`

Shows up to 3 linked tags (each linking to its tag archive page), separated by ` | `,
instead of only the single primary tag that upstream renders.

The replaced upstream block was:
```hbs
{{#primary_tag}}
    <span class="post-card-primary-tag"><a href="{{url}}">{{name}}</a></span>
{{/primary_tag}}
```

---

### 2 — Conditional "Updated" date

**File:** `partials/custom-updated-date.hbs`  
**Used in:** `post.hbs`

When a post's `updated_at` date differs from its `published_at` date (day-level
comparison), an "Updated: &lt;date&gt;" line is appended to the byline.

---

### 3 — Tags listing page

**File:** `custom-tags.hbs` *(fork-only — not present in upstream)*

A Ghost custom page template that lists all tags alphabetically with their post
counts, linking each to its tag archive page. Assign a page the template
`"Tags"` in Ghost Admin to use it.

---

### 4 — Linkable tags in post cards

**File:** `partials/post-card.hbs`

The upstream template wraps the tags, title and excerpt in a single
`<a class="post-card-content-link">` element, and a tag link can't be nested inside
another `<a>` (invalid HTML). This fork moves only the `<div class="post-card-tags">`
block up so it sits just above that link; the link, header, title and excerpt are
otherwise exactly upstream's markup.

> **Note:** This is the one structural change outside a partial. The diff is a single
> moved block, so upstream edits to the title, excerpt or meta merge cleanly. A
> conflict is only likely if upstream changes the tags block itself.

---

### 5 — Dark color scheme by default

**File:** `package.json` (`config.custom.color_scheme.default`)

Ghost keeps theme settings per installed theme, and each release zip installs as a
separate theme, so every upload would start from the theme's defaults. Changing the
default from `"Light"` to `"Dark"` means each new upload is already in dark mode.

> **Note:** Upstream's routine `package.json` edits (version, dependencies) are in
> other parts of the file and merge cleanly around this line; it was last changed
> upstream in 2022.

---

## Upstream sync history

See the merged [sync pull requests](https://github.com/nirajsanghvi/casper_niraj/pulls?q=is%3Apr+is%3Amerged+%22Sync+upstream%22)
and the [releases](https://github.com/nirajsanghvi/casper_niraj/releases), whose tags
start with the upstream version each build is based on.
