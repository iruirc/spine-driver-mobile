---
name: manifest
description: Driver manifest for spine-driver-mobile. Data, not instructions — the blocks spine-toolkit reads to learn which tools drive an app, which surfaces they reach, and what each surface supports.
---

# spine-driver-mobile Manifest

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

The modular edition defaults to no platform plugins loaded, so what this table declares is what the
server can do when fully installed, not what a given machine has. Platform plugins (android, ios,
web, desktop) are loaded at server startup. Check which are installed at runtime via the shell:
`mcp-devices platforms` lists them. A surface you declare but that is absent on a machine narrows
what this table promises and is not an error — the server will report "unavailable" for that surface.

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
