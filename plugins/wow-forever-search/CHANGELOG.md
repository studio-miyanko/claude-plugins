# Changelog

## 2.2.0 (2026-10-11)

- Wowhead runs as the hosted endpoint of wowhead-mcp, locked to WoW Forever, so only Reddit still runs locally.
- Moved into `studio-miyanko/claude-plugins` with its history. The install commands stay the same.

## 2.1.0 (2026-10-11)

- The armory runs as the hosted endpoint of wow-armory-mcp, so character lookups need no Blizzard client and no setup.

## 2.0.0 (2026-10-11)

- The plugin no longer ships its own MCP server. It starts three standalone servers instead: `wowhead-mcp` locked to WoW Forever, `reddit-mcp` and `wow-armory-mcp`.
- New: what players say about WoW Forever on Reddit, through public RSS feeds.
- New: character level, spec, gear and stats through Blizzard's official API, for Retail and Classic until Blizzard adds WoW Forever.
- Breaking for MCP-only users: `github:studio-miyanko/wow-forever-search` no longer runs a server from 2.0.0 on. Use `github:studio-miyanko/wowhead-mcp#semver:^1` with `WOWHEAD_EDITION=forever`. The `#semver:^1` range keeps running 1.1.0.

## 1.1.0 (2026-10-10)

- `src/server.js` exports `createServer()`, so other hosts such as a Cloudflare Worker can run the same tools over an in-memory transport. The stdio server is unchanged.

## 1.0.0 (2026-10-09)

- First release. A Claude Code plugin with a bundled MCP server and skill that search WoW Forever on Wowhead: items, drops, quests, NPCs, spells, guides, news and patch notes.
- Reads WoW Forever only: pages and comments Wowhead labels as another version are left out.
- Reads long pages in parts, keeps old and new values in patch notes and sums up class changes per class.
