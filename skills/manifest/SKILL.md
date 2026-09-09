---
name: manifest
description: Driver manifest for mobile-driver. Data, not instructions — the blocks spine-toolkit reads to learn which tools drive an app, which surfaces they reach, and what each surface supports.
---

# mobile-driver Manifest

> This skill is **data**, not instructions. spine-toolkit reads the blocks below by invoking this
> skill; there is no procedure here to follow.

Adapter for the MCP server published as `mcp-devices` (named `claude-in-mobile` before 4.0, and
still shipped under that name as the all-in-one edition). The adapter does not install the server —
install it separately and register it with your MCP client.

## Driver

Both editions print `mobile` as the key in their own install instructions, so it leads the list; the
package names follow for anyone who registered it under those instead.

namespace = mobile, mcp-devices, claude-in-mobile

## Targets

ios-simulator
ios-device
android-emulator
android-device
macos
browser

## Capabilities: ios-simulator

launch stop install reset_state
ui_tree find assert screenshot video logs
tap type swipe gesture key
deeplink background permissions alerts viewport locale
a11y_audit visual_baseline performance
record_replay multi_device

## Capabilities: ios-device

launch stop install reset_state
ui_tree find assert screenshot video logs
tap type swipe gesture key
deeplink background permissions alerts viewport locale
a11y_audit visual_baseline performance
record_replay multi_device

## Capabilities: android-emulator

launch stop install reset_state
ui_tree find assert screenshot video logs
tap type swipe gesture key
deeplink background permissions alerts viewport locale webview
network_conditions
a11y_audit visual_baseline performance
record_replay multi_device

## Capabilities: android-device

launch stop install reset_state
ui_tree find assert screenshot video logs
tap type swipe gesture key
deeplink background permissions alerts viewport locale webview
network_conditions
a11y_audit visual_baseline performance
record_replay multi_device

## Capabilities: macos

launch stop
ui_tree find assert screenshot logs
tap type key
viewport
performance

## Capabilities: browser

launch stop
ui_tree find assert screenshot
tap type key
deeplink
performance

## Procedure

The modular edition defaults to no platform plugins loaded, so what this table declares is what the
server can do when fully installed, not what a given machine has. Platform plugins (android, ios,
web, desktop) are loaded at server startup. Check which are installed at runtime via the shell:
`mcp-devices platforms` lists them. A surface you declare but that is absent on a machine narrows
what this table promises and is not an error — the server will report "unavailable" for that surface.

Reach for the text tree before a screenshot — it is an order of magnitude cheaper and answers most
of what a screenshot is reached for.

Choose the target from what is actually attached rather than pinning a device: a manifest that names
one device is wrong on every machine but its author's.

Leave the app stopped when the run ends, and do not reset device defaults unless the task asked.
