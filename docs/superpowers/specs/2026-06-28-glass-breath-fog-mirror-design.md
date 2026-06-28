# Glass — breath-fog mirror (design)

Date: 2026-06-28

## What it is
A single-page web toy that recreates a foggy bathroom mirror in the browser.
The webcam is your reflection; a fog layer sits on top. You **breathe** at your
mic to fog the glass, and you **wipe / write** on it with your fingertip via hand
tracking. Inspired by "Glass" by nasha.natcha (Instagram).

## Stack
- Single `index.html`, no build step.
- `getUserMedia({video, audio})` — one stream, one permission prompt.
- Webcam shown in a mirrored `<video>` (`object-fit: cover`, `scaleX(-1)`).
- Fog drawn on a full-screen `<canvas>` above the video.
- Breath detection: Web Audio `AnalyserNode` (RMS volume + spectral flatness).
- Hand tracking: MediaPipe `@mediapipe/tasks-vision` `HandLandmarker` (VIDEO mode,
  1 hand), loaded from jsDelivr CDN; model from Google's model store.

## Interactions
1. **Breath → fog.** Each frame compute RMS volume and spectral flatness of the
   mic. A breath is broadband + non-tonal (high flatness) + loud enough — this
   separates a "whoosh" from speech. Breath intensity adds fog opacity across the
   glass. Fallback: hold **Space** to fog.
2. **Fingertip → wipe.** `HandLandmarker` gives index-fingertip (landmark 8) each
   frame; mirror its x to match the displayed video and erase fog along its path
   (`globalCompositeOperation = 'destination-out'`, soft radial brush, segments
   interpolated for smooth strokes). Fallback: mouse/touch drag also wipes.
3. **Slow re-condense.** A tiny per-frame ambient refog makes wiped writing fade
   over ~12s, like real glass. Press **c** to clear all fog instantly.

## Fog rendering (one canvas)
Per frame, in order:
1. Refog: `source-over` fill of cool-white at small alpha (ambient) + extra alpha
   scaled by breath / Space. Faint grain stamped for a frosted look.
2. Wipe: `destination-out` radial brush along hand + pointer stroke segments.
Starts fully fogged so you must wipe to see yourself. DPR-scaled backing store;
re-fill on resize.

## Flow & constraints
- A **Enter** button gates startup (camera/mic/AudioContext need a user gesture).
- Must be served over a secure context — `localhost` counts. Run with
  `python3 -m http.server` and open `http://localhost:8000`. `file://` may block
  the camera in some browsers.
- Everything runs client-side; no data leaves the device. Needs internet for the
  CDN scripts + hand model on first load.

## Out of scope (v1, YAGNI)
Color picker, saving snapshots, multi-hand, gesture menus, mobile-specific tuning.
