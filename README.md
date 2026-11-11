# Home Assistant add-ons

Add-ons for my Home Assistant. In HA, go to Settings › Add-ons › Add-on store ›
⋮ › Repositories and add `https://github.com/ogglord/ha-addons`.

| Add-on | What |
|---|---|
| `hemma_hub` | The hub behind the Hemma iOS app: push notifications, the nursery, live video |

Nothing here is edited by hand. The add-on's manifest and changelog are
published from the (private) source repository by its CI, after the image
`ghcr.io/ogglord/hemma-hub-{arch}:<version>` has been built. Settings and
secrets (rooms, APNs key, Cloudflare Access token) are the add-on's options in
HA, not files here.
