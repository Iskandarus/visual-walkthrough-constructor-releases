# Walkthrough Constructor releases

The built files of [Walkthrough Constructor](https://github.com/Iskandarus/visual_walkthrough_constructor), a visual constructor for story game walkthroughs. The source lives in a repository of its own; this one carries the releases, so that an installed build can look for a newer one without a token.

Every release carries all five platforms:

| Platform | File |
| --- | --- |
| Windows 10/11 | `WalkthroughConstructor-<version>-win64.zip` |
| macOS 11+ | `WalkthroughConstructor-<version>-macos.zip` |
| Linux x64 | `WalkthroughConstructor-<version>-linux-x64.tar.gz` |
| Android | `WalkthroughConstructor-<version>-<abi>.apk`, or `-android.apk` for every instruction set in one file |
| iPhone / iPad | `WalkthroughConstructor-<version>-ios-unsigned.ipa` |

The Windows and Linux builds update themselves from here: the app looks once a day and the button in its settings looks now. Android installs through the system installer. The macOS and iOS builds open this page instead, because neither is signed by a paid Apple account and installing one takes a step an app cannot take on its own behalf - each release says which.

Nothing is committed here by hand. The files and the notes are written by the release pipeline of the source repository.
