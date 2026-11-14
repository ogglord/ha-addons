# Home Assistant add-ons

Add-ons for my Home Assistant. In HA, go to Settings › Add-ons › Add-on store ›
⋮ › Repositories and add `https://github.com/ogglord/ha-addons`.

| Add-on | What |
|---|---|
| `hemma_hub` | The hub behind the Hemma iOS app: push notifications, the nursery, live video |
| `ha_agent` | Hemma Agent: headless Claude Code and the homelab `peer` listener on the HA box |

Nothing here is edited by hand. Each add-on's manifest and changelog are
published from the (private) source repository by its CI, after its image
(`ghcr.io/ogglord/hemma-hub-{arch}`, `ghcr.io/ogglord/hemma-agent-{arch}`)
has been built for that version. Settings and
secrets (rooms, APNs key, Cloudflare Access token) are the add-on's options in
HA, not files here.
