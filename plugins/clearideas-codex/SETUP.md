# Clear Ideas Setup

Use this setup guide when the user installs or enables the Clear Ideas Codex plugin and wants to connect the bundled Clear Ideas MCP server.

## Requirements

- A Clear Ideas account: [clearideas.com](https://clearideas.com/)
- Codex with plugin support
- Browser access to complete the Clear Ideas OAuth flow

## Connect The MCP Server

1. Open a new Codex thread after installing the plugin.
2. Select the Clear Ideas plugin or ask Codex to use Clear Ideas.
3. Start the `clearideas` MCP authentication flow when prompted.
4. Approve the requested Clear Ideas scopes in the browser.
5. Return to Codex and confirm that the server is connected.

Authentication uses Clear Ideas OAuth through MCP discovery.

## Expected Capabilities

After authentication, Codex should see Clear Ideas MCP tools for:

- governed site and content search
- cited answers
- file metadata, content, summaries, and diffs
- folder and file write operations when authorized
- agent listing, authoring, validation, execution, run inspection, and scheduling

To refresh available marketplace files after a Clear Ideas plugin update, run:

```sh
codex plugin marketplace upgrade clearideas
```
