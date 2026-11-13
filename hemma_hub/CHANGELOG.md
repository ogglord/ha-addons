# Changelog

HA shows this in the add-on's update dialog. `scripts/release-hub.sh` adds a
section for each release, drafted from the commit titles; rewrite it for the
family.

## 0.1.23

- A security update. **Nothing needs re-pairing:** every phone keeps
  working as it is, and the app needs no update.
- A paired phone could reach more of Home Assistant through the hub than the
  notification pictures it is meant to fetch. That is closed.
- The pairing code the hub prints in this log for the very first admin phone
  no longer contains the Cloudflare key, and lasts an hour instead of a day.
  This only matters on a fresh install: that first phone then works on the
  home Wi-Fi only, until it is paired again from Admin › Lägg till telefon.
- While a nursery camera's privacy switch is unavailable (the camera is
  offline or restarting), the app shows that room as in Integritetsläge and
  shows no picture or sound from it, until the switch is back.
- Notifications no longer carry Home Assistant's camera names, and a button
  on a notification can only do what that notification offered.
- The add-on no longer runs as root. If it does not start after this update,
  go back to 0.1.22 from a backup and tell Oscar.

## 0.1.22

- Ready for the app's camera controls: move a nursery camera up, down, left
  or right, and change a few of its settings (step size, night vision, status
  light, flip, microphone). To turn it on, add `tapo:` to each room in
  Configuration: the camera's prefix in HA, e.g. `bedroom_ludvig` for
  `button.bedroom_ludvig_move_up`.

## 0.1.21

- Ready for the app's sharper Utomhus: at home, the outdoor cameras can
  show their full-resolution picture live instead of the small one. The
  driveway's camera sends a format the live path can't carry, so its sharp
  picture comes a few seconds behind instead.

## 0.1.20

- Ready for the app's next Utomhus: each outdoor camera's events from the
  last ten days, an event's picture in full size, and its clip from Frigate's
  recording, also away from home. Only the outdoor cameras' events are shown.

## 0.1.19

- The add-on log starts each run with a large HEMMA banner, the hub's version
  and the time it started, so it's easy to see where the latest start begins.

## 0.1.18

- The app's new El tab: whether the house uses more electricity than usual
  this week, the price per kWh now against the last 30 days with today's
  hours, and the metered plugs. Set it up under `electricity` in the
  add-on's Configuration (the same keys as `hub/hemma.yaml`). Without it the
  tab says there is no electricity data.

## 0.1.17

- The add-on log no longer fills with ffmpeg "No such file or directory"
  errors when a live stream stops (no viewers left, or Integritetsläge).

## 0.1.16

- Privatläge is now called Integritetsläge, in HA too: the helpers
  "Hemma: <room> integritetsläge till" and the automation
  "Hemma: integritetsläge av".

## 0.1.15

- Privatläge's timer in HA starts cleared when the hub creates it. Before,
  privacy mode turned on in the Tapo app could be turned off by HA within
  5 minutes.

## 0.1.14

- The snapshot says which nurseries have Privatläge and how long it lasts
  there (`privacy_hours`), so the app shows the button only where it works.

## 0.1.13

- Privatläge: a phone can put a nursery camera in its own privacy mode, with
  no picture, sound or alerts from that room. Only the phone that started it
  can end it; otherwise it ends by itself after 8 hours (`privacy_hours`).
  A room opts in with `privacy_switch`: the HA switch for the camera's
  privacy mode (from the TP-Link integration). Without it, nothing changes.

## 0.1.12

- The nursery switch in HA is now called "Gråtlarm" (was "Barnen sover"), and
  the hub renames an existing helper when its name in the settings changes.

## 0.1.11

- The hub remembers which app build each phone runs, and `/v1/me` tells a
  phone when another one runs a newer build (`newer_build`), so the app can
  point to TestFlight.

## 0.1.10

- The snapshot carries the sun's elevation (`sun_elevation`, from HA's
  `sun.sun`), for the sky in the app's house drawing.

## 0.1.9

- Lyssna has noise reduction: about 12 dB less hiss. Very faint sounds get a little fainter too (quiet speech about 4 dB), so there is a new option `listen_denoise` (default on) to turn it off.

## 0.1.8

- Lyssna filters out rumble below 100 Hz. Nobody hears it, but it was most of the level in a silent room, so the level meter moved with nothing to hear. A silent room now reads about -66 dB at the default gain (was -52).

## 0.1.7

- No change to the hub. The first release built by a faster release pipeline.

## 0.1.6

- Lyssna plays again. In 0.1.5 the audio stream never started in the add-on: ffmpeg could not open the output for the level meter on Linux and exited at once, so the app got no sound and no level.

## 0.1.5

- Lyssna has gain: new option `listen_gain_db` (0-40, default 20), with a limiter so a cry can't clip. A sleeping child's room sits near the camera's noise floor (about -70 dBFS), which the Tapo app plays back louder than a raw RTSP stream; at +20 dB breathing and household noise are audible.

## 0.1.4

- Lyssna starts faster. ffmpeg's audio path probes the stream briefly (3-4 s to the first playlist became about 1.3 s), and each nursery's audio stream is kept running (restarted if it exits or stalls), so the first request finds a playlist already there.

## 0.1.3

- Lyssna (listening) reads a nursery's sub stream instead of its main one. Frigate keeps the sub open, so it starts at once; a main stream opened on demand took up to a minute over a weak camera link.

## 0.1.2

- `GET /v1/me` also returns `lan_url` and `tunnel_url`, so phones pick up a changed address without pairing again (app 0.1.3 and later).

## 0.1.1

- Options `frigate_url` and `outdoor`, which the hub already read: set them to get the Utomhus tab.

## 0.1.0

- First add-on build: notifications, the nursery, pairing, live video.
