# Hemma Agent

Runs the homelab `peer` listener and Claude Code on this box, with no
terminal. Its Web UI is a status page. Peer requests sent to host `homeassistant` run as
headless `claude -p` in auto mode. Claude Code Remote Control (on by default)
lets you start a session here from claude.ai/code or the Claude app.

## Before you start

- The Tailscale add-on must run in TUN mode (`userspace_networking: false`)
  and advertise `tag:ha-agent`. This add-on reaches the hub through the host's
  tailnet route; the hub knows the box by Tailscale WhoIs.
- Stop the old Claude Terminal add-on (claude-code-ha) first. Two listeners
  would both claim requests for `homeassistant`.
- Set `hub_url` in Configuration (the hub's tailnet address, port 7790).

## Logging in

Remote Control needs the claude.ai login (your Max subscription); a
`setup-token` is not enough for it. The add-on logs in by itself, with no SSH:

1. When Claude is logged out, this add-on sends a Hemma notification,
   "Hemma Agent: logga in på Claude" (Hemma Hub 0.1.32 and app 0.2.15 or
   later). The same link is in this add-on's log.
2. Tap **Logga in** and sign in on claude.ai. The page shows a code.
3. A second notification, "Hemma Agent: skicka koden", arrives. Long-press it
   (or pull the banner down), tap **Skicka kod**, paste the code and send.
   Without Hemma: Developer tools › Events, event type `ha_agent_login`,
   event data `code: <the code>`, Fire event.
4. "Claude är inloggat" follows, and Remote Control and the peer listener
   start.

A link waits two hours; after that a fresh one comes. The login lives in
`/data/home` and Claude refreshes it there, so it survives restarts, updates
and backups. You see the notification again only if the login is revoked
or expires unused.

### Runs on a setup-token (optional)

Run `claude setup-token` on your Mac and paste the token into `claude_token`.
Peer runs then use it instead of the claude.ai login, so they keep working
whatever happens to it. It lasts a year; the add-on warns you through Hemma a
month before.

## The Web UI

**Open Web UI** (or the sidebar, if you turn that on) shows, read only, and
reloads every 30 seconds:

- **Claude Code:** that it answers, the claude.ai login (and, when logged
  out, whether a login waits for your code), the setup-token's age, and
  Remote Control: running, starting, or waiting for the login, with a link
  to it in claude.ai/code when it prints one.
- **Peer:** whether the listener is connected to the hub, and its gate
  (requests run only once the canary has verified it).
- **peer doctor:** the path to the hub, this box's identity and its peer
  capability, run at most once a minute.

## Options

| Option | What it does |
|---|---|
| `hub_url` | The peer hub, `http://<tailnet IP>:7790`. |
| `allow`, `deny` | Tool rules in Claude Code's permission syntax. Empty uses peer's built-in lists. The built-in deny list has `curl` and `wget`, so add your own lists if runs need them. |
| `policy_environment`, `policy_allow`, `policy_hard_deny` | Auto mode's prose rules for peer runs and Remote Control sessions. `$defaults` keeps Claude Code's built-ins. |
| `model` | The model for runs and sessions. Empty uses Claude's default. |
| `checkout` | A git checkout to work in, such as `/homeassistant` if HA's config is a repo. Each peer request gets its own worktree; Remote Control sessions start in the checkout. Empty uses `/data/agent`. |
| `remote_control` | Starts Remote Control. |
| `remote_control_name` | The name of Remote Control and of the sessions it starts, in claude.ai/code and the Claude app. |
| `claude_token` | A `claude setup-token` token for peer runs (see above). |
| `notify_script` | The HA script that reaches your phone: `hemma_notify_admin`. Empty: the log only. |
| `install_releases` | Installs a new Hemma Hub or Hemma Agent within 30 s of its release being published (see below). `false`: you update them in HA. |
| `log_level` | How much this add-on's log says: `info` (the default) logs requests, logins, sessions and anything wrong; `debug` adds the routine lines, such as the hub stream reconnecting and each run starting. |
| `ha_plugin` | The `homeassistant-config` plugin (ESJavadex/claude-homeassistant-plugins): skills for writing and checking HA YAML, and a hook that checks a YAML file after Claude edits it. The pinned copy ships with the add-on. |

## Installing releases

With `install_releases` on (the default), this add-on watches the add-on
repository it came from. When a release is published, it updates Hemma Hub
straight away, then itself. A running peer request finishes first (up to 30
minutes). A Remote Control session in use is cut off: start it again once the
add-on is back, which takes about a minute. A failed update is in this log
and comes to your phone through `notify_script`.

The Supervisor doesn't let an add-on update itself, so this add-on asks Home
Assistant to (`update.install` on its update entity). HA may take up to five
minutes to see the new version, so the agent's own update can lag the hub's.

## What runs can reach

`ha-ws-client` (HA's WebSocket API: states, registries, history, automation traces), `ha` (the Supervisor CLI), `addon-options` (another add-on's options, one key at a time, the rest kept), `curl`, `jq`, `yq`, `git`, `ssh`,
`mosquitto_pub`/`mosquitto_sub`, HA's config at `/homeassistant`, and
`/share`. The add-on has the Supervisor's `manager` role, so it can change
other add-ons' options (a Mosquitto login, for example).

Every session reads `/etc/claude-code/CLAUDE.md` from the image, which tells
Claude about these tools. Your own notes can go in `/data/home/.claude/CLAUDE.md`.

Remote Control runs in auto mode as root on this box. Anyone signed in to
your claude.ai account can start a session here. Turn it off with
`remote_control: false` if that's more than you want.

## Troubleshooting

- **Claude hangs or crashes on every call:** the CPU lacks AVX. On a
  Proxmox or QEMU VM, set the CPU type to `host`.
- **"gate verified" never appears:** Claude isn't logged in, or this Claude
  build fails peer's gate. Status and secrets still flow; requests wait.
- **No notification:** check `notify_script`, and that the Hemma Hub is
  0.1.32 or later. The link is in this log either way.
- **Remote Control asks "Enable Remote Control? (y/n)" or says the folder is
  untrusted:** the add-on answers both itself before Remote Control starts
  (0.3.1 and later). On an older version, update.
