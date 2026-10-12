# claude-plugins

## Rules

- The repo is the marketplace `studio-miyanko`, listed in `.claude-plugin/marketplace.json`. The local folder is `~/Developer/studio-miyanko-claude-plugins`, because `~/Developer/claude-plugins` is Marco's own marketplace.
- Plugins without a runtime package live in `plugins/<name>/` with their own `.claude-plugin/plugin.json`, `skills/`, README and CHANGELOG, listed with `source: "./plugins/<name>"`.
- Commits are authored as miyanko through the `includeIf` in `~/.gitconfig`, which matches the `studio-miyanko` remote.

## wow-addon-dev

- Lookups use shallow per-branch clones from `skills/wow-addon-dev/scripts/wow-source.sh`, not GitHub MCP. MCP returns whole files and needs a token, and GitHub code search only indexes `live`.
- Add no `userConfig`. A public clone must work without a secret.
- Hard-code no interface numbers or builds in the skill. They change every patch, so the skill reads them from the branch.
- For a submission to Anthropic's directory, point to it with a `git-subdir` source: `url` `studio-miyanko/claude-plugins`, `path` `plugins/wow-addon-dev`.

## wow-forever-search

- Skill plus `.mcp.json`, no server code. It moved here from `studio-miyanko/wow-forever-search` on 2026-10-11, with its history, and that repo is archived. Fix sources in the server repos `studio-miyanko/wowhead-mcp` and `studio-miyanko/wow-armory-mcp`.
- Wowhead and the armory are the hosted endpoints `https://wowhead-mcp.marco-vrinssen.workers.dev/forever/mcp` and `https://wow-armory-mcp.marco-vrinssen.workers.dev/mcp`, so users need no setup for them. They serve the server release deployed last.
- WoW Forever only (user, 2026-10-09), so answers never mix in Retail or Classic. The wowhead endpoint refuses pages of other versions and keeps only Forever comments. `character` reads Retail and Classic until Blizzard's API has Forever characters, and the skill says so.
- Public and installable by anyone, but not submitted to Anthropic's directory: its policy wants control over every site a plugin fetches, and Wowhead's EULA forbids commercial use without written permission (checked 2026-10-09).
- Add a source only as a standalone MCP server, after checking its robots.txt, terms and AI content signals. Identify honestly, and never fake a browser or route around a block.
- Left out: Icy Veins and Archon.gg (Cloudflare blocks non-browser clients), Warcraft Logs (no Forever logs yet), Forever Logs (robots.txt and API refuse), Warcraft Wiki (user's call), Reddit (added 2026-10-11, removed 2026-10-12 at the user's request because the anonymous RSS server did not work). WoW Forever launches on 2026-11-04 without realms: once Blizzard publishes a Forever namespace, add a `forever` flavor to wow-armory-mcp.
- `claude --plugin-dir plugins/wow-forever-search` loads it, and `/mcp` shows whether both servers connect.

## Checks

- `claude plugin validate --strict .` checks the marketplace, and `claude plugin validate --strict plugins/<name>/.claude-plugin/plugin.json` each local plugin. Fix every warning.
- `npx skills add . --list` confirms that other agents find every skill.
- The Validate workflow runs all of them on every push to `main` and every pull request.

## Release

1. Bump `version` in the plugin's `plugin.json`, or installed copies never update.
2. Add the changes to the plugin's `CHANGELOG.md`.
3. Push and wait for the Validate workflow to pass.
4. Tag `<name>-vX.Y.Z` and create a GitHub release.
