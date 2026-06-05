<img src="plugins/clearideas-codex/assets/logo.png" alt="Clear Ideas" height="64">

# Clear Ideas Codex Marketplace

This repository is the Codex plugin marketplace for Clear Ideas. It makes Clear Ideas available in Codex through a reusable marketplace install.

## Plugin

- `clearideas-codex`: Clear Ideas MCP tools, OAuth authentication, cited answers, governed knowledge search, and agent authoring guidance.

## Requirements

- A Clear Ideas account: [clearideas.com](https://clearideas.com/)
- Codex with plugin support
- Browser access to complete the Clear Ideas OAuth flow

## Install

Add the marketplace:

```sh
codex plugin marketplace add clearideas/codex
```

Open the plugin directory:

```text
/plugins
```

Select the `clearideas` marketplace and install **Clear Ideas**.

After installation, start a new Codex thread and authenticate the `clearideas` MCP server when prompted.

## What You Get

- Governed Clear Ideas search from Codex
- Cited answers from approved Clear Ideas content
- Content metadata, summaries, retrieved file content, and diffs
- Authorized file and folder write operations
- Clear Ideas agent authoring, validation, execution, run inspection, and scheduling

## Repository Layout

```text
.agents/
  plugins/
    marketplace.json
plugins/
  clearideas-codex/
    .codex-plugin/plugin.json
    .mcp.json
    SETUP.md
    skills/create-edit-agents/SKILL.md
    assets/
```

## Configuration

The plugin uses the Clear Ideas MCP endpoint:

```text
https://api.clearideas.com/mcp
```

Authentication is handled through Clear Ideas OAuth and Codex MCP discovery.

For releases, keep both versions in sync:

- `version` in `plugins/clearideas-codex/.codex-plugin/plugin.json`
- the top-level marketplace version if one is added to `.agents/plugins/marketplace.json`
