# 📝 AI Notes (MCP Server)

This is a simple yet powerful **MCP server** that allows an AI like **Claude** to read, write, and summarize sticky notes using the [Model Context Protocol (MCP)](https://github.com/modelcontextprotocol).

---

## Features

- Add new notes
- Read all notes
- Fetch the latest note
- Ask the AI to summarize the current notes

---

## How it works

This project uses [`FastMCP`](https://pypi.org/project/fastmcp/) to expose a local server that defines:

- **Tools**: `add_note`, `read_notes`
- **Resource**: `notes://latest`
- **Prompt**: `note_summary_prompt`

These are exposed via the MCP protocol so that an AI assistant (like Claude) can call them programmatically.

---

## File Structure

```bash
.
├── server.py           # Main MCP server
├── notes.txt           # File to store notes
├── file_funct.py       # (Optional) File functions if modularized
├── requirements.txt    # Dependencies
└── README.md           # You're here!
```

---

## Clone the repository:
   ```bash
   git clone https://github.com/<your-username>/weather-mcp.git
   cd weather-mcp
   ```

---

## Set up the environment

To get started, make sure you have uv installed and that your Python version is 3.11.

And you also need FastMCP and httpx are available.

Then run the following steps:
- mkdir <folder_name>
- cd <folder_name>
- uv venv --python=3.11
- source .venv/bin/activate
- uv init --no-workspace
- uv add "mcp[cli]" httpx

⚠️ Note: These steps require Python version 3.11 to work properly. You may need to install it manually if your system uses a different version.

---

## How to add your MCP Tool to Claude
1. Open Claude, go to Settings → Developer.
2. Click on Edit Config.
3. From the list of files, open claude_desktop_config.json.
4. Add the following JSON structure to configure the MCP server:
```JSON
{
  "mcpServers": {
    "AI Notes": {
      "command": "<path-to-uv>",
      "args": [
        "run",
        "--with",
        "mcp[cli]",
        "mcp",
        "run",
        "<path-to-your-main.py>"
      ]
    }
  }
}
```

- Replace <path-to-uv> with the full path to your uv binary.

Example (on macOS):

"/Users/yourname/.local/bin/uv"

- Replace <path-to-your-main.py> with the full path to your main.py file.

Example:

"/Users/yourname/Projects/weather-mcp/main.py"

5. Save the config file. Claude will now recognize and run your local MCP.

---

## Adding multiple MCP Tools
If you want to run more than one MCP at the same time, structure your config like this:

```JSON
{
  "mcpServers": {
    "Weather": {
      "command": "<path-to-uv>",
      "args": [
        "run",
        "--with",
        "mcp[cli]",
        "mcp",
        "run",
        "<path-to-weather-main.py>"
      ]
    },
    "Sticky Notes": {
      "command": "<path-to-uv>",
      "args": [
        "run",
        "--with",
        "mcp[cli]",
        "mcp",
        "run",
        "<path-to-notes-main.py>"
      ]
    }
  }
}
```

📌 Tip: If you're unsure about the exact path, use the which uv or realpath main.py command in your terminal.