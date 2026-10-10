# Qolsys Panel Camera for Scrypted

This Scrypted plugin shows the built-in camera of a Qolsys IQ Panel. You get
fresh snapshots and a basic live view. Everything stays on your local network.
It does not use Alarm.com.

I built it for my own panel. It works, but it has real limits. Please read them
before you install it.

## Limits

- I have only tested it on an IQ Panel 2+ running firmware 2.8.1. Other panels
  and firmware may not work.
- Live view is choppy. Expect about 3 to 5 frames a second, about 5 seconds
  behind. The picture freezes briefly every 6 seconds between clips.
- The panel pauses its camera motion detection while you watch. The plugin
  turns it back on when you stop, if it was on before.
- Live view only runs while the panel is disarmed, has no alarm, and is on AC
  power. One view lasts up to 5 minutes.
- Each snapshot takes about 2 to 3 seconds.
- Each snapshot writes a small file to the panel's flash storage. The plugin
  deletes it right after. To limit wear, it takes at most one snapshot every
  30 seconds by default. You can change that in the settings.
- There is no continuous recording. Scrypted NVR and HomeKit Secure Video will
  not work with this camera.
- The panel must be paired as an IQ Remote. You confirm the pairing on the
  panel itself.

## How it works

The panel has a local connection for IQ Remote touchscreens. This plugin pairs
with the panel as one of those remotes and uses the same connection.

For a snapshot, it asks the panel to take one picture. It downloads that
picture, then deletes the file and the request from the panel. The picture is
never sent to Alarm.com.

For live view, it asks the panel to record a short 6-second clip. It downloads
the clip while the panel is still recording it and sends the new frames to
Scrypted. When the clip ends, it starts the next one, as long as someone is
still watching. Each clip is deleted when it is done. If an alarm needs the
camera, the panel's own recording always wins.

The plugin never arms or disarms the panel. It only uses the camera.

The panel protocol comes from the
[qolsys-controller](https://github.com/EHylands/QolsysController) Python
library by EHylands. This plugin needs version 1.11.0 or newer, which Scrypted
installs for you.

## Setup

1. Install the plugin from the Scrypted plugin list. Search for
   `@cbusillo/scrypted-qolsys`.
2. Open the Qolsys Panel Camera device. Enter the panel's IP address.
3. Select **Pair IQ Remote**. Then, on the panel, go to IQ Remote Devices and
   press **Pair**. When pairing is done, the panel lists the plugin as one of
   its IQ Remote devices.
4. Snapshots work now.
5. For live view, first open **Extensions** on the device and turn off
   **Rebroadcast Plugin**. Scrypted adds it on its own, and it would keep
   asking the panel to record.
6. Turn on **Experimental live preview**. While the panel is disarmed, select
   **Calibrate video**. This records one 8-second clip to learn the video
   format, then deletes it.

The pairing keys are stored in the plugin's data folder under `qolsys/pki`.
Keep them if you move or reinstall Scrypted, or you will need to pair again.

## Development

You need Node.js and [uv](https://docs.astral.sh/uv/).

```sh
npm install
npm test
npm run build
```

`npm test` runs the Python tests on Python 3.12. They use made-up media and
never talk to a real panel. `npm run build` writes `out/plugin.zip`.

## License

MIT. See [LICENSE](LICENSE).

Qolsys and IQ Panel are trademarks of their owners. This project is not
affiliated with Qolsys, Johnson Controls, or Alarm.com.
