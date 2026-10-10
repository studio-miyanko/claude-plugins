# claude-plugins

## Rules

- The repo is the marketplace `studio-miyanko`, listed in `.claude-plugin/marketplace.json`. The local folder is `~/Developer/studio-miyanko-claude-plugins`, because `~/Developer/claude-plugins` is Marco's own marketplace.
- Plugins without a runtime package live in `plugins/<name>/` with their own `.claude-plugin/plugin.json`, `skills/`, README and CHANGELOG, listed with `source: "./plugins/<name>"`.
- wow-forever-search stays its own repo, because its root is also the MCP server package that `npx github:studio-miyanko/wow-forever-search` installs. It is listed with a `url` source, which Codex reads too. Its rules and release steps live there.
- Commits are authored as miyanko through the `includeIf` in `~/.gitconfig`, which matches the `studio-miyanko` remote.

## wow-addon-dev

- Lookups use shallow per-branch clones from `skills/wow-addon-dev/scripts/wow-source.sh`, not GitHub MCP. MCP returns whole files and needs a token, and GitHub code search only indexes `live`.
- Add no `userConfig`. A public clone must work without a secret.
- Hard-code no interface numbers or builds in the skill. They change every patch, so the skill reads them from the branch.
- For a submission to Anthropic's directory, point to it with a `git-subdir` source: `url` `studio-miyanko/claude-plugins`, `path` `plugins/wow-addon-dev`.

## Checks

- `claude plugin validate --strict .` checks the marketplace, and `claude plugin validate --strict plugins/<name>/.claude-plugin/plugin.json` each local plugin. Fix every warning.
- `npx skills add . --list` confirms that other agents find every skill.
- The Validate workflow runs all of them on every push to `main` and every pull request.

## Release

1. Bump `version` in the plugin's `plugin.json`, or installed copies never update.
2. Add the changes to the plugin's `CHANGELOG.md`.
3. Push and wait for the Validate workflow to pass.
4. Tag `<name>-vX.Y.Z` and create a GitHub release.
