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

### If it doesn't work

- **Added cobuilt with `claude mcp add` before?** Run `claude mcp remove cobuilt --scope user` first. A server you
  added by hand at the same address silently replaces the plugin's.
- **No cobuilt tools?** Run `/plugin configure cobuilt@cobuilt`, paste your token, then `/reload-plugins`. Installing
  from the shell (`claude plugin install`) doesn't ask for the token.
- **Use Cursor too?** Cursor imports the Claude Code plugins you installed (**Cursor Settings → Agents → Third-Party
  Imports**, on by default) and lists this one as "cobuilt · Plugin" with an error, because it can't read the token.
  Switch that entry off in Cursor's MCP list (it stays off after a restart) and set Cursor up with the
  `~/.cursor/mcp.json` snippet from [cobuilt.dev/connect](https://cobuilt.dev/connect).

## Install (Gemini CLI)

1. Create an agent token as above.
2. In a terminal, run:

   ```text
   gemini extensions install https://github.com/cobuilt-dev/agent-plugins
   ```

   Paste your token when it asks for **cobuilt agent token**.
3. Start Gemini CLI and say **"Publish this project to cobuilt"**.

**No cobuilt tools?** Run `gemini extensions config cobuilt`, paste your token, then restart Gemini CLI.
`gemini mcp list` should show `cobuilt` as connected.

Setup for other clients: [cobuilt.dev/connect](https://cobuilt.dev/connect).

## What it contains

- The cobuilt MCP server (`https://cobuilt.dev/mcp`), sent your token as `Authorization: Bearer …`. Its tools: list
  your projects, get the drafting guide, read your current receipt, submit a build receipt.
- One skill, `publish`, shared by Claude Code and Gemini CLI. In Claude Code the drafting guide, the project list and
  your current receipt load without a permission prompt; submitting a receipt always asks you first.

## Your token

Claude Code stores the token as a secret: in the macOS Keychain, elsewhere in `~/.claude/.credentials.json`.
Uninstalling the plugin removes it.

Gemini CLI stores it in your system keychain (on Windows, Credential Manager under "Gemini CLI Extensions cobuilt …";
without a keychain, `~/.gemini/gemini-credentials.json`). Uninstalling the extension does **not** remove it: delete
that entry yourself (on Windows: Control Panel → Credential Manager → Windows Credentials → Generic Credentials).

Revoking the token in **Settings → Agent tokens** on cobuilt.dev is the real off switch.

## Privacy

Only the receipt fields you approved are sent. The plugin never sends source code, diffs, file contents, prompts, chat
transcripts, secrets, environment variables or local file paths. See the
[privacy policy](https://cobuilt.dev/privacy) and [terms](https://cobuilt.dev/terms).

## Releases

Each change bumps the version in every manifest and gets a GitHub Release tagged `vX.Y.Z`. Changes to the drafting
guide and the tools ship on cobuilt.dev and need no plugin update. Changes to this README alone don't bump the version.

## License

[MIT](LICENSE)
