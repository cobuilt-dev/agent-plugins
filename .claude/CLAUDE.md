# agent-plugins

The cobuilt plugin: one skill (`skills/publish/SKILL.md`) plus a pointer to the hosted MCP server `https://cobuilt.dev/mcp`. It ships to Claude Code, Gemini CLI and the MCP Registry from this one repo. There is no code, build or test suite; the repo is manifests, one skill and a README. The server and its tools live in the separate cobuilt repo (`E:\home\repos\cobuilt`), not here.

## Manifests

Each client reads its own file; all describe the same plugin.

- `.claude-plugin/plugin.json`: Claude Code plugin. Holds the version and the `agent_token` userConfig (`sensitive`).
- `.claude-plugin/marketplace.json`: makes the repo its own marketplace (`cobuilt-dev/agent-plugins`).
- `.mcp.json`: Claude Code's MCP wiring. Token comes from `${user_config.agent_token}`.
- `gemini-extension.json`: Gemini CLI. Token comes from env var `COBUILT_TOKEN`; uses `httpUrl`, not `url`.
- `server.json`: official MCP Registry entry (`dev.cobuilt/cobuilt`).

## Releasing

- `version` must be identical in `plugin.json`, `gemini-extension.json` and `server.json`. CI fails on any mismatch, and on a tag `vX.Y.Z` that differs from `plugin.json`. Bump all three in one commit.
- A change to the skill or any manifest is a release: bump the version (patch for wording, minor for new behaviour), then tag `v<version>` after merge.
- Check locally before pushing: `claude plugin validate . --strict` and `claude plugin validate . --profile directory --strict`. These are what `.github/workflows/validate.yml` runs.
- Work goes on a branch and lands through a PR into `main`.

## Gotchas

- The skill must not name tools in `allowed-tools`; it was removed in 1.3.1 because it broke the directory checks. Tool names appear only in the skill body.
- `SKILL.md` deliberately holds no drafting rules. They come from the server's `get_drafting_guide` so they update without a plugin release. Don't copy them into the skill or README.
- The skill's fallback text (tool-missing messages per client) and the README troubleshooting section repeat the same commands. Edit both together.
- The skill must never send source, diffs, prompts, transcripts, secrets, env vars or local paths, and `submit_build_receipt` only runs after the user approves the exact fields. Keep both guardrails in any rewrite.
- README states where tokens are stored without naming credential file paths (a past fix); keep it that way.
- Never put a real `cbt_` token in any file. Placeholders only.
- `.claude/settings.local.json` is local; don't commit it.
- Line endings are LF (`.gitattributes`); `icon.png` is binary.
