# dikio.gr for Claude Code

Build and edit your dikio.gr website from Claude Code. dikio.gr is the Greek directory and website
builder for lawyers, doctors, hotels and small businesses. With this plugin Claude pulls your site into a
local folder, edits it there like any project, saves your changes back as one draft version, gives you a
preview link and publishes only when you ask. It also reaches the rest of your dikio.gr workspace:
your listing, services, availability and appointments.

## Install

```
/plugin marketplace add https://dikio.gr/claude/marketplace.json
/plugin install dikio@dikio
```

Claude Code asks for your dikio.gr API token during the install. Create one at
https://dikio.gr/app/developer; it acts as you, in the workspace you create it in, and you can revoke it
there at any time. Then run `/dikio:site` and describe what you want to build or change.

## What it contains

- **The dikio MCP server**: a remote server at `https://dikio.gr/api/mcp`, reached over HTTPS with your
  token in the `Authorization` header. Claude Code keeps the token in your operating system's keychain.
- **The `site` skill** (`/dikio:site`): the workflow for building and editing a site.

The plugin has no hooks, runs no programs on your computer and sends nothing anywhere except to
dikio.gr. Through the MCP server, Claude reads your workspace's sites, listing and appointments, and
writes what you ask it to: site files saved as draft versions, a published version when you ask for it,
listing and service changes. When you work on a site, Claude writes its files to a folder on your
computer (`dikio-sites/<site>/` by default) with its own file tools.

## Privacy and terms

What dikio.gr stores and how is described in its [privacy policy](https://dikio.gr/privacy). Use of the
service follows its [terms](https://dikio.gr/terms).
