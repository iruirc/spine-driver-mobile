---
name: manifest
description: Driver manifest for spine-driver-mobile. Data, not instructions — the blocks spine-toolkit reads to learn which tools drive an app, which surfaces they reach, and what each surface supports.
---

# spine-driver-mobile Manifest

> This skill is **data**, not instructions. spine-toolkit reads the blocks below by invoking this
> skill; there is no procedure here to follow.

Adapter for the MCP server published as `mcp-devices` (named `claude-in-mobile` before 4.0, and
still shipped under that name as the all-in-one edition). The plugin ships the server: its `.mcp.json`
runs the all-in-one edition `claude-in-mobile@4.4.1` through `npx`, so installing the driver registers
it in Claude Code and in Codex.

## Driver

The key in this plugin's `.mcp.json` is `mobile`, the one both editions print in their own install
instructions, so it leads the list; Claude Code names the plugin's server
`plugin_spine-driver-mobile_mobile`, which follows. The package names are for anyone who registered the
server by hand under those instead.

namespace = mobile, plugin_spine-driver-mobile_mobile, mcp-devices, claude-in-mobile

## Targets

ios-simulator
android-emulator
android-device
macos
browser

## Capabilities: ios-simulator

launch stop install
ui_tree find assert screenshot logs
tap type swipe key
deeplink permissions location
a11y_audit visual_baseline
record_replay multi_device

## Capabilities: android-emulator

launch stop install
ui_tree find assert screenshot logs
tap type swipe key
deeplink permissions location webview
network_conditions
a11y_audit visual_baseline performance
record_replay multi_device

## Capabilities: android-device

launch stop install
ui_tree find assert screenshot logs
tap type swipe key
deeplink permissions webview
network_conditions
a11y_audit visual_baseline performance
record_replay multi_device

## Capabilities: macos

launch stop
ui_tree find assert screenshot logs
tap type swipe key
viewport
a11y_audit visual_baseline performance
record_replay

## Capabilities: browser

launch stop
ui_tree find assert screenshot
tap type swipe key
deeplink
visual_baseline
record_replay

## Procedure

**Physical iOS devices are not a target.** The server discovers them and can touch and read the
screen through WebDriverAgent, but every app-lifecycle path — launch, stop, install, deep link,
permissions, hardware keys, logs — is `xcrun simctl` or AppleScript aimed at Simulator.app, and
reaches simulators only. A run that cannot start the app under test is a run for a human, so the
surface is left undeclared and resolves to `unavailable` rather than to a half-drivable `ok`.

What this table declares is what the server can do when fully installed, not what a given machine
has. The server loads its platforms (android, ios, web, desktop, …) at startup, from the first of:
the `MCP_DEVICES_PLATFORMS` variable, the machine's `~/.mcp-devices/config.json`, and — only when
neither exists — every platform of the all-in-one edition this plugin ships. A machine set up once
for one platform keeps only that one, for every edition. `mcp-devices platforms` lists what is
enabled.

**A platform the server did not load is a setup step, not a missing capability.** When a tool
answers `Platform '<name>' is not installed`, stop driving that surface and tell the user the one
command that enables it, and that the MCP client must be restarted after it:
`mcp-devices install <name>`. Until then the surface is `unavailable` — not an error.

Reach for the text tree before a screenshot — it is an order of magnitude cheaper and answers most
of what a screenshot is reached for.

**Taps the app reads as one sequence go in one `flow` batch, with hints off on each tap.** A hidden
menu opened by a burst of taps is not recognised otherwise: separate `input` calls, or the UI tree
fetched for hints after each tap, leave gaps longer than the app's recognition window.

**Raw `x`/`y` given to `input` are pixels of the last full screenshot of that device**, scaled by
its preset; before one, or after a diff capture, they are device coordinates, as the `ui` tree and
find report them. Read coordinates off the screenshot you tap right after, or tap by text, id or label.

Choose the target from what is actually attached rather than pinning a device: a manifest that names
one device is wrong on every machine but its author's.

Leave the app stopped when the run ends, and do not reset device defaults unless the task asked.

WebDriverAgent stays up between calls and no tool closes it: the server stops it only when it exits.
Its `xcodebuild … test-without-building` process is safe to stop after a run — the next iOS call starts
a new one — and the orchestrator offers exactly that when a stage leaves it running.
