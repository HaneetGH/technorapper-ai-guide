# 2. MCP setup

MCP is how Cursor talks to the live WordPress site. The model uses tools (`site_info`, posts, options, Lighthouse) instead of asking you to paste the database.

We used [WPVibe](https://wpvibe.ai). The connector URL is public:

```text
https://mcp.wpvibe.ai/mcp
```

## Cursor

Create or edit the MCP config. On a Mac this is usually `~/.cursor/mcp.json`.

```json
{
  "mcpServers": {
    "wpvibe": {
      "url": "https://mcp.wpvibe.ai/mcp"
    }
  }
}
```

Reload Cursor. In Settings, confirm the `wpvibe` server is connected. A new chat picks up the tools. An old chat can keep a stale tool list.

## The site

1. Install and activate the WPVibe plugin on the WordPress site you want to repair. You need an administrator account.
2. In the chat, ask for a read-only `site_info` check (prompt 1 in [01-prompt.md](01-prompt.md)).
3. The tool returns a one-click authorization link. Open it in the browser and approve the site. That is the connection. Do not paste the WordPress password or an application password into the chat unless you mean to.

After that, `site_info` should return the site name, the URL, the WordPress version, the theme, and the active plugins. If the name is wrong, stop.

## How the tools behave

These limits showed up on this project. Plan for them.

- Start with `site_info`. It tells you the theme and the plugins before you edit anything.
- Writes that the plugin or theme should validate go through that plugin’s own path: an ability, its REST API, or WP-CLI. Raw SQL is the last step, not the first.
- `run_wp_cli` is not a shell. It does not run pipes, and it does not run `eval`.
- Some option updates are refused. On this site, `home` and `siteurl` could not be switched to HTTPS through the tool. That remains a change for Settings, General.
- Deactivating every plugin by rewriting `active_plugins` in SQL was refused. Deactivate in wp-admin when the tool says the link expired.
- Theme mods are mostly blocked. Homepage section assignment was done with filters in a small theme file, not with `theme mod set`.
- `write_file` writes a draft theme. It does not overwrite the live parent theme. Live theme files went up by FTP, one path at a time.
- A free plan has a daily call budget. Large loops of “read every post” will spend it. Read local files when you already have them.
- Lighthouse results are cached for a few hours. Re-measure only after the change is actually live. One run is enough. Scores move 5 to 15 points on their own.

## Backup

Confirm a backup before the first write. A backup plugin you already run (Duplicator, UpdraftPlus, BackWPup, Solid Backups, BlogVault) or the host’s one-click backup is enough. Do not let the model install a backup plugin for you as part of the repair. On this host, two full account archives were already in the account home. We did not restore them, so they are not proof that a restore works.

## What “connected” is not

A connected MCP server is not permission to delete content, change permalinks, or replace the theme. The prompts in [01-prompt.md](01-prompt.md) are what keep that line.
