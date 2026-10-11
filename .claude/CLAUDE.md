# wow-forever-search

## Rules

- Since 2.0.0 (2026-10-11) the repo is a Claude Code plugin with the Agent Skill in `skills/wow-forever-search/` and no server code. `.mcp.json` starts three standalone MCP servers: `studio-miyanko/wowhead-mcp` with `WOWHEAD_EDITION=forever`, `marco-vrinssen/reddit-mcp` and `studio-miyanko/wow-armory-mcp`. Fix sources in those repos.
- `.mcp.json` pins each server to an exact tag with `npx -y github:<repo>#vX.Y.Z`, because Claude Code runs no `npm install` for plugins. Skill and tools never drift, and a server's `main` can break without reaching users. Bumping a server tag is a plugin release.
- It is listed in the marketplace repo `studio-miyanko/claude-plugins` with a `url` source, which both Claude Code and Codex read.
- WoW Forever only (user, 2026-10-09), so answers never mix in Retail or Classic. The wowhead server refuses pages without the Forever label, checked after redirects, and keeps only comments written for Forever. The skill limits Reddit to posts about Forever. `character` reads Retail and Classic until Blizzard's API has Forever characters, and the skill says so.
- Personal, non-commercial use only. Wowhead's EULA forbids commercial use without written permission, Reddit's Public Content Policy restricts commercial use, and Anthropic's directory policy wants control over every fetched site, so the plugin is not submitted there (checked 2026-10-09).

## Sources

- Add a source only as a standalone MCP server, after checking its robots.txt, terms and AI content signals. Identify honestly in the User-Agent, and never fake a browser or route around a block.
- Reddit was added on 2026-10-11 at the user's request, although its robots.txt disallows all crawlers. The reddit server reads only the public RSS feeds Reddit offers to feed readers, on demand and at Reddit's rate limit. The logged-out JSON API answers 403, and new OAuth apps need Reddit's approval since 2025-11.
- Checked on 2026-10-07 and left out: Icy Veins (has `/wow-forever/` guides, but Cloudflare blocks every non-browser client), Archon.gg (same), Warcraft Logs (no WoW Forever logs, API needs an OAuth client), Forever Logs (robots.txt disallows `/api/` and `/reports/`, its API answers "API access not allowed"). Raider.IO went with the retail character tool on 2026-10-09. Warcraft Wiki was dropped at the user's request.
- WoW Forever launches on 2026-11-04 without realms: players pick a ruleset and have a two-part name unique per region. Once Blizzard publishes a Forever namespace, add a `forever` flavor to wow-armory-mcp and lock the plugin's server to it.

## Checks

- `claude plugin validate --strict .claude-plugin/plugin.json` checks the plugin. Fix every warning.
- `npx skills add . --list` confirms that other agents find the skill.
- `claude --plugin-dir .` loads the plugin with its three servers. `/mcp` shows whether each one started.
- The Validate workflow runs both checks on every push to `main` and every pull request.

## Release

1. Bump `version` in `.claude-plugin/plugin.json`. Installed copies only update when the plugin version changes.
2. Point each server tag in `.mcp.json` at a released tag of that server.
3. Run the checks and fix every warning.
4. Add the changes to `CHANGELOG.md`.
5. Push and wait for the Validate workflow to pass.
6. Tag `vX.Y.Z` and create a GitHub release.
