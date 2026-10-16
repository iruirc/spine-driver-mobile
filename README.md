# spine-driver-mobile

Driver adapter for spine-toolkit: declares what the `mcp-devices` MCP server can drive, per platform and device type.

## What This Is

spine-driver-mobile is a driver plugin for [spine-toolkit](https://github.com/iruirc/spine-toolkit) that declares the capabilities of the `mcp-devices` MCP server (named `claude-in-mobile` in versions before 4.0, and still shipped under that name as the all-in-one edition). The driver ships the server: installing the plugin registers it.

## Installing the MCP Server

Nothing to install by hand: the plugin's `.mcp.json` runs the all-in-one edition
`claude-in-mobile@4.4.1` through `npx` (Node.js required), and Claude Code and Codex CLI both pick it
up when the plugin is installed. The first start downloads the package. In Codex the server's tools
are approved without a question per call; to be asked again, set
`default_tools_approval_mode = "prompt"` under `[plugins."spine-driver-mobile@<marketplace>".mcp_servers.mobile]`
in `~/.codex/config.toml`.

iOS needs WebDriverAgent. Install it the way the server's documentation says
(`npm install -g appium && appium driver install xcuitest`), or, if you installed it some other way,
export its path in your shell profile — the plugin passes it through to the server:

```bash
export WDA_PATH=/path/to/appium-webdriveragent
```

Running a build of the server of your own: in Codex, register it under the key `mobile` in your
configuration — it replaces the plugin's. In Claude Code a server you register yourself runs next to
the plugin's rather than instead of it; point a copy of the plugin's `.mcp.json` at your build instead.

The manifest also recognises a server registered by hand under `mcp-devices` or `claude-in-mobile`.

## Using the Driver

In your project config (`SPINE.md`), declare this driver in the `## Task defaults` block:

```markdown
## Task defaults

[DRIVER] = [spine-driver-mobile]
```

The manifest in `skills/manifest/SKILL.md` documents which platforms and capabilities are supported. See [spine-toolkit: docs/building-a-driver.md](https://github.com/iruirc/spine-toolkit/blob/main/docs/building-a-driver.md) for the driver contract and architecture.
