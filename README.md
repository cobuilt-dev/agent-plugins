# cobuilt plugin

Publish a build receipt for your project to [cobuilt.dev](https://cobuilt.dev) from your AI coding agent. A build
receipt is an honest record of how a project was built and how AI was involved. Your agent drafts it from the
repository and the session, asks you what only you can answer, and sends nothing until you have approved every field.

## Install (Claude Code)

1. Sign in at [cobuilt.dev](https://cobuilt.dev) and create an agent token: **Settings → Agent tokens** (it starts
   with `cbt_`).
2. In a Claude Code session, run:

   ```text
   /plugin install cobuilt --marketplace cobuilt-dev/agent-plugins
   ```

   Paste your token when the dialog asks for it, then run `/reload-plugins`.
3. Say **"Publish this project to cobuilt"**, or run `/cobuilt:publish`.

Setup for other clients: [cobuilt.dev/connect](https://cobuilt.dev/connect).

### If it doesn't work

- **Added cobuilt with `claude mcp add` before?** Run `claude mcp remove cobuilt --scope user` first. A server you
  added by hand at the same address silently replaces the plugin's.
- **No cobuilt tools?** Run `/plugin configure cobuilt@cobuilt`, paste your token, then `/reload-plugins`. Installing
  from the shell (`claude plugin install`) doesn't ask for the token.

## What it contains

- The cobuilt MCP server (`https://cobuilt.dev/mcp`), sent your token as `Authorization: Bearer …`. Its tools: list
  your projects, get the drafting guide, submit a build receipt.
- One skill, `publish`. The drafting guide and the project list load without a permission prompt; submitting a receipt
  always asks you first.

## Your token

Claude Code stores the token as a secret: in the macOS Keychain, elsewhere in `~/.claude/.credentials.json`.
Uninstalling the plugin removes it. Revoking the token in **Settings → Agent tokens** on cobuilt.dev is the real off
switch.

## Privacy

Only the receipt fields you approved are sent. The plugin never sends source code, diffs, file contents, prompts, chat
transcripts, secrets, environment variables or local file paths. See the
[privacy policy](https://cobuilt.dev/privacy) and [terms](https://cobuilt.dev/terms).

## Releases

Each change bumps the version in every manifest and gets a GitHub Release tagged `vX.Y.Z`. Changes to the drafting
guide and the tools ship on cobuilt.dev and need no plugin update.

## License

[MIT](LICENSE)
