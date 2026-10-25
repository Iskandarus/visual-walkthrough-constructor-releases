# What the statistics of Walkthrough Constructor hold

Walkthrough Constructor can send anonymous statistics of how it is used to Google Analytics, so that its maker learns which systems and versions it runs on, which parts of it are used and which are never found, how large the stories drawn in it grow, how often the checks and the route search run out of room, and which errors happen on machines nobody reports from. This page lists everything it can send and says what it never sends.

## Nothing without your yes

The app asks once whether it may send statistics: on its first start, after the welcome, or on the first start of the update that brings the question, after what changed. **Send** and **Don't send** are offered alike and neither is chosen for you; closing the question any other way is a no. Until the answer is yes nothing is gathered, nothing is sent and no id exists.

The answer can be changed at any time in **Settings -> Send anonymous statistics**. Switching it off stops the gathering at once and forgets the anonymous id. The command line and MCP clients can read the answer but never set it: it is yours alone.

A build made for testing marks every event with `debug_mode`, which keeps it out of the reports; no such build is published.

## The anonymous id

Every request carries a `client_id`: a random identifier, a version 4 UUID, made on your machine when you say yes and kept in the app's settings. It is tied to no account, email, machine or address. A no forgets it, and a later yes makes a new one, which Google counts as a new installation. There is no user id of any kind.

## When it is sent

Events are counted in memory and sent every five minutes while the app is open, when its window loses the focus or is hidden, and as it closes. Events alike are sent once, with how many times they happened. A send that fails is dropped: nothing is kept on disk and nothing is tried again.

Every event carries, beside its own parameters:

| Parameter | Value |
| --- | --- |
| `count` | How many times it happened since the send before |
| `session_id` | When this start of the app began, in whole seconds since 1970, the same for every event of one start |
| `engagement_time_msec` | On the first event of a send, how long the window was in focus since the send before; `1` on the rest |

## The events

| Event | When | Parameters |
| --- | --- | --- |
| `app_start` | The app started, and the moment of a yes | `start`: `after_install`, `after_update` or `again` |
| `command` | A command of the editor was pressed | `id`: the name the command is declared under in the app, such as `undo`, `fit-page` or `run-random`; a start from a chosen entry is always `run-start-from` and the switch of a tag always `view-tag`, so that no name of a scene or a tag is sent. `by`: `button`, `menu`, `key`, `find` or `command_line` |
| `checks_walk` | A walk of the checks ended | `scope`: `whole` or `part`; `end`: `finished`, `room`, `memory`, `depth`, `gathered`, `called_off` or `failed`; `states` and `seconds` in ranges; `room`, the room it was given |
| `route_search` | A search for a way to a scene ended | `search`: `quick` or `every`; `asked`: `window` or `command_line`; `end`: `found`, `no_way`, `room`, `memory`, `called_off` or `failed`; `stage`, which part of the quick search answered: `geometry`, `closure`, `settled`, `rollout`, `dive` or `none`, and `walk` for a search of every way; `states` and `seconds` in ranges; `room` |
| `project_open` | A project was opened | `scenes`, `lines`, `variables`, `pages` and `functions`, each in a range |
| `app_error` | The app met an error | `kind`: `framework`, `platform`, `save`, `walk` or `cloud`; `type`: the type of what was thrown, such as `StateError`, or the kind of what a cloud refused; `at`: the file and line of the app's own code it was thrown from, such as `ui/editor/scene_card.dart:42`, where there is one; `code`: the system's number for a save that failed |

A number that could tell a story apart is sent as a range, never as itself: a size among the powers of four (`0`, `1-3`, `4-15`, `16-63`, `64-255`, `256-1023`, `1024+`), a number of states among the powers of ten (`0`, `1-9`, `10-99` and on to `10000000+`), and seconds on a scale of one, three and ten (`0`, `1-2`, `3-9`, `10-29`, `30-99`, `100-299`, `300-999`, `1000+`). The room of a search is sent as it is set in the settings.

## What every request says of the installation

| Property | Value |
| --- | --- |
| `app_version` | The version and build of the app, such as `1.4.2+57`, or `unknown` |
| `system` | `windows`, `macos`, `linux`, `android` or `ios` |
| `system_version` | The numbers of the system's version, such as `10.0.26300`, or `unknown` |
| `window_language` | The language of the window, such as `en` or `ru` |
| `language_chosen` | `yes` where the language was chosen in the settings, `no` where it follows the system |
| `search_room` | The room for a search set in the settings, in states |
| `command_line` | `on` or `off`, whether the command line may reach the window |
| `mcp_clients` | Which MCP clients are set up to start the app's server, `claude-desktop`, `cursor` and `vscode` joined by commas, or `none` |
| `cloud` | The kind of cloud the projects are kept in, `firebase` or `drive`, or `none` |
| `theme` | `system`, `light` or `dark` |

## What is never sent

- A name or any word of a project: titles, descriptions, variables and their values, pages, tags, styles, notes.
- An id of anything in a project, a path, or the name of a file.
- An account, an email, or the name of a machine.
- What a cloud said, or the message of an error, which may quote what it was handed.
- Anything you typed.

The words of every event are kept in one file of the app's source, and a test there fails when the app could send an event, a parameter or a property this page does not name.

## What Google receives with any request

A request reaches Google from the network address of your connection, as every request on the internet does, with the plain headers any program sends. The app sends no address and no place of its own: neither `ip_override` nor `user_location`, the two fields by which the [Measurement Protocol](https://developers.google.com/analytics/devguides/collection/protocol/ga4/reference#user_location) is told where an event came from, and no details of the device beyond the system and its version above. Collection of granular location and device data is switched off on the property as well.

How Google itself uses data from apps that use its services is described in [How Google uses information from sites or apps that use our services](https://policies.google.com/technologies/partner-sites).

## How long it is kept

Events, and everything tied to an anonymous id, are kept for two months, the shortest [Google Analytics allows](https://support.google.com/analytics/answer/7667196). The totals of the standard reports, which count events and installations and name none of them, stay as long as the property does.

The property keeps as little as it can: Google signals are off, no Google Ads account or other product is linked to it, and none of its data is shared with Google's other products and services.

## Questions

Ask in the [issues of this repository](https://github.com/Iskandarus/visual-walkthrough-constructor-releases/issues).
