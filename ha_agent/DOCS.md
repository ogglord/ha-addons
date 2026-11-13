# HA Agent

Runs the homelab `peer` listener and Claude Code on this box, with no
terminal and no web UI. Peer requests sent to host `homeassistant` run as
headless `claude -p` in auto mode. Claude Code Remote Control (on by default)
lets you start a session here from claude.ai/code or the Claude app.

## Before you start

- The Tailscale add-on must run in TUN mode (`userspace_networking: false`)
  and advertise `tag:ha-agent`. This add-on reaches the hub through the host's
  tailnet route; the hub knows the box by Tailscale WhoIs.
- Stop the old Claude Terminal add-on (claude-code-ha) first. Two listeners
  would both claim requests for `homeassistant`.
- Set `hub_url` in Configuration (the hub's tailnet address, port 7790).

## The one-off login

Claude needs the claude.ai login (your Max subscription). A `setup-token`
is not enough: Remote Control refuses it.

1. Start this add-on. The log says "Claude is not logged in".
2. Open a shell on the host that can run `docker`: the Advanced SSH & Web
   Terminal add-on with Protection mode off.
3. Run:

   ```sh
   docker exec -it "$(docker ps -qf name=ha_agent)" claude auth login
   ```

4. Open the link it prints, sign in, and paste the code back.
5. Restart this add-on. The log shows "Claude is logged in", the peer
   listener's "gate verified ..." line, and Remote Control starting.

The login lives in `/data/home` (`.claude/.credentials.json` and
`.claude.json`). Claude refreshes it there, so it survives restarts, add-on
updates and backups. You log in again only if you sign out or revoke it on
claude.ai, or if the refresh token expires unused.

## Options

| Option | What it does |
|---|---|
| `hub_url` | The peer hub, `http://<tailnet IP>:7790`. |
| `allow`, `deny` | Tool rules in Claude Code's permission syntax. Empty uses peer's built-in lists. The built-in deny list has `curl` and `wget`, so add your own lists if runs need them. |
| `policy_environment`, `policy_allow`, `policy_hard_deny` | Auto mode's prose rules for peer runs and Remote Control sessions. `$defaults` keeps Claude Code's built-ins. |
| `model` | The model for runs and sessions. Empty uses Claude's default. |
| `checkout` | A git checkout to work in, such as `/homeassistant` if HA's config is a repo. Each peer request gets its own worktree; Remote Control sessions start in the checkout. Empty uses `/data/work`. |
| `remote_control` | Starts Remote Control. |
| `remote_control_name` | The session's name in claude.ai/code. |
| `ha_plugin` | The `homeassistant-config` plugin (ESJavadex/claude-homeassistant-plugins): skills for writing and checking HA YAML, and a hook that checks a YAML file after Claude edits it. The pinned copy ships with the add-on. |

## What runs can reach

`ha-ws-client` (HA's WebSocket API: states, registries, history, automation traces), `ha` (the Supervisor CLI), `curl`, `jq`, `yq`, `git`, `ssh`,
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
- **Remote Control says the folder is untrusted:** restart the add-on once
  after the first login. It marks the work folder trusted at start.
