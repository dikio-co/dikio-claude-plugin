---
name: site
description: Build or edit a dikio.gr website. Pulls the site into a local folder, edits it there, saves it as a draft version, shares a preview link and publishes when the owner says so. Use when the user wants to create, redesign or change their dikio.gr site, or asks about its pages.
argument-hint: "[what to build or change]"
---

# Build or edit a dikio.gr site

The user's dikio.gr websites are reached through the `dikio` MCP server of this plugin. You edit a
local copy with your own file tools and save it back as one draft version. Nothing goes live until
`site_publish`.

Task: $ARGUMENTS

## 0. Make sure you are signed in

The dikio tools need the user's dikio.gr account. The first time, or after a sign-out, a dikio tool fails
with an authentication error, or `/mcp` lists dikio as needing authentication. Then tell the user to run
`/mcp`, pick **dikio** and choose to authenticate: dikio.gr opens in their browser, they sign in and
approve. Someone without an account creates one there first (free) and sets up a workspace, then
approves. Their sites live in that workspace.

## 1. Read the rules first

Call `site_guide` once per session and follow it. It is what dikio's own site agent follows: Greek first,
the advertising rules for lawyers and doctors, structured data, how widgets (forms, booking, maps) are
written in the HTML so the owner can still edit them in dikio's editor. Ignore the parts about the
built-in agent's own tools (`sh`, `write`, `deploy`) and about answering with buttons.

## 2. Pick the site

- `sites_list` shows the workspace's sites with their draft and published versions.
- If the user has none, or asks for a new one, `site_create` with the office or business name.
- Use the `site` value exactly as returned (for example `grafeio-papadopoulou--112`).

## 3. Pull it into a local folder

Call `site_pull`. Write every file that comes back with `content` to `dikio-sites/<site>/<path>`
(relative to the current folder, unless the user names another place). Files marked binary come without
bytes; fetch one with `site_read_file` only if you need it (it returns `base64`).

Then write `dikio-sites/<site>/.dikio.json`:

```json
{ "site": "<site>", "base_version": 268, "files": { "index.html": "<hash>", "styles.css": "<hash>" } }
```

with the `base_version` and every file's `hash` from the pull. The hash is the SHA-256 of the file's
bytes, so you can tell later exactly which files changed.

## 4. Edit locally

Edit, add and delete files in that folder like any project. Keep pages as plain HTML, CSS and JS, keep
`index.html` as the homepage, and keep widget elements and their `data-*` attributes intact.

For a quick look you can serve the folder with any static server, but the real render (layouts, widgets,
forms) is dikio's draft preview, so save and use `site_preview` before telling the user it is done.

## 5. Save as one draft version

1. Hash every local file (`shasum -a 256` or `sha256sum`) and compare with `.dikio.json`. Changed and new
   files go in `files`; files listed in `.dikio.json` but gone locally go in `delete`.
2. Call `site_save` once with `base_version`, a short `message` (what changed, in the user's language),
   the files (text as `content`, images and fonts as `base64`) and the deletions.
3. Keep one call under about 4 MB. Save large images in their own `site_save` calls first, each with the
   version the previous call returned.
4. Update `.dikio.json` with the returned `base_version` and the hashes of what you saved.

A `409` means the draft changed since you pulled (the owner edited it in dikio's editor). Pull again into
a fresh folder, reapply your changes on top of their version, and save again. Never overwrite their edits.

`site_save` returns `validation` for the pages you saved: `errors` (unclosed or stray tags) and `warnings`
(no `<title>`, no viewport meta, no `lang`, links to files that do not exist). Fix them and save again.

## 6. Preview, then publish only when asked

- Give the user the `preview_url` from `site_save` or `site_preview`. It works without signing in for 15
  minutes; call `site_preview` again for a fresh link.
- Call `site_publish` only when the user explicitly asks to publish. It makes the current draft live at
  the site's public address and returns the URL.
