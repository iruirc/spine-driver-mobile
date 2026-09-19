# spine-driver-mobile

Driver adapter for spine-toolkit: declares what the `mcp-devices` MCP server can drive, per platform and device type.

## What This Is

spine-driver-mobile is a driver plugin for [spine-toolkit](https://github.com/iruirc/spine-toolkit) that declares the capabilities of the `mcp-devices` MCP server (named `claude-in-mobile` in versions before 4.0, and still shipped under that name as the all-in-one edition). The driver does not install the server — you register it separately with your MCP client.

## Installing the MCP Server

Follow the installation instructions for [mcp-devices](https://github.com/AlexGladkov/claude-in-mobile) or its modular edition. Register it with your MCP client under any of these keys (in order of preference): `mobile`, `mcp-devices`, or `claude-in-mobile`.

## Using the Driver

In your project's `CLAUDE-spine-toolkit.md`, declare this driver in the `## Task defaults` block:

```markdown
## Task defaults

[DRIVER] = [spine-driver-mobile]
```

The manifest in `skills/manifest/SKILL.md` documents which platforms and capabilities are supported. See [spine-toolkit: docs/building-a-driver.md](https://github.com/iruirc/spine-toolkit/blob/main/docs/building-a-driver.md) for the driver contract and architecture.
