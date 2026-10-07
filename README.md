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

There is nothing to paste. The first time Claude uses dikio.gr, run `/mcp`, pick **dikio** and sign in:
dikio.gr opens in your browser, you sign in (or create a free account) and approve the connection for
your workspace. Then run `/dikio:site` and describe what you want to build or change. You can disconnect
at any time from https://dikio.gr/app/developer.

## What it contains

- **The dikio MCP server**: a remote server at `https://dikio.gr/api/mcp`, reached over HTTPS. You sign
  in with OAuth (dikio.gr's own sign-in page); Claude Code keeps the resulting access, never your
  password.
- **The `site` skill** (`/dikio:site`): the workflow for building and editing a site.

The plugin has no hooks, runs no programs on your computer and sends nothing anywhere except to
dikio.gr. Through the MCP server, Claude reads your workspace's sites, listing and appointments, and
writes what you ask it to: site files saved as draft versions, a published version when you ask for it,
listing and service changes. When you work on a site, Claude writes its files to a folder on your
computer (`dikio-sites/<site>/` by default) with its own file tools.

## Privacy and terms

What dikio.gr stores and how is described in its [privacy policy](https://dikio.gr/privacy). Use of the
service follows its [terms](https://dikio.gr/terms).
