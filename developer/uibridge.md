# UIBridge

UIBridge is a [Model Context Protocol (MCP)](https://en.wikipedia.org/wiki/Model_Context_Protocol) server for automating and inspecting GNUstep applications.
You (or your AI agents) can use it to programmatically explore, control, and test real UI state and user-flows.

Typical workflow: Launch app -> list apps -> inspect UI tree -> drive app via selectors or X11 input.

## How it works

UIBridge consists of three parts:

- **Eau Theme UIBridge Service**: A component within the Eau theme that runs inside each target application's memory space, providing access to the Objective-C runtime via Distributed Objects.
- **UIBridge Server**: An MCP-compliant coordinator (`UIBridgeServer`) that manages the lifecycle of target applications and proxies requests between MCP clients and applications.
- **Common Interface**: A shared protocol definition that ensures type-safe communication and consistent serialization of Objective-C objects.

## Installation in VSCode

* Click on the "tools" icon in the text box that says "Describe what to build next" (where you enter text prompts)
* Click on the MCP icon in the "Configure Tools" popover (the leftmost of the three icons in the upper right corner of the popover)
* Select "Command (stdio)"
* Enter `/System/Library/Tools/UIBridgeServer`
* Enter a name, e.g., `uibridge`
* On the left-hand side of VSCode, in the Extensions tab, you should now see the `uibridge` as an installed MCP server
* You can start, restart, view logs from there using the cog icon

Alternatively, you can add `~/.config/Code/User/mcp.json`:

```json
{
    "servers": {
        "uibridge": {
            "type": "stdio",
            "command": "/System/Library/Tools/UIBridgeServer",
            "args": []
        }
    },
    "inputs": []
}
```

## Core tools

### Application lifecycle

- **launch_app**: Launch a GNUstep application. The application will automatically register its UIBridge service via Distributed Objects if it uses the Eau theme.
- **list_apps**: List all available GNUstep applications found in standard system directories.

### UI introspection

- **get_root**: Retrieve the root objects of the target application (NSApp and open windows).
- **get_object_details**: Fetch detailed state for a specific object (class name, frame, titles, children).
- **list_menus**: Retrieve the entire menu hierarchy of the application.

### Interaction and control

- **invoke_selector**: Invoke an Objective-C selector on a remote object (`object_id`, `selector`, optional `args`).
- **invoke_menu_item**: Trigger the action associated with an `NSMenuItem`.
- **x11_mouse_move**, **x11_click**, **x11_type**: Simulate hardware input events directly via the X server.
- **lldb_exec**: Execute an arbitrary command via LLDB attached to the target process. Use for deep state analysis or memory inspection.

### X11 window queries

- **x11_list_windows**: Query the X11 window tree.
- **x11_window_info**: Query geometry and properties of a specific X11 window.

## Logging

All internal diagnostic logs are written to `/tmp/uibridge.log`. You can monitor them with:

```console
$ tail -f /tmp/uibridge.log
```

## Building from source

```console
$ cd Server
$ make
$ sudo make install
```

The server installs to `GNUSTEP_SYSTEM_TOOLS` (default: `/System/Library/Tools`).
