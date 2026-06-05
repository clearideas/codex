<img src="assets/logo.png" alt="Clear Ideas" height="64">

# Clear Ideas Codex Plugin

This directory packages Clear Ideas as a Codex plugin inside the `clearideas/codex` marketplace repository. It connects Codex to Clear Ideas through MCP, supports OAuth authentication, and includes a skill for Clear Ideas agent authoring workflows.

## Requirements

- A Clear Ideas account: [clearideas.com](https://clearideas.com/)
- Codex with plugin support
- Browser access to complete the Clear Ideas OAuth flow

## What It Includes

- `.codex-plugin/plugin.json` for Codex plugin metadata
- `.mcp.json` for the Clear Ideas MCP connection
- `SETUP.md` for OAuth connection guidance after install
- `skills/create-edit-agents/SKILL.md` for Codex-guided agent authoring
- `assets/` for icon and logo files

## Install

Add the marketplace and install the plugin:

```sh
codex plugin marketplace add clearideas/codex
```

Then open the plugin directory:

```text
/plugins
```

Select the `clearideas` marketplace and install **Clear Ideas**.

## Authentication

The plugin is configured for remote MCP authentication through OAuth. Codex connects to the Clear Ideas MCP endpoint and follows the OAuth discovery flow advertised by the server.

The plugin connection uses the Clear Ideas MCP endpoint:

```json
{
  "mcpServers": {
    "clearideas": {
      "type": "http",
      "url": "https://api.clearideas.com/mcp"
    }
  }
}
```

After loading the plugin, authenticate the `clearideas` server when Codex prompts for MCP authorization.

## Capabilities

- Governed Clear Ideas search from Codex
- Cited answers from approved Clear Ideas content
- Content metadata, summaries, retrieved file content, and diffs
- Authorized file and folder write operations
- Clear Ideas agent authoring, validation, execution, run inspection, and scheduling
- Namespaced skill command: `/clearideas-codex:create-edit-agents`
