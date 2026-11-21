# Install and update Amble

[**Download the latest release**](https://github.com/AmbleCode/amble-releases/releases/latest) ·
[Release notes](https://github.com/AmbleCode/amble-releases/releases) ·
[Back to Amble](README.md)

## Windows

For a 64-bit Intel or AMD PC, download the file ending in **Setup-…-x64.exe**,
run it, and launch Amble. The release's **Windows installer** link selects
this file for you.

Current Windows builds are unsigned, so Windows may display a SmartScreen warning.
Check that your download came from this repository before deciding whether to run it.

For a portable installation, download the Windows **.zip**, extract the whole archive
to a folder, and launch the app from there.

## Linux

For a 64-bit Intel or AMD computer, choose one of these formats:

- **AppImage:** save the file to a folder you own. In its file properties, allow it
  to run as a program, then open it. Some distributions require AppImage/FUSE support.
- **.deb:** install with your distribution's package installer on Debian/Ubuntu-based systems.
- **.rpm:** install with your distribution's package installer on RPM-based systems.
- **.tar.gz:** extract the whole archive and launch the included app.

AppImage is the format used by Amble's Linux in-app updater. Other packages update manually.

## macOS

For an **Apple silicon** Mac (M1 and newer), download the **.dmg** from the
latest release, open it, and drag **Amble** into **Applications**. Launch
Amble from Applications. The 0.1.4 Mac download is Developer ID signed, notarized,
and stapled.

A **.zip** is also available. No Intel Mac package is currently published.
The 0.1.4 release includes the macOS update feed; older preview releases may
require a manual download.

## iOS and Android

Phone clients are in testing. Distribution links will appear in the
[availability guide](https://www.amblecode.com/docs/release-status.html#ios)
when a public or testing channel is available.

## Command-line interface

The [Amble CLI](https://www.npmjs.com/package/@amble/cli) lets you
control projects, workspaces and agents from a terminal. It also includes a host
daemon for a computer that runs Amble without the desktop app. The daemon runs
projects and coding agents; a connected Amble client provides the graphical
workspace. Downloading the desktop app and installing the npm CLI are separate
installation choices.

The website also has a [CLI quickstart](https://www.amblecode.com/docs/cli-quickstart.html),
[command reference](https://www.amblecode.com/docs/cli.html) and
[Amble workflow examples](https://www.amblecode.com/docs/cli-workflows.html).

### 1. Install and check the CLI

Install **Node.js 22.12 or newer** with npm, then open a terminal and run:

```sh
node --version
npm --version
npm install --global @amble/cli@beta
amble --version
amble --help
```

The current npm version is **0.11.0-beta.3**. To select it explicitly, use
`npm install --global @amble/cli@0.11.0-beta.3`. The npm CLI and desktop app
have separate version numbers; this CLI accompanies desktop **0.1.6**.
`amble`, `amble-foundation` and `paseo` are equivalent executable names, so help
output may use `amble-foundation` even when you entered `amble`.

npm installs the runtime dependencies automatically. Coding providers still need
their own installation and account or model setup.

### 2. Start or select your host

If Amble desktop is already running, use `amble status` to check the selected
host. Manage a desktop-owned host from the desktop app.

For a standalone npm host:

```sh
amble daemon start
amble status
```

The daemon runs in the background after the start command exits. Start reports
its PID, endpoint and log location. Status shows whether the local daemon is
running and whether its connection is reachable. Initial provider discovery can
take a moment; retry status if the host is still starting.

Commands use the local home `~/.amble` by default. Use the same
`--home` on every command when selecting another local host. For example:

```sh
amble daemon start --home PATH_TO_AMBLE_HOME
amble status --home PATH_TO_AMBLE_HOME
```

Replace `PATH_TO_AMBLE_HOME` with a short absolute path outside your project
repository, and use that same path on subsequent commands. `--host` selects an
explicit remote endpoint or pairing URL; project paths then refer to the selected computer. See
`amble --help` for supported connection formats.

### 3. Set up a provider and work in an existing project

Check which providers the selected host can use:

```sh
amble provider ls
amble provider diagnostic codex
```

The diagnostic example selects Codex; substitute the provider you use. Install
and sign into its tool, or configure its supported model source, on the host
computer. Follow the [provider setup guide](https://www.amblecode.com/docs/providers.html).
A provider's service may require its own account, subscription or API key.

Open a terminal in an existing project folder, then run:

```sh
amble project create .
amble run --provider codex --cwd . "Explain this project without changing files."
amble ls
```

`project create` registers the current folder with the host. The example request
asks for an explanation only; choose an available provider from `amble provider ls`.
For a coding task, describe the change you want.
`amble run` normally waits for the task; add `--background` to return to the
terminal while it runs. Provider permission requests still need your response.

Replace `AGENT_ID` below with the ID shown by `amble ls`:

| What you want to do               | Command                                       |
| --------------------------------- | --------------------------------------------- |
| List agents                       | `amble ls`                                     |
| Follow a task's activity          | `amble logs AGENT_ID --follow`                 |
| Send a follow-up request          | `amble send AGENT_ID "Explain the next step."` |
| Interrupt a running agent         | `amble stop AGENT_ID`                          |
| List registered projects          | `amble project ls`                             |
| List workspaces                   | `amble workspace ls`                           |
| Get machine-readable agent output | `amble ls --json`                              |
| Explore a command's options       | `amble run --help`                             |

### 4. Connect an Amble client

Run `amble pair` on the host and review its prompts to approve a trusted owner
device. On that device, open a supported Amble client, choose **Connect** in the
Library, and use the pairing link or QR code. Keep the host awake and connected
while accessing its projects remotely.

Owner pairing grants management access. Use Amble's app-sharing controls when
inviting someone to try only an app. Mobile clients have separate testing and
distribution channels; see [mobile availability](https://www.amblecode.com/docs/release-status.html#ios).

### Amble features from the terminal

These examples use the current project folder and Codex. Substitute a provider
available on your selected host. Replace uppercase IDs with values from the
corresponding `ls` command.

#### Use a provider's own mode

Some providers offer modes of their own, such as a plan mode. Choose one on the first
request with `--mode`. `amble provider ls` shows the modes each provider supports:

```sh
amble run --provider PROVIDER --cwd . --mode MODE "Plan a fix for the login error before editing."
```

Provider modes are set by the provider and its permission policy; they do not grant extra
access. Agents decide when to hand independent work to helper agents.

#### Keep parallel work in separate checkouts

From a Git repository, create an agent in a new managed worktree and branch:

```sh
amble run --provider codex --cwd . --new-workspace worktree --new-branch fix-login --background "Fix the login validation and run its tests."
amble workspace ls
```

To start another agent in that same checkout, use its workspace ID:

```sh
amble run --provider codex --workspace WORKSPACE_ID "Review the login fix and its tests."
```

This keeps edits separate from your original checkout. Review and merge the
branch through your normal Git workflow; starting an agent does not merge it.

#### Give an agent screenshot context

With an image-capable provider, send a local image to an existing agent from
`amble ls`:

```sh
amble send AGENT_ID --image screenshot.png "Explain the layout issue shown in this screenshot."
```

Replace `screenshot.png` with an existing PNG or JPEG path. Review the CLI's
image-transfer prompt before sending it to the model provider. Repeat `--image`
for more than one image. Hosts requiring this review reject images in a new
agent's initial `run` request; use `send` for the reviewed transfer. Image support
depends on the selected provider and model.

#### Prepare and control your app preview

Inspect local project metadata, then review a proposed web service on an explicit
port. These two commands read the project without launching it:

```sh
amble preview inspect . --json
amble preview plan . --port 5173 --json
```

If the proposal is suitable and your project has no `paseo.json`, explicitly
create the configuration with `amble preview configure . --port 5173 --json`.
It creates a new file and refuses to overwrite an existing configuration. These
preview setup commands operate on local files; they do not support `--host`.
Install your project's dependencies before running its services.

List the selected workspace's scripts, then start or stop one by name:

```sh
amble script ls --cwd .
amble script start SCRIPT_NAME --cwd .
amble script stop SCRIPT_NAME --cwd .
```

Replace `SCRIPT_NAME` with a listed name; newly generated preview configurations
use `preview`. Select `--workspace WORKSPACE_ID` instead of `--cwd .` when the
directory has multiple workspaces. Stopping an agent does not stop the preview,
and stopping a preview does not stop the agent. Open and interact with the app
through a supported Amble client; local configuration is not public deployment
or an app-sharing grant.

#### Run recurring tasks while your host is available

For example, schedule a weekday project review at 9 a.m. Chicago time, limited
to five runs:

```sh
amble schedule create "Review this project's open work and summarize the next steps without editing files." --provider codex --cwd . --cron "0 9 * * 1-5" --timezone America/Chicago --max-runs 5
amble schedule ls
amble schedule logs SCHEDULE_ID
amble schedule pause SCHEDULE_ID
```

Use `amble schedule resume SCHEDULE_ID` to resume it, or
`amble schedule run-once SCHEDULE_ID` to request an extra run. The daemon must be
running and the provider ready; scheduled work uses that provider's usual
permissions and may incur its normal service costs.

#### Review permissions and trusted plugins

See pending agent permission requests with `amble permit ls`. Review the agent,
request ID and requested operation, then choose either
`amble permit allow AGENT_ID REQUEST_ID` or
`amble permit deny AGENT_ID REQUEST_ID`.

For host plugins, list the current configuration and check available updates:

```sh
amble plugin ls
amble plugin update --all --check
```

Plugin authors can scaffold a project with
`amble plugin init ./my-plugin --template lifecycle` and statically check it with
`amble plugin check ./my-plugin --json`. Scaffolding and checking do not activate
the plugin. Use `amble plugin --help` for explicit installation and lifecycle
commands. Plugins run trusted code with the host account's access; their client
surfaces appear in connected Amble apps. A successful static check does not prove
runtime behavior or authorize installation.

### Update or stop a standalone host

Finish running tasks before stopping the host. To update a standalone npm
installation while keeping its existing home and project folders:

```sh
amble daemon stop
npm install --global @amble/cli@beta
amble daemon start
amble --version
amble status
```

Include your chosen `--home` on the daemon/status commands if you use a custom
home. For a desktop-managed host, use the desktop app's update and restart
controls. The npm command updates the CLI and its runtime dependencies; desktop
updates follow the [application update instructions](#updating).

### Troubleshooting and current limits

| Symptom                                   | What to check                                                                                                                                                                                                             |
| ----------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `amble` is not found after installation    | Reopen the terminal and check that npm's global executable directory is on PATH. On Windows it is the npm global prefix; on macOS/Linux it is the prefix's `bin` directory. Find the prefix with `npm config get prefix`. |
| Node or npm is not found                  | Install a supported Node.js version with npm, reopen the terminal and check `node --version` and `npm --version`.                                                                                                         |
| The selected daemon is unreachable        | Start the standalone daemon, allow startup to finish, and retry `amble status`. Check that all commands use the same `--home` or the intended `--host`; start/status report the daemon log path.                           |
| A provider is unavailable                 | Run `amble provider diagnostic codex` with your provider's name, then complete its installation and account setup on the host.                                                                                             |
| A custom daemon listen option is rejected | Configure it with `amble daemon config set daemon.listen VALUE`, then start or restart the standalone host. Use `amble daemon config set --help` for values and include your custom `--home` if needed.                     |

The initial npm release is verified on **Windows x64 with Node.js 24**, including
the Windows image-input and process helpers. Installed macOS/Linux qualification
is pending, and macOS native helpers are absent from this npm build. Keep custom
Windows daemon homes short: the image-file helper does not support full image or
temporary paths of **260 characters or more**. The VS Code companion is not yet
published; use the CLI or the available Amble clients.

## Your first project

Open a project folder in Amble, connect a coding provider or local model, and set up
the project's preview command. Your host needs the dependencies and development
tools that the project uses. Local-model hardware requirements depend on the model
and model runner you choose.

Local projects and local-network workflows work without an Amble cloud account.
Keep your host awake and connected when accessing it from another device.
[Follow the setup guide](https://www.amblecode.com/docs/quickstart.html).

## Updating

**macOS, Windows installer, and Linux AppImage:** builds with updates enabled check the
release feed from within Amble. Open **Settings → About → App updates**, choose
**Check**, then use **Update** when a newer version is available. Automatic download
can be enabled there; applying the update restarts the app. Let running tasks finish
and save your work first.

**Portable ZIP, .deb, .rpm, .tar.gz, and builds without in-app updates:** download
the newer package from [Releases](https://github.com/AmbleCode/amble-releases/releases)
and replace or upgrade the app using the same installation method. Read the release
notes before switching versions or installation types.

Amble stores its saved app state separately from the application files. Keep that
existing profile and your project folders when updating; do not delete or reset them.
Back up valuable work before an early-access upgrade. A platform's completed upgrade
checks and any migration steps belong in that release's notes.

## Choosing files and checking downloads

Current package names begin with **Amble**. These are Amble downloads;
the architecture labels `x64`, `x86_64`, and `amd64` all identify 64-bit Intel/AMD builds.
`arm64` identifies the Apple silicon Mac build.

Use the release's labeled download links. The **.yml** and **.blockmap** files are
for the updater. GitHub's **Source code (zip/tar.gz)** links contain this public
repository's documentation, not the Amble application.

**SHA256SUMS.txt** covers the Windows and Linux packages.
**SHA256SUMS-macos.txt** covers the Mac packages. Use the manifest attached to the
same release as your download. To compare a saved file against its entry, use
PowerShell's `Get-FileHash -Algorithm SHA256` on Windows, `shasum -a 256` on
macOS, or `sha256sum` on Linux. Matching a checksum confirms the download's
bytes match the published value; it does not replace platform signing checks.

[Report an installation problem](https://github.com/AmbleCode/amble-releases/issues/new?template=bug_report.yml).
