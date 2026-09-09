# mobile-driver

Driver adapter for spine-toolkit: declares what the `mcp-devices` MCP server can drive, per platform and device type.

## What This Is

mobile-driver is a driver plugin for [spine-toolkit](https://github.com/spine/spine-toolkit) that declares the capabilities of the `mcp-devices` MCP server. This server was formerly known as `claude-in-mobile`. The driver does not install the server — you register it separately with your MCP client.

## Installing the MCP Server

Follow the installation instructions for [mcp-devices](https://github.com/anthropics/mcp-servers) or its all-in-one distribution `claude-in-mobile`. Register it with your MCP client under any of these keys (in order of preference): `mobile`, `mcp-devices`, or `claude-in-mobile`.

## Using the Driver

In your project's `CLAUDE-spine-toolkit.md`, declare this driver in the `## Validation` block:

```markdown
## Validation

driver: mobile-driver
```

The manifest in `skills/manifest/SKILL.md` documents which platforms and capabilities are supported. See [spine-toolkit: docs/building-a-driver.md](https://github.com/spine/spine-toolkit/blob/main/docs/building-a-driver.md) for the driver contract and architecture.
