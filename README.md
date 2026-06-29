# Glass

A foggy mirror that runs in your browser. Your webcam is the mirror. You breathe at your mic and the glass fogs up. Then you point your finger at the camera and write on the fog, just like writing on a real bathroom window.

It is all in one HTML file. No frameworks and no build step.

## How to run it

The browser only gives camera and mic access on a secure page, and localhost counts as secure. So you need to open it from a small local server instead of double clicking the file.

1. Open a terminal in this folder.
2. Start a simple server:

   ```
   python3 -m http.server 8000
   ```

3. Go to http://localhost:8000 in your browser.
4. Click Enter and allow the camera and the mic.

The first time you load it you need internet, because the hand tracking model is downloaded from a CDN. After that everything runs on your own computer and nothing is uploaded anywhere.

## How to use it

- Breathe at your mic and the glass fogs up.
- Point your index finger at the camera and move it to write on the fog.
- Curl your finger or drop your hand to stop writing.
- You can also drag with the mouse to wipe, which works even without a camera.
- Hold the space key to fog the glass if your mic is quiet.
- Press the c key to clear everything.

## Tech stack

Everything is plain web tech, so there is nothing to install.

- HTML, CSS and JavaScript for the whole page, all in one file.
- getUserMedia to get the webcam video and the microphone audio.
- Canvas 2D to draw the fog and erase it where you wipe. The fog is a blurred and brightened copy of your video with a frosted texture on top, and wiping reveals the sharp video underneath.
- Web Audio API with an analyser node to read the mic. It checks how loud and how noisy the sound is, so a breath counts but normal talking mostly does not.
- MediaPipe Hand Landmarker from Google for the hand tracking. It finds the points of your hand in each frame. I use the index fingertip to write, and I check the other fingers so it only writes when you are actually pointing.

## Notes

This was a fun project, so the breath detection is not perfect on every mic. If breathing does not fog it up, you can lower the sensitivity values at the top of the script in index.html, or just use the space key.
