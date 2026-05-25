# EasyEDA MCP Bridge — Installation Guide

Connect Claude AI to EasyEDA Pro. Ask Claude to place components, route traces, run DRC checks, search the JLCPCB library, export Gerbers, and more — all directly inside your PCB editor.

---

## What's included

| File | Purpose |
|------|---------|
| `mcp-bridge.eext` | EasyEDA Pro extension (install once) |
| `easyeda-mcp-macos-arm64` | MCP server — macOS Apple Silicon |
| `easyeda-mcp-win-x64.exe` | MCP server — Windows x64 |
| `easyeda-mcp-linux-x64` | MCP server — Linux x64 |

---

## Prerequisites

- [EasyEDA Pro](https://pro.easyeda.com) (desktop app)
- [Claude Desktop](https://claude.ai/download) or [Cursor](https://www.cursor.com)

---

## Step 1 — Install the EasyEDA extension

1. Open **EasyEDA Pro**
2. Go to **Settings → Extensions → Extensions Manager**
3. Click **Load Extension** and select `mcp-bridge.eext`
4. Enable the extension — it will activate automatically on every startup

The extension opens a local WebSocket connection on `ws://localhost:18601` to communicate with the MCP server.

---

## Step 2 — Place the server binary

Copy the binary for your platform somewhere permanent, for example:

- **macOS:** `~/Applications/easyeda-mcp-macos-arm64`
- **Windows:** `C:\Users\<you>\easyeda-mcp-win-x64.exe`
- **Linux:** `~/.local/bin/easyeda-mcp-linux-x64`

On **macOS/Linux**, make it executable:
```bash
chmod +x ~/Applications/easyeda-mcp-macos-arm64
```

On **macOS**, you may need to allow it the first time:
> System Settings → Privacy & Security → scroll down → click **Allow Anyway**

---

## Step 3 — Configure Claude Desktop

Open `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS) or `%APPDATA%\Claude\claude_desktop_config.json` (Windows) and add:

**macOS:**
```json
{
  "mcpServers": {
    "easyeda-pro": {
      "command": "/Users/<you>/Applications/easyeda-mcp-macos-arm64"
    }
  }
}
```

**Windows:**
```json
{
  "mcpServers": {
    "easyeda-pro": {
      "command": "C:\\Users\\<you>\\easyeda-mcp-win-x64.exe"
    }
  }
}
```

**Linux:**
```json
{
  "mcpServers": {
    "easyeda-pro": {
      "command": "/home/<you>/.local/bin/easyeda-mcp-linux-x64"
    }
  }
}
```

Replace `<you>` with your actual username.

---

## Step 3 (alternative) — Configure Cursor

Open Cursor settings, go to **MCP**, and add a new server:

- **Name:** `easyeda-pro`
- **Command:** path to the binary (same as above)

---

## Step 4 — Start using it

1. Launch **EasyEDA Pro** (extension auto-connects)
2. Launch **Claude Desktop** (or Cursor)
3. Open a PCB or schematic in EasyEDA Pro
4. Ask Claude:

> *"Search for a 10k resistor in the JLCPCB library and place one at (100, 100)"*
> *"Route a trace from pad 1 of R1 to pad 2 of C1"*
> *"Run a DRC check and tell me what errors exist"*
> *"Export the Gerber files"*

Claude will use the EasyEDA tools automatically.

---

## Troubleshooting

**Claude says "no tools available" or the MCP server doesn't appear:**
- Make sure the binary path in the config is absolute (no `~`)
- Restart Claude Desktop after editing the config

**Extension not connecting:**
- Confirm the extension is enabled in EasyEDA Pro → Extensions Manager
- Make sure EasyEDA Pro is open before asking Claude to use EDA tools

**macOS Gatekeeper blocks the binary:**
- System Settings → Privacy & Security → Allow Anyway
- Or run once in Terminal: `xattr -d com.apple.quarantine /path/to/easyeda-mcp-macos-arm64`

**Windows SmartScreen warning:**
- Click **More info** → **Run anyway**
