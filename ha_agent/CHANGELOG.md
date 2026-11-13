# Changelog

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
