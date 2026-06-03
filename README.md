# VolumeKnuckle ✊

VolumeKnuckle is a gesture-controlled volume adjuster: it uses your webcam and
**MediaPipe Hands** to detect when you make a fist, then raises or lowers your
system volume based on how high or low you hold it. Built for the
**BUILDCORED ORCAS — Day 03** challenge.

## How it works

- Uses **OpenCV** to read frames from your webcam in real time.
- Runs **MediaPipe Hands** to track 21 hand landmarks on each frame.
- Detects a **fist** by checking whether the fingertips are curled closer to the
  wrist than their knuckles, with a curl margin + a "one lazy finger" allowance
  so detection doesn't flicker.
- Tracks the **vertical position** of the fist center frame-to-frame (EMA
  smoothed, with a movement deadzone) — raise your fist to increase volume,
  lower it to decrease.
- Uses **pycaw** to directly control the Windows system audio endpoint.

## What's tuned

A few quality-of-life improvements over a naïve implementation:

| Tweak | Why |
|---|---|
| EMA smoothing on fist position | Removes hand jitter so volume doesn't twitch |
| Movement deadzone | Sub-pixel noise won't nudge the volume |
| Margin-based fist detection | Hysteresis stops flicker near the open/closed threshold |
| Smoothed on-screen volume bar | The bar glides instead of jumping |

Tuning constants live near the top of `main()` (`VOL_SENSITIVITY`,
`UPDATE_INTERVAL`, `DEADZONE_PX`, `Y_SMOOTH`, `VOL_SMOOTH`) — adjust to taste.

## Requirements

- Python 3.10.x
- Windows OS (pycaw controls the Windows audio endpoint)
- A working webcam

Python packages:

```
pip install -r requirements.txt
```

## Usage

```
python volumeknuckle.py
```

- A webcam window opens, mirrored like a selfie.
- On-screen overlay shows a volume bar (right), FPS counter (top-left), and a
  `FIST DETECTED` / `NO FIST` status badge (bottom-left).

| Gesture | Action |
|---|---|
| ✊ Make a fist + raise it | Volume **UP** |
| ✊ Make a fist + lower it | Volume **DOWN** |
| 🖐 Open hand / no hand | Volume unchanged |
| `Q` key | Quit |

## Credits

- Hand tracking: [MediaPipe Hands](https://developers.google.com/mediapipe/solutions/vision/hand_landmarker)
- Video capture: [OpenCV](https://opencv.org/)
- System volume control: [pycaw](https://github.com/AndreMiras/pycaw)

---

Built as part of the **BUILDCORED ORCAS — Day 03: VolumeKnuckle** challenge.
