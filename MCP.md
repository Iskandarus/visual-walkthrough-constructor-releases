# Walkthrough Constructor and MCP clients

Every Windows, macOS and Linux release of Walkthrough Constructor carries an MCP server, `vwc-mcp`, with the app. An MCP client - Claude Desktop, Claude Code, Cursor, VS Code and others - starts it, and through it reads the story open in the app - its pages, scenes and lines, what the checks found, a picture of a page - and builds and arranges it. Nothing else has to be installed.

## Before you start

The app has to be running, with **Settings -> Command line -> Let the command line reach this window** switched on. The server reaches the window only on this computer, on 127.0.0.1, and only with the token the app writes beside its settings, new every time it starts. While the app is closed or the switch is off, the server says so and how to switch it on.

## With one press

**Settings -> Command line** shows where `vwc-mcp` lies on this computer, and a button for each client it finds here: **Connect Claude Desktop**, **Connect Cursor**, **Connect VS Code**. A press adds one entry, `walkthrough-constructor`, to that client's own settings file, leaves every other entry as it was, and keeps a copy of the file as it was beside it, named `<file>.<date>-<time>.backup`. Restart the client afterwards.

## Where vwc-mcp lies

| System | Path |
| --- | --- |
| Windows | `%LOCALAPPDATA%\Programs\Walkthrough Constructor\bin\vwc-mcp.exe` after the setup, or `bin\vwc-mcp.exe` in the folder the zip was unpacked to |
| macOS | `Walkthrough Constructor.app/Contents/MacOS/vwc-mcp`, inside the app wherever you put it |
| Linux | `vwc-mcp` in the unpacked folder, or `~/.local/share/walkthrough-constructor/vwc-mcp` after `./install.sh` |

The settings show the exact path with a button that copies it. The command line, `vwc`, lies beside it, and on Windows the setup puts that folder on PATH unless its box is unchecked, so a new terminal runs both by name.

## By hand

Register it as a stdio server: the path as the command, no arguments. **Copy the entry** in the settings copies this with the right path in it, which most clients take as it is:

```json
{
  "mcpServers": {
    "walkthrough-constructor": {
      "command": "C:\\Tools\\WalkthroughConstructor\\bin\\vwc-mcp.exe",
      "args": []
    }
  }
}
```

A backslash in a JSON string is written twice, as above.

- **Claude Desktop**: Settings -> Developer -> Edit Config opens `claude_desktop_config.json`. Add the entry under `mcpServers` and restart Claude Desktop.
- **Claude Code**: `claude mcp add --scope user walkthrough-constructor -- "<the path of vwc-mcp>"`
- **Cursor**: `~/.cursor/mcp.json` (on Windows `%USERPROFILE%\.cursor\mcp.json`), under `mcpServers`.
- **VS Code**: the command **MCP: Open User Configuration** opens `mcp.json`. VS Code keeps servers under `servers` and wants the type named:

```json
{
  "servers": {
    "walkthrough-constructor": {
      "type": "stdio",
      "command": "<the path of vwc-mcp>",
      "args": []
    }
  }
}
```

## macOS

The app runs in the sandbox macOS gives it and keeps its token inside its own container. The first time a client starts `vwc-mcp`, macOS may ask whether that client may access data from other apps; allow it, or the server cannot find the app.

## After an update

The app updates the server together with itself. A client that kept the old server running goes on with it until it starts it again, so restart the client after an update.

## Building, and what is asked first

The `edit` tool takes command lines and makes all of them one step of the app's history, so one Ctrl+Z, or the `undo` tool, takes back whatever one call built. Where an edit would lose work - deleting a page with its scenes, turning scenes into notes, taking a door away - the model cannot decide it: the client shows you a question listing what would go, and only your yes lets it happen. A client that shows no such question gets it in the app's own window instead, the same one the app asks when you do it by hand, and only a press there answers it.

## Attaching a page, and the usual questions

In Claude Code in a terminal, `@` lists the pages and scenes of the story open in the app beside your files: type part of a title, pick it, and the page comes with your message as compact text, or the scene with everything it holds. Its VS Code extension lists only files there, so name the page in your message and the model reads it. `vwc://here` is what you have on screen and `vwc://findings` what the checks found. Four questions come as commands, `/walkthrough-constructor:` followed by:

- `why_unreachable <scene>` - asks for a way there and what stands in the way, and says it in words;
- `tidy_page <page>` - arranges a page until nothing is drawn badly, looking at it as it goes;
- `check_page <page>` - runs the checks and says what is wrong on the page;
- `compare_with_map <page> <link to the map>` - lists where the page differs from the map it was drawn from.

A name of several words is typed as it is: Claude Code hands a command only the first word, and the model reads the rest from what you typed. Other clients keep a server's resources and prompts where they keep them for any server.

## Phones

Not on Android or iPhone: no MCP client runs a local server there.
