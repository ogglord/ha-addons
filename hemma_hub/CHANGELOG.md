# Changelog

HA shows this in the add-on's update dialog. `scripts/release-hub.sh` adds a
heading for each release; fill in what changed.

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
