# Pontive blog content

The posts, authors and tags behind **https://pontive.com/blog**.

This repo holds content only — no application code. The website
(`revotech-group/pontive-website`) reads it at build time, so a commit here
triggers a rebuild of the site.

## Writing a post

**Use the editor at https://pontive.com/keystatic.** Sign in with GitHub; you need
`write` access to this repo.

The editor writes the files in this repo for you. You should not need to touch
git, and in normal use you should not edit these files by hand — see the warning
below.

### Publishing

Every new post starts as a **draft**.

| | Draft ticked | Draft unticked |
|---|---|---|
| pontive.com/blog | hidden | **live** |
| the staging site | visible | visible |

So: write with **Draft** ticked, check how it looks on staging, then untick
**Draft** and save. The site rebuilds within a few minutes.

Nothing is deleted when you untick Draft — you can tick it again to pull a post
back down.

## Two rules that matter

**1. Don't hand-edit the `.mdx` files.**

The editor stores posts as MDX, which it parses strictly. If a file ends up
containing raw HTML (`<div>`, `<br>`, `<iframe>`…) or an `import` line, the editor
will **refuse to reopen that post** and the website build will fail. Everything
you need is available from the editor's toolbar:

| You want | Use |
|---|---|
| A highlighted box | **Callout** |
| An image with a caption | **Figure** |
| A video | **Embed** |
| A collapsible section | **Details** |
| A button | **Cta** |
| Tables, lists, bold, code blocks | the normal toolbar |

If you need something that isn't there, ask — it's a small change to add one.

**2. Paste as plain text.**

Copying from Google Docs, Notion or Word carries hidden HTML along with the
words, which is the most common way a post ends up unopenable. Paste with
`Cmd+Shift+V` (Mac) or `Ctrl+Shift+V` (Windows).

## Images

Uploaded through the editor; they land in `images/` and are served over the
jsDelivr CDN.

- **Cover images:** 1600×900, WebP or JPEG, **under 300 KB**. Name them
  `<post-slug>-cover.webp`.
- All covers share one folder, so a generic name like `cover.jpg` will overwrite
  another post's. A PR adding anything over 500 KB is rejected automatically.
- Replacing an image at the same filename can take up to 12 hours to update on
  the live site, because the CDN caches it. Upload under a new name instead.

## Layout

```
blog/<slug>/index.mdx   one folder per post
authors/<slug>.yaml
tags/<slug>.yaml
images/blog/            cover and in-body images
images/authors/         portraits
```

---

## For developers

The website (`revotech-group/pontive-website`) reads this repo at build time via
Keystatic's GitHub reader — no token, because this repo is public.

A push here does not produce a commit there, so
`.github/workflows/rebuild-website.yml` calls a Vercel deploy hook instead. It
needs two repository secrets, documented at the top of that file:
`VERCEL_DEPLOY_HOOK_PRODUCTION` (required) and `VERCEL_DEPLOY_HOOK_STAGING`
(optional). Until the production one is set, content pushes will fail the
workflow — deliberately, because the site genuinely is not being rebuilt.
