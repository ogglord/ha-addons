# Changelog

HA shows this in the add-on's update dialog. `scripts/release-hub.sh` adds a
section for each release, drafted from the commit titles; rewrite it for the
family.

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
