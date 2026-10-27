# Interactive Engine Audio Sandbox (Unity + FMOD)

A playable spaceship prototype where player input drives engine sound in real time. A C# controller turns throttle into **RPM** and **Load** values and streams them to FMOD, which blends engine layers, bends pitch, and fades in mechanical strain as the ship accelerates.

**▶ Play it in the browser:** [deniztugluaudio.com/demo](https://deniztugluaudio.com/demo/) (hold **W** to accelerate, **← / →** to switch ships, headphones recommended)

![Spaceship audio sandbox in action](https://deniztugluaudio.com/wp-content/uploads/2026/01/trimmedGifForWeb-ezgif.com-video-to-gif-converter.gif)

## Signal flow

```
Player input (throttle) → SpaceshipController.cs → Engine_RPM + Engine_Load → FMOD event → layered engine sound
```

## How it works

**C# controller** — [`Assets/Scripts/SpaceshipController.cs`](Assets/Scripts/SpaceshipController.cs)

- Maps throttle input to a target RPM between `idleRPM` and `maxRPM`.
- Smooths RPM with `Mathf.SmoothDamp`, using separate spool-up (`timeToReachMax`) and spool-down (`timeToDropToIdle`) times, so engines rev and settle believably.
- Derives **Load** from how fast RPM is rising compared with the ship's maximum expected acceleration (0–1), so hard acceleration reads as strain.
- Sends both values to FMOD every frame with `setParameterByName`, and keeps the event's 3D position in sync with the ship.
- All tuning values are exposed in the Inspector, so one script can power different ships: a heavy transport that revs slowly, or a fighter that responds instantly.

**FMOD event** — [`TechDemoFMOD/`](TechDemoFMOD/)

- **RPM** drives pitch (AutoPitch) and crossfades between idle, mid, and high recordings without audible seams.
- **Load** fades in layers of mechanical strain and exhaust rumble when the ship accelerates hard.
- Swapping the source recordings creates a new ship profile while the mixing logic stays the same.

## Opening the project

- Unity **6000.3.0f1** (Unity 6) with the FMOD for Unity integration (included under `Assets/Plugins/FMOD`).
- The FMOD Studio project lives in `TechDemoFMOD/`; built banks are in `Assets/StreamingAssets/`.
- Open `Assets/Scenes/Main.unity` and press Play.

## Credits

- Design, programming, implementation and sound design: **Deniz Tuglu**
- Shaders and UI: Onur Şahin Şentürk
- 360° skybox image: José Francisco
- Spaceship models: [Niko-3D-Models](https://niko-3d-models.itch.io/free-sc-fi-spaceships-pack)

## License

My code in this repository is MIT licensed (see [LICENSE](LICENSE)). Third-party assets keep their own licenses: the FMOD for Unity integration, TextMesh Pro, the spaceship models, the skybox image, and the shaders/UI credited above.

---

More of my work: [deniztugluaudio.com](https://deniztugluaudio.com) · [LinkedIn](https://www.linkedin.com/in/deniz-tuglu/)
