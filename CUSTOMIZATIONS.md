# Fork Customizations

This fork of [TryGhost/Casper](https://github.com/TryGhost/Casper) adds the following
personal customizations on top of the upstream theme.

---

## How this fork stays up-to-date

A GitHub Actions workflow (`.github/workflows/sync-upstream.yml`) runs every Monday
and automatically opens a pull request when upstream has new commits.  
If the merge is clean the PR is ready to review and merge immediately.  
If there are genuine conflicts the workflow files a GitHub Issue instead.

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

## Upstream sync history

| Synced date | Upstream version | Upstream commit |
|---|---|---|
| 2024-11-19 | v5.8.0 | `e29691b4` |
| 2026-03-14 | v5.10.0 | `10b05b09` |
| 2026-10-29 | v5.12.5 | `2f93171d` |
