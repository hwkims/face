# Real-time Face Landmark Detection with Audio Pitch Control (face)

[![GitHub](https://img.shields.io/badge/GitHub-hwkims-blue?logo=github)](https://github.com/hwkims)

# Real-time Face & Eye Detection with Interactive Audio (face v4)

[![GitHub](https://img.shields.io/badge/GitHub-hwkims-blue?logo=github)](https://github.com/hwkims)

This project demonstrates real-time face and eye interaction using a webcam feed. It utilizes **TensorFlow.js** with the **MediaPipe Face Mesh** model to detect 468 detailed facial landmarks.

**Interactions:**

1.  **Mouth Opening -> Pitch Control:** Opening your mouth wider changes the pitch (frequency) of a continuous sine wave sound in real-time.
2.  **Eye Blinking -> Sound Trigger:** Blinking your eyes triggers a short 'blip' sound effect.
3.  **Adjustable Blink Sensitivity:** A slider allows you to adjust the threshold for detecting eye blinks, catering to different users or lighting conditions.

Detected facial landmarks are visualized as a mesh overlay on the webcam video feed. The entire application runs client-side within a single HTML file.

*(Consider adding a GIF or screenshot of the demo in action!)*
*`![Demo GIF](link_to_your_demo.gif)`*

## ✨ Features

*   **Real-time Face Detection:** Identifies faces using the MediaPipe Face Mesh model.
*   **Detailed Landmark Tracking:** Tracks 468 facial landmarks (`refineLandmarks: true`).
*   **Face Mesh Visualization:** Draws the facial structure using landmark triangulation (with fallback to drawing points if triangulation data fails to load).
*   **Mouth Opening Detection:** Calculates distance between lip landmarks.
*   **Interactive Pitch Control:** Maps mouth opening distance to the frequency of a main sine wave generated via Web Audio API.
*   **Eye Blink Detection:** Calculates distance between eyelid landmarks.
*   **Blink Sound Trigger:** Plays a distinct 'blip' sound effect (triangle wave) upon detecting an eye blink.
*   **Adjustable Blink Sensitivity:** UI slider to fine-tune the eye blink detection threshold.
*   **Dynamic Audio Handling:** Main sound plays only when a face is detected; requires initial user interaction (click) to enable audio context.
*   **Client-Side Implementation:** Runs entirely in the browser.
*   **Single File:** All code (HTML, CSS, JavaScript) in one `.html` file.

## 🛠️ Technologies Used

*   **HTML5:** Canvas API, WebRTC (`getUserMedia`)
*   **CSS3:** Basic styling and layout.
*   **JavaScript (ES6+):** Core application logic, event handling.
*   **TensorFlow.js (`@tensorflow/tfjs`):** Core machine learning library.
    *   `@tensorflow/tfjs-backend-webgl`: WebGL backend.
*   **TensorFlow.js Models (`@tensorflow-models/face-landmarks-detection`):** MediaPipe Face Mesh model.
*   **Web Audio API:** Synthesizing and controlling both the continuous sine wave and the short blink sound effect.

## 🚀 How to Run

1.  **Clone or Download:**
    *   Clone this repository:
        ```bash
        git clone https://github.com/hwkims/face.git
        cd face
        ```
    *   Or, download the main HTML file (e.g., `face_sound_v4.html` or `index.html`).

2.  **Run a Local Web Server:**
    *   **⚠️ Crucial:** You **must** run this file from a local web server due to browser security policies (webcam access, potentially model loading). Opening via `file://` **will not work**.
    *   **Python 3:** `python -m http.server` (then go to `http://localhost:8000`)
    *   **Node.js (`http-server`):** `http-server .` (then go to the address shown, e.g., `http://127.0.0.1:8080`)
    *   **VS Code:** Use the "Live Server" extension.

3.  **Open in Browser:** Navigate to the local server address.

4.  **Grant Camera Permission:** Allow browser access to your webcam.

5.  **Enable Audio (Click!):** **Click anywhere on the page once** to activate the browser's audio context (required by modern browser policies).

6.  **Interact:**
    *   Position your face in the camera view. A green mesh should appear.
    *   **Open/Close Mouth:** Hear the pitch of the main sound change.
    *   **Blink Eyes:** Hear a short 'blip' sound.
    *   **Adjust Slider:** Change the "눈 깜빡임 민감도" (Eye Blink Sensitivity) slider. A lower value makes detection more sensitive (easier to trigger), while a higher value makes it less sensitive (requires closing eyes more). Find a value that works well for you.

## 🎮 How it Works

1.  **Face Detection:** MediaPipe Face Mesh model estimates 468 landmarks on the detected face.
2.  **Mesh Drawing:** If `TRIANGULATION` data is loaded, it connects landmarks to draw a mesh; otherwise, it draws individual landmark points. This is drawn *over* the webcam feed on the canvas.
3.  **Mouth Pitch Control:** Calculates the distance between upper/lower lip landmarks. This distance is mapped to a frequency range (`minFreq` to `maxFreq`) and updates the `mouthOscillator`'s frequency.
4.  **Blink Sound Trigger:** Calculates the vertical distance between upper/lower eyelid landmarks for both eyes. If either distance drops below the `eyeBlinkThreshold` (set by the slider), the `playBlinkSound` function is called (subject to cooldown). This function quickly creates, plays, and destroys a separate oscillator/gain node for the 'blip' effect.
5.  **Audio Management:** The main `mouthOscillator` runs continuously once started, but its volume (`mouthGainNode`) is controlled. Volume ramps up to 0.5 when a face is detected and ramps down to 0 when no face is detected.

## 💡 Notes & Considerations

*   **Performance:** Can vary depending on your device.
*   **Lighting:** Good, consistent lighting is important for reliable face and landmark detection.
*   **Audio Context Click:** Remember the initial click needed to enable sound.
*   **Tuning Parameters:**
    *   **Blink Sensitivity (`eyeBlinkThreshold`):** Use the slider! The default is 6.0. Lower values are more sensitive.
    *   Mouth distance mapping (`minMouthOpenDist`, `maxMouthOpenDist`) and frequency range (`minFreq`, `maxFreq`) can be adjusted in the code for different effects.
*   **Mirroring:** Video/canvas are flipped horizontally for a mirror view.
*   **Single Face:** Currently detects and processes only one face (`maxFaces: 1`).

## 📄 License

(Optional: Add license information here, e.g., MIT License)

---

Enjoy experimenting with facial sound control!
This project demonstrates real-time face landmark detection using a webcam feed. It utilizes **TensorFlow.js** with the **MediaPipe Face Mesh** model to detect 468 detailed facial landmarks.

The primary interaction involves **controlling the pitch (frequency) of a sine wave sound based on how wide the user opens their mouth**. The detected face mesh is also visualized on the HTML Canvas overlay.

The entire application is self-contained within a single HTML file.

*(Consider adding a screenshot or GIF of the demo here!)*
*`![Demo Screenshot](link_to_your_screenshot.png)`*

## ✨ Features

*   **Real-time Face Detection:** Identifies faces in the webcam feed.
*   **Detailed Landmark Tracking:** Tracks 468 facial landmarks using TensorFlow.js and MediaPipe Face Mesh (`refineLandmarks: true`).
*   **Face Mesh Visualization:** Draws the detected facial structure using landmark triangulation.
*   **Mouth Opening Detection:** Calculates the distance between the upper and lower lip landmarks.
*   **Interactive Audio Pitch Control:** Maps the mouth opening distance to the frequency of a sine wave generated by the **Web Audio API**. Opening the mouth wider changes the sound's pitch in real-time.
*   **Dynamic Audio Handling:** Sound only plays when a face is detected and requires user interaction (a click) to start initially due to browser policies.
*   **Client-Side Implementation:** Runs entirely within the user's web browser.
*   **Single File:** All code (HTML, CSS, JavaScript) is included in one `.html` file.

## 🛠️ Technologies Used

*   **HTML5:** Canvas API, WebRTC (`getUserMedia` for webcam access)
*   **CSS3:** Basic styling and layout.
*   **JavaScript (ES6+):** Core application logic.
*   **TensorFlow.js (`@tensorflow/tfjs`):** Core machine learning library.
    *   `@tensorflow/tfjs-backend-webgl`: WebGL backend for GPU acceleration.
*   **TensorFlow.js Models (`@tensorflow-models/face-landmarks-detection`):** Pre-trained MediaPipe Face Mesh model.
*   **Web Audio API:** For generating and controlling synthesized sounds (sine waves) directly in the browser.

## 🚀 How to Run

1.  **Clone or Download:**
    *   Clone this repository:
        ```bash
        git clone https://github.com/hwkims/face.git
        cd face
        ```
    *   Or, download the main HTML file (e.g., `face_sound.html` or `index.html`) directly.

2.  **Run a Local Web Server:**
    *   **⚠️ Important:** You **must** run this HTML file from a local web server due to browser security policies regarding webcam access (`getUserMedia`) and potentially model loading. Opening the file directly via `file://` protocol will **not** work.
    *   **Option 1: Using Python 3**
        ```bash
        # Navigate to the directory containing the HTML file in your terminal
        python -m http.server
        ```
        Then open your browser to `http://localhost:8000` (or the port shown).
    *   **Option 2: Using Node.js and `http-server`**
        ```bash
        # Install http-server globally if you haven't already: npm install -g http-server
        # Navigate to the directory containing the HTML file
        http-server .
        ```
        Then open your browser to the address shown (e.g., `http://127.0.0.1:8080`).
    *   **Option 3: Using VS Code's Live Server Extension**
        *   Install the "Live Server" extension in Visual Studio Code.
        *   Open the project folder in VS Code.
        *   Right-click the HTML file and select "Open with Live Server".

3.  **Open in Browser:**
    *   Navigate to the local server address provided by your chosen method.

4.  **Grant Camera Permission:**
    *   Allow the browser to access your webcam when prompted.

5.  **Enable Audio (Click):**
    *   Click anywhere on the page once. This is often required by browsers to enable audio playback.

6.  **Interact:**
    *   Position your face in front of the webcam. You should see a green mesh overlay on your face.
    *   **Slowly open and close your mouth.** You should hear a sine wave sound whose pitch changes according to how wide your mouth is open. The sound will stop if your face is no longer detected.

## 📝 How Pitch Control Works

1.  The application continuously detects 468 face landmarks.
2.  It specifically tracks the landmarks for the center of the upper lip (`index 13`) and the center of the lower lip (`index 14`).
3.  The Euclidean distance between these two landmarks is calculated in each frame.
4.  This distance is normalized to a range of [0, 1] based on predefined minimum (`minMouthOpenDist`) and maximum (`maxMouthOpenDist`) expected distances (these might need tuning).
5.  The normalized distance is used to linearly interpolate a frequency value between a minimum (`minFreq`) and maximum (`maxFreq`) frequency.
6.  The frequency of the Web Audio API's `OscillatorNode` is smoothly updated to this calculated frequency using `setTargetAtTime`.

## 💡 Notes & Considerations

*   **Performance:** Detection speed depends on your device's hardware.
*   **Lighting:** Good lighting improves detection accuracy.
*   **Audio Context:** Requires user interaction (click) to start due to browser autoplay policies.
*   **Tuning:** The values for `minMouthOpenDist`, `maxMouthOpenDist`, `minFreq`, `maxFreq`, and `minConfidence` in the JavaScript code might need adjustment based on your camera, distance, and desired sensitivity/range. You can check the console for the raw `mouthDistance` values to help with tuning.
*   **Mirroring:** The video and canvas are flipped horizontally for a natural mirror effect.
*   **Single Face:** The current configuration (`maxFaces: 1`) only processes the first detected face.

## 📄 License

(Optional: Add license information here. MIT License is common for such projects.)
Example: This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details (if you add one).

---

Feel free to experiment and modify the code!
