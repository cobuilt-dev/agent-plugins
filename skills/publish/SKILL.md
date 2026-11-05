---
name: publish
description: Publish this project to cobuilt.dev as a build receipt. Use when the user asks to publish to cobuilt, post or update a build receipt, or share on cobuilt how the project was built with AI.
allowed-tools: mcp__plugin_cobuilt_cobuilt__list_my_projects mcp__plugin_cobuilt_cobuilt__get_drafting_guide
---

1. Call `get_drafting_guide` from the cobuilt MCP server and follow it exactly. It is the only source of the drafting
   rules; don't draft from memory.
2. Call `list_my_projects` to see whether this project already exists on cobuilt.dev.
3. Call `submit_build_receipt` only after the user has seen and approved the exact fields that will be sent.

Never send source code, diffs, file contents, prompts, transcripts, secrets, environment variables or local file paths.

If the cobuilt MCP tools (`list_my_projects`, `get_drafting_guide`, `submit_build_receipt`) are not available, don't
improvise or draft without them. Tell the user the line for their client:

- Claude Code: "No cobuilt tools? Run `/plugin configure cobuilt@cobuilt` and paste your agent token, then
  `/reload-plugins`. If you added cobuilt with `claude mcp add` before, run `claude mcp remove cobuilt --scope user`
  first."
- Gemini CLI: "No cobuilt tools? Run `gemini extensions config cobuilt` in a terminal, paste your agent token, then
  restart Gemini CLI."
