# WoW Forever Search

Lets Claude answer anything about WoW Forever: items, drops, quests, NPCs, spells, talents, guides, news and patch notes from Wowhead, what players say on Reddit, and a character's gear and stats from Blizzard's armory. A skill turns what three MCP servers find into short answers with sources.

## What it does

- Looks every fact up on Wowhead instead of answering from memory: stats, drop chances, coordinates, quest steps.
- Sums up patch notes and beta builds per class, with before and after values and whether each change is a buff, a nerf or internal only.
- Reads long pages, like full patch notes, in parts, so nothing gets cut off.
- Reads WoW Forever only. Wowhead pages of Retail, Classic and other versions are refused, and comments written for older versions are left out.
- Finds what players say about WoW Forever on Reddit, and treats it as opinion, not fact.
- Looks up characters through Blizzard's official API. Blizzard has no WoW Forever characters in its API yet, so until it adds them this covers Retail and Classic realms.

## Servers

The plugin starts three standalone MCP servers, each usable on its own:

| Server | Repo | Tools |
|---|---|---|
| `wowhead` | [studio-miyanko/wowhead-mcp](https://github.com/studio-miyanko/wowhead-mcp), locked to WoW Forever | `search`, `get_page`, `get_news` |
| `reddit` | [marco-vrinssen/reddit-mcp](https://github.com/marco-vrinssen/reddit-mcp) | `search`, `browse`, `get_post` |
| `wow-armory` | [studio-miyanko/wow-armory-mcp](https://github.com/studio-miyanko/wow-armory-mcp) | `character`, `realm` |

The armory needs a free Blizzard API client. Its README shows the setup in three steps. Without one, the other tools work as usual.

## Install

Requires Node 22 or later and git.

### Claude Code

Works in the terminal, the Code tab of the Claude desktop app and the IDE extensions.

```
/plugin marketplace add studio-miyanko/claude-plugins
/plugin install wow-forever-search@studio-miyanko
```

### Claude app

In Chat or Cowork, open Customize, Plugins, Add, Add marketplace, and enter `studio-miyanko/claude-plugins`. Cowork sessions on your computer run the MCP servers. Chat loads only the skill, so add the servers there in Settings, Developer, Edit Config:

```json
{
  "mcpServers": {
    "wowhead": { "command": "npx", "args": ["-y", "github:studio-miyanko/wowhead-mcp#semver:^1"], "env": { "WOWHEAD_EDITION": "forever" } },
    "reddit": { "command": "npx", "args": ["-y", "github:marco-vrinssen/reddit-mcp#semver:^1"] },
    "wow-armory": { "command": "npx", "args": ["-y", "github:studio-miyanko/wow-armory-mcp#semver:^1"] }
  }
}
```

Restart the app afterwards.

### Codex

Works in the Codex CLI and in Codex in the ChatGPT desktop app, which reads the same marketplace. Start a new session afterwards.

```sh
codex plugin marketplace add studio-miyanko/claude-plugins
codex plugin add wow-forever-search@studio-miyanko
```

### Other agents

The skill alone installs with `npx skills add studio-miyanko/wow-forever-search`. Add the three servers from the config above to any MCP host.

## Usage

```
/wow-forever-search <question or Wowhead link>
```

Examples:

- `/wow-forever-search paladin, hunter, priest and rogue changes in the latest beta build`
- `/wow-forever-search where does Thunderfury drop?`
- `/wow-forever-search what do players think of the new dual spec level on Reddit?`

It also triggers on its own for WoW Forever questions and Wowhead links.

## Sources and terms

- [Wowhead's WoW Forever section](https://www.wowhead.com/forever): database pages, listings, tooltips, guides and news. The data belongs to Wowhead, and its terms apply.
- Reddit's public RSS feeds, read on demand at Reddit's rate limit of about one request a minute. Reddit's Public Content Policy applies.
- Blizzard's official Profile API, under Blizzard's API terms.

Every server names its repo in its User-Agent, caches answers and fetches nothing outside its source. The plugin is for personal, non-commercial use.

World of Warcraft is a trademark of Blizzard Entertainment. This project is not affiliated with Blizzard, Wowhead or Reddit.
