# Changelog

HA shows this in the add-on's update dialog. `scripts/release-hub.sh` adds a
heading for each release; fill in what changed.

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
