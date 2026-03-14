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

### 4 — Granular linking in post cards

**File:** `partials/post-card.hbs`

The upstream template wraps both the header and excerpt in a single
`<a class="post-card-content-link">` element.  This fork splits them into
**two separate anchor tags** — one for the title, one for the excerpt — so that the
tag links inside the header (added by customization #1) are not nested inside
another `<a>` element (which is invalid HTML).

> **Note:** This is a structural change to `partials/post-card.hbs` itself (not
> isolated in a partial), but because it only affects the element nesting around
> the title/excerpt, upstream changes to post-card content rarely conflict with it.

---

## Upstream sync history

| Synced date | Upstream version | Upstream commit |
|---|---|---|
| 2024-11-19 | v5.8.0 | `e29691b4` |
| 2026-03-14 | v5.10.0 | `10b05b09` |
