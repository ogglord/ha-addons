# Changelog

HA shows this in the add-on's update dialog. `scripts/release-hub.sh` adds a
section for each release, drafted from the commit titles; rewrite it for the
family.

## 0.1.34

- Hubben kan nu visa homelabben i appen, på en egen flik för administratörer:
  översikt, tjänster, mätvärden, backuper och lagring, var man än är. Den
  läser bara från homelabben. Att starta om en tjänst eller köra en backup
  går via Home Assistant, med de knappar homelabben redan har där.
- Fyll i `homelab_url` (homelabbens adress) i tilläggets inställningar för
  att slå på det. Fliken kräver nästa version av appen.

## 0.1.33

- Telefonerna pratar nu krypterat med hubben hemma på wifi:t. Hubben gör ett
  eget certifikat första gången den startar och lyssnar på en ny port, 8788.
  Telefonerna känner igen certifikatet, så en annan server på nätverket kan
  varken läsa med eller logga ut dem. Parade telefoner hämtar certifikatet
  via tunneln, utan att paras om. Det kräver nästa version av appen.
- Den gamla okrypterade porten 8787 finns kvar för tunneln och homelabben.

## 0.1.32

- Knappar i notiser kan nu öppna en länk i Safari, eller be om ett kort svar
  som man skriver direkt i notisen (som i Home Assistants app). Svaret går
  bara till Home Assistant: hubben varken loggar eller sparar det. Att skriva
  i notisen kräver nästa version av appen.

## 0.1.31

- The homelab can notify the admins through Hemma, and send them steps to
  approve. A new option, `homelab_token`, is the homelab's key to the hub:
  set it to the same value the homelab has (32 characters or more). Left
  empty, nothing changes. Approving needs the next app.

## 0.1.30

- The speaker now speaks Swedish, without the internet: it uses Home
  Assistant's Piper instead of Google's English voice, and names the room
  ("Ludde gråter i sitt rum."). **After updating, change `tts` in the nursery
  options to `tts.piper`.** The new `tts_voice` option is the Piper voice
  (default `sv_SE-lisa-medium`).
- When a crying alert can't reach a phone (the internet is down, or the hub
  is), the speaker says it at once instead of after a minute.
- At start, the hub checks that the speaker's voice exists in Home Assistant
  and says so in its log and on the health page ("Högtalaren (tal)").

## 0.1.29

- For the next app: Huset shows the temperature and humidity on each floor,
  and the temperature outside. Two new options say where the sensors are:
  `climate` (a list of `place`, `temperature` and `humidity`, where place is
  a room such as `bedroom` or `bath_upstairs`, or a floor: `upper`, `main`,
  `basement`) and `weather` (a weather entity, e.g. `weather.forecast_home`).
  Left empty, Huset shows no temperatures. The current app works as before.

## 0.1.28

- Opening the add-on's Web UI in Home Assistant now shows the hub's health
  (Home Assistant, push, video, Frigate, ffmpeg, the heartbeat and any alert
  that didn't get through) instead of a "not paired" error. It reloads
  every 30 seconds.

## 0.1.27

- Home Assistant now notices when the hub is down. The hub checks in every
  minute; after 5 minutes without it (the new `hub_down_minutes` option),
  HA shows a notification saying alerts are not getting through, and says
  it on the speaker while Gråtlarm is on. The notification goes away when
  the hub is back.
- An alert that didn't reach a phone is no longer silent. The admins get a
  push about it (at most once an hour) and HA keeps a note. An alert sent
  without an APNs key now shows in Larmlogg with that reason.
- If the hub hangs, Home Assistant restarts it (the add-on's watchdog).
- For the next app: a health page for the admins (Home Assistant, push,
  video, Frigate, ffmpeg), which phones get no alerts, and the hub's
  version. The current app works as before.

## 0.1.26

- A small fix to 0.1.25: when a live video request failed, the add-on's log
  showed the short-lived link the phone used. It no longer does. Nothing
  needs re-pairing, and the app needs no update.

## 0.1.25

- Alerts are harder to lose. If the connection to Apple's push service has
  died quietly, the hub notices, opens a new one and sends the alert again
  (once), instead of failing every alert for a while. It also tries once
  more when Apple is busy.
- When Home Assistant restarts, the hub is back within about 5 seconds and
  its log says how long the gap was. An alert sent while the connection was
  down still can't arrive, so that gap is visible in the log.
- A button in the app that Home Assistant does not answer (moving a camera,
  "Barnen sover") now gives an error after 10 seconds instead of hanging.
  A camera no longer stays "still moving" afterwards.
- An error in one alert no longer stops the whole hub.
- A notification whose `tag` is a number (HA templates turn "123" into one)
  now arrives instead of failing.
- Nothing needs re-pairing, and the app needs no update.

## 0.1.24

- A nursery camera that is really offline (unplugged, or restarting) now
  shows as "Kameran är offline" in the app. 0.1.23 showed it as in
  Integritetsläge, which it was not. Still no picture or sound from the room
  until the camera is back. Nothing needs re-pairing, and the app needs no
  update.

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
