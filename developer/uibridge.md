# UIBridge

UIBridge is a [Model Context Protocol (MCP)](https://en.wikipedia.org/wiki/Model_Context_Protocol) server for automating and inspecting GNUstep applications.
You (or your AI agents) can use it to programmatically explore, control, and test real UI state and user-flows.

Typical workflow: Discover running apps → attach_app → get_full_tree / find_widgets → invoke_selector or simulate X11 input → watch_app to monitor changes.

## UIBridge for AI agents

What agents can do
- **Discover and attach to running apps** (list_running_apps → attach_app).
- **Inspect UI state** (get_root, get_full_tree, get_object_details) and map UI widgets to absolute screen coordinates.
- **Find and wait for widgets or text** (find_widgets, wait_for_object, wait_for_text) even when modal dialogs are present.
- **Drive apps** at multiple layers: invoke_selector / invoke_menu_item (high level), and x11_mouse_move / x11_click / x11_type (low level).
- **Monitor live changes** with notifications/ui_event (watch_app) to let agents react to UI events and adjust plans.
- **Run diagnostics** and **deep-debugging** (list_files, read_file_content, lldb_exec).

How this integrates with VS Code and agents
- VSCode extensions call the server via MCP (tools/call) so an agent can query tools, call them, and consume JSON responses.
- Agents use the server as a grounding layer: plan (LLM) → probe UI (UIBridge) → choose action → execute → observe results → iterate.
- Prompts and prebuilt “assistant” prompts (prompts/list, prompts/get) provide structured guidance for common tasks (inspect_ui, automate_task, debug_ui_flow).

Typical agent workflow
1. tools/call list_running_apps → pick target PID or service name.
2. tools/call attach_app → set server currentPID.
3. tools/call get_full_tree → analyze UI structure and find targets (find_widgets).
4. Execute action (invoke_selector or x11_click / invoke_menu_item).
5. tools/call watch_app (enabled) → receive notifications/ui_event and confirm outcome.
6. If needed, run lldb_exec for deep debugging or read_file_content for config/log checks.

:::{note}
`UIBridgeServer` and target application must run under compatible user privileges (root vs user affects attachability).
:::

Use cases
- End‑to‑end UI testing and regression checks driven by agents.
- Autonomous task execution (e.g., “Open X, perform Y, verify Z”).
- Accessibility and layout audits using absolute coordinates and full tree inspection.
- Assisted debugging workflows inside VS Code with an LLM orchestrator.

##  Installation in VSCode

* Click on the "tools" icon in the text box that says "Describe what to build next" (where you enter text prompts)
* Click on the MCP icon in the "Condfigure Tools" popover (the leftmost of the three icons in the upper right hand corner of the popover)
* Select "Command (stdio)"
* Enter `/System/Library/Tools/UIBridgeServer`
* Enter a name, e.g., `uibridge`
* On he left-hand side of VSCode, in the Extensions tab, you should now see the `uibridge` as an installed MCP server
* You can start, restart, view logs from there using the cog icon

Alternatively, you can add `~/.config/Code/User/mcp.json`:

```
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
