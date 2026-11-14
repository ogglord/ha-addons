# Changelog

## 0.3.7

- Remote Control shows in claude.ai/code's environment list as
  "agent · homeassistant", not "work · <hash>-ha-agent".
- The work folder moved from `/data/work` to `/data/agent`, along with its
  files and Claude's session history. (Its name is the first half of that
  label.)

## 0.3.5

- The sessions Remote Control starts are named after `remote_control_name`,
  not the container's hostname (`<hash>-ha-agent`).
- `remote_control_name` now defaults to "Home Assistant Agent". An existing
  install keeps its saved name: change it in Configuration.

## 0.3.4

- The Web UI showed Remote Control and the peer listener as not running when
  they were. It now reads their state correctly, and says so when it cannot.

## 0.3.3

- Remote Control no longer floods the add-on's log: it redraws its status
  twice a second, and each line now reaches the log once per 10 minutes. A
  change, such as a new session, still shows at once.

## 0.3.2

- A Web UI: **Open Web UI** shows a status page, read only, like Hemma hub's.
  It covers Claude Code (does it answer, is it logged in, the setup-token's
  age), Remote Control (running, starting, or waiting for the login), the
  peer listener (connected to the hub, gate verified) and `peer doctor`.
  While a login waits for your code, the page says so.

## 0.3.1

- Fixes Remote Control restarting in a loop with "Enable Remote Control?
  (y/n) ... got no answer". With no terminal to answer that one-time
  question, the add-on now answers it itself before Remote Control starts.
- The work folder is trusted right after the first login, so no restart is
  needed after it.

## 0.3.0

- Renamed Hemma Agent (was HA Agent): it is now part of the Hemma family,
  built from the same repository as Hemma hub. The slug stays `ha_agent`, so
  this is an ordinary update: options, login and data are kept. Its
  notifications now start "Hemma Agent:".
- The image is now `ghcr.io/ogglord/hemma-agent-amd64` (was
  `ha-peer-runner-amd64`).

## 0.2.4

- peer from homelab `d9d61c1`: `peer --version` prints the homelab commit,
  and the listener reports it to the hub, so homelab's Peer page shows
  whether this host is current or behind.
- The peer skill is shorter, and its Home Assistant copy is gone: this host
  reads the same skill as every other.

## 0.2.3

- The `peer` skill ships in the image from the same homelab commit as the
  `peer` binary and is linked into Claude's user skills, so the notes Claude
  reads match the CLI it calls and update with it.

## 0.2.2

- Claude's notes on this host say that `ha-ws-client --json` wraps its
  output in `data`.

## 0.2.1

- Tapping Logga in no longer loses the way back: a second notification,
  "HA Agent: skicka koden", follows with only the Skicka kod reply.

## 0.2.0

- Log in without SSH: when Claude is logged out, HA Agent sends the sign-in
  link through Hemma, and you reply with the code (Hemma hub 0.1.32, app
  0.2.15). Or fire `ha_agent_login` with the code from Developer tools.
- `claude_token`: a `claude setup-token` token for peer runs, with a warning
  through Hemma a month before it expires.
- `notify_script`: where those notifications go (`hemma_notify_admin`).

## 0.1.3

- Renamed HA Agent (slug `ha_agent`, was `peer_runner`). Install it anew:
  HA treats a new slug as a new add-on.

## 0.1.2

- `ha-ws-client` (schoolboyqueue/home-assistant-blueprints): HA's WebSocket
  API from the shell, for states, registries, history and automation traces.
- A CLAUDE.md for every session that names this host's tools: `ha-ws-client`,
  `ha`, the config at `/homeassistant` and the HA YAML plugin.

## 0.1.1

- The Home Assistant config plugin (ESJavadex/claude-homeassistant-plugins
  2.1.4): skills for HA YAML and a check after every YAML edit, in peer runs
  and Remote Control sessions. Turn it off with `ha_plugin: false`.

## 0.1.0

- First version: the peer listener and Claude Code Remote Control, headless.
- Claude Code 2.1.323, `ha` CLI 5.7.0, peer from homelab@823df709.
