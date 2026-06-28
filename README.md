# Glass — breath fog mirror

A foggy-mirror toy in a single `index.html`. Your webcam is the reflection;
breathe at your mic to fog the glass, then wipe and write on it with your
fingertip (hand tracking). Inspired by "Glass" by nasha.natcha.

## Run

The camera needs a secure context, so serve it over `localhost` (opening the
file directly with `file://` may block the camera in some browsers):

```sh
cd ~/Projects/glass
python3 -m http.server 8000
```

Then open **http://localhost:8000** and click **Enter**. Allow camera + mic.

## Use
- **Breathe** at your mic — the glass fogs up.
- **Move your finger** in front of the camera — wipe a clear streak / write.
- **Mouse drag** also wipes (works even without a camera).
- Hold **space** to fog if your mic isn't picking up your breath.
- Press **c** to clear all fog.

First load needs internet (MediaPipe hand model + scripts come from a CDN).
Nothing leaves your device.
