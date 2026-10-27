# Walkthrough Constructor releases

The built files of [Walkthrough Constructor](https://github.com/Iskandarus/visual_walkthrough_constructor), a visual constructor for story game walkthroughs. The source lives in a repository of its own; this one carries the releases, so that an installed build can look for a newer one without a token.

Every release carries all five platforms:

| Platform | File |
| --- | --- |
| Windows 10/11 | `WalkthroughConstructor-<version>-win64-setup.exe`, or `-win64.zip` to unpack anywhere |
| macOS 11+ | `WalkthroughConstructor-<version>-macos.zip` |
| Linux x64 | `WalkthroughConstructor-<version>-linux-x64.tar.gz` |
| Android | `WalkthroughConstructor-<version>-<abi>.apk`, or `-android.apk` for every instruction set in one file |
| iPhone / iPad | `WalkthroughConstructor-<version>-ios-unsigned.ipa` |

The setup for Windows installs for the one user who runs it, without asking for an administrator, adds the app to the Start menu and to Settings, Apps, and puts the command line on PATH. Windows SmartScreen does not know the setup, which is not signed, and says "Windows protected your PC" the first time: press More info, then Run anyway.

The Windows and Linux builds update themselves from here, installed or unpacked: the app looks once a day and the button in its settings looks now. Android installs through the system installer. The macOS and iOS builds open this page instead, because neither is signed by a paid Apple account and installing one takes a step an app cannot take on its own behalf - each release says which.

Every file has a `.sig` beside it, a signature made with the project's own key, which is not kept on GitHub. Before an update is unpacked or handed to the installer, the app checks that its file was signed with that key, for that file and that version, so a file nobody signed is not installed whoever put it here.

The Windows, macOS and Linux builds carry an MCP server with the app, so Claude Desktop, Claude Code, Cursor, VS Code and other MCP clients can read the story open in it. [MCP.md](MCP.md) says where it lies and how to connect a client.

The app can send anonymous statistics of how it is used to Google Analytics, and only after the author has said yes. [STATISTICS.md](STATISTICS.md) lists everything it can send, what it never sends, and how to switch it off.

Nothing is committed here by hand. The files, the notes and these pages are written by the release pipeline of the source repository.
