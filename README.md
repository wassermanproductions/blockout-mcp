# blockout-mcp

MCP (Model Context Protocol) stdio bridge for **[Blockout](https://github.com/wassermanproductions/blockout)** — the open-source previs desktop app for AI filmmaking. With this server connected, an AI agent can stage grey-box worlds, drop in choreographed crowds, animate characters, frame and move the camera with classic cinematography presets, scrub the timeline, take viewport screenshots, and drive exports — everything a human does in the app.

Zero dependencies. Node ≥ 18. One file.

## Requirements

1. **The Blockout desktop app must be running** with a project open. Get it from the [Blockout repo](https://github.com/wassermanproductions/blockout) (build from source or use a release DMG).
2. On launch, Blockout starts a **localhost-only** control server on a random port and writes a discovery file to `~/.config/blockout/control.json` (random bearer token, mode 0600, removed on quit). This bridge reads that file automatically — there is nothing to configure and no credentials to enter.

## Connect

### Hermes

Add to `~/.hermes/config.yaml` (or install from the Hermes MCP catalog once listed):

```yaml
mcp_servers:
  blockout:
    command: "node"
    args: ["/absolute/path/to/blockout-mcp/blockout-mcp.mjs"]
```

### Claude Code

```bash
claude mcp add blockout -- node /absolute/path/to/blockout-mcp/blockout-mcp.mjs
```

### Any MCP client (generic stdio config)

```json
{ "mcpServers": { "blockout": { "command": "node", "args": ["/absolute/path/to/blockout-mcp/blockout-mcp.mjs"] } } }
```

## Tools (30)

`get_state` · `list_assets` · `add_entity` · `move_entity` · `delete_entity` · `add_actor_mark` · `add_camera_mark` · `clear_camera_marks` · `set_shot` · `new_shot` · `apply_framing` · `list_camera_moves` · `apply_camera_move` · `set_track_subject` · `list_action_presets` · `apply_action_preset` · `list_sequence_styles` · `spawn_sequence` · `set_reference` · `snap_to_ground` · `set_time` · `play` · `stop` · `screenshot` · `list_presets` · `save_preset` · `apply_preset` and more — call `get_state` first; every tool description carries the scene conventions (meters; +X right, −Z forward; heading 0 faces −Z).

A typical agent session: `get_state` → `add_entity`/`spawn_sequence` → `apply_camera_move` → `screenshot` to see the result → `set_shot`/`play`.

## Security

The control server binds to 127.0.0.1 only, uses a per-launch random bearer token, and the app validates every action against a whitelist. Nothing is exposed off-machine.

## License & credit

Apache-2.0 — see [LICENSE](LICENSE). Per the [NOTICE](NOTICE) file, use, forks, and redistribution must credit **Sam Wasserman ([wassermanproductions.com](https://wassermanproductions.com))**.
