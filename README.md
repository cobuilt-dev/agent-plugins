# cobuilt plugin

[![mcptry](https://mcptry.com/badge/dev.cobuilt/cobuilt)](https://mcptry.com/servers/dev.cobuilt/cobuilt)

Publish a build receipt for your project to [cobuilt.dev](https://cobuilt.dev) from your AI coding agent. A build
receipt is an honest record of how a project was built and how AI was involved. Your agent drafts it from the
repository and the session, asks you what only you can answer, and sends nothing until you have approved every field.

## Where it works

Publishing works in Claude Code and Gemini CLI, which can send your agent token. In claude.ai chat and Cowork the skill
loads but the cobuilt tools don't work, because those apps connect servers through an OAuth sign-in, which cobuilt
doesn't offer yet. Open your project in Claude Code to publish.

## Try it

Say any of these in your project:

- "Publish this project to cobuilt." Your agent drafts the receipt, asks what it can't know, and shows you the final
  fields to approve.
- "Show me the build receipt this project has on cobuilt." It finds the project among yours and reads the current
  receipt. Nothing is changed.
- "Update this project's build receipt on cobuilt." It starts from the published receipt, shows you the old and new text
  side by side, and warns you before replacing anything.

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
- **Added the plugin on claude.ai?** It reaches Claude Code as `cobuilt@synced`, so run
  `/plugin configure cobuilt@synced` instead.
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

Claude Code stores the token as a secret: in the macOS Keychain, elsewhere in Claude Code's own credentials file in your
user folder.
Uninstalling the plugin removes it.

Gemini CLI stores it in your system keychain (on Windows, Credential Manager under "Gemini CLI Extensions cobuilt …";
without a keychain, in a file in Gemini CLI's own settings folder). Uninstalling the extension does **not** remove it: delete
that entry yourself (on Windows: Control Panel → Credential Manager → Windows Credentials → Generic Credentials).

Revoking the token in **Settings → Agent tokens** on cobuilt.dev is the real off switch.

## What it sends and where

The plugin runs nothing on your machine: no hooks, scripts or local commands. It talks to one server,
`https://cobuilt.dev/mcp`, and every request carries your agent token.

- **Reading:** your agent asks for the drafting guide, your list of projects and a project's current receipt.
- **Publishing:** after you approve, it sends the receipt: project title, description, links, type and tech stack (for
  a new project), how AI was involved, the AI tools used, what the AI did, what you did, what went wrong, the agent and
  model names, and optional summary numbers such as commit counts.

A submitted receipt is public once its project is approved, and a new project is reviewed first. Sending a receipt for
an existing project replaces its current one. cobuilt keeps the receipt until you replace it or delete the project.

The plugin's instructions tell your agent never to send source code, diffs, file contents, prompts, chat transcripts,
secrets, environment variables or local file paths. Your agent shows you every field first, so check them before you
approve. See the [privacy policy](https://cobuilt.dev/privacy) and [terms](https://cobuilt.dev/terms).

## Releases

Each change bumps the version in every manifest and gets a GitHub Release tagged `vX.Y.Z`. Changes to the drafting
guide and the tools ship on cobuilt.dev and need no plugin update. Changes to this README alone don't bump the version.

`server.json` lists the server in the [official MCP Registry](https://registry.modelcontextprotocol.io) as
`dev.cobuilt/cobuilt`. It carries the same version as the plugin, and a published version can't be republished, so a change
to it needs a release and then a new publish.

## License

[MIT](LICENSE)
