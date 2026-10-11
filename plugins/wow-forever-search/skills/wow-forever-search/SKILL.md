---
name: wow-forever-search
description: "answer any question about wow forever through this plugin's wowhead, reddit and wow-armory mcp tools. use for items, drops, quests, npcs, spells, talents, classes, professions, guides, news, patch notes, beta builds and class changes, for what players say about wow forever on reddit, for a character's gear and stats, and whenever a wowhead forever link appears. looks everything up instead of answering from memory, and turns long patch notes into short per-class overviews with before and after values and source links."
---

# WoW Forever Search

Answer questions about WoW Forever from this plugin's tools. Look everything up, never answer stats, drop chances, coordinates, quest steps or numbers from memory, because WoW Forever changes them from the original game.

## Sources

| Server | Tools | Use for |
|---|---|---|
| wowhead | `search`, `get_page`, `get_news` | Game facts, guides, news and patch notes. Reads WoW Forever pages only |
| reddit | `search`, `browse`, `get_post` | What players say: opinions, tips, bugs and reactions to changes |
| wow-armory | `character`, `realm` | A character's level, spec, gear and stats |

## WoW Forever only

- The wowhead tools refuse pages of other versions. If the user asks about Retail, Classic or another version, say in one sentence that this plugin covers WoW Forever only.
- If the user pastes a Retail or Classic Wowhead link, open the same path under `https://www.wowhead.com/forever/` instead, like `/forever/item=19019`.
- On Reddit, look in r/wowforever first, and add "WoW Forever" to queries across Reddit. Keep only posts about WoW Forever.
- Blizzard's API has no WoW Forever characters yet. Forever launches on 2026-11-04 without realms, so until Blizzard adds it, `character` reads Retail and Classic realms only. Say so when asked about a Forever character, and name the version of any character you look up.

## Find the source

| The user gives | Do |
|---|---|
| A Wowhead URL | `get_page` on it |
| Recent changes, a patch, a beta build | `get_news`, then `get_page` on the matching post |
| An item, quest, NPC or spell name | wowhead `search` with the English name, then `get_page` on the best match |
| A topic without a name | `get_page` on the guide hub `https://www.wowhead.com/forever/guides` |
| What players think, a bug, a community tip | reddit `search` in r/wowforever, then `get_post` on the best one or two threads |
| A Reddit URL | `get_post` for a post, `browse` for a community or user |
| A character and realm | `character`. Use `realm` first if the realm name is unclear |

## Read all of it

Long pages come in parts. A part that ends with `[Part ends at N ... offset N ...]` has more: call `get_page` again with that `offset` until a part has no such line. Never summarize patch notes from the first part only.

Reading patch notes:

- `52 (was 38)` is a change from 38 to 52. `(new: ...)` is added text, `(removed: ...)` is removed text.
- `## Hunter`, `## Survival` and similar headings name the class and the talent tree or spell group.
- A spell listed several times is one entry per rank. Report the change across ranks, like `7/12/18 → 5/8/12`.
- Lines like `Effect #1 Apply Aura`, `Mod Resistance → Effect Aura #674`, `ProcChance changed` or `Category changed` are internal data. When the tooltip numbers stay the same, call it "no visible change".
- Wording that only changes "The hunter takes" to "You take" is "wording only".

## Reddit

- Anonymous Reddit allows about one request a minute, so each call can wait up to a minute. Use at most three Reddit calls per answer, and only when the question is about players or Wowhead has no answer.
- Reddit RSS has no scores or comment counts. Never claim a post is popular or upvoted.
- Posts are opinions, not facts. Say how many threads an answer rests on, and keep game facts from Wowhead.

## Answer

- First line is the answer.
- Patch notes and class changes: one `##` section per class or topic the user asked for, each a table with the columns Change, Before → after and Effect. Effect is buff, nerf, no visible change or wording only, plus a few words on why it matters.
- Order rows by impact, biggest first. Put internal-only changes in one closing row or sentence.
- Give percentages where they help, like "about 33% less".
- Link the Wowhead pages and Reddit threads you used.
- If the sources don't have the answer, say so plainly.
