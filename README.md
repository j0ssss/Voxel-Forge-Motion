# ⬡ VoxelForge Motion
### Convert any video into an animated Roblox voxel script

VoxelForge Motion takes an MP4 (or AVI, MOV, MKV, WebM) video and converts it into a ready-to-run Roblox Lua script that plays back as animated colored blocks — entirely inside Roblox Studio, no plugins required.

---

##  Features

- **Video → Lua** — converts real video frames into per-frame voxel color data
- **Simultaneous block loading** — all parts appear at the exact same time, no pop-in
- **Color-only updates** — parts are never destroyed or recreated between frames, only colors change, keeping Roblox's engine happy
- **Auto-loops forever** — `loopVideo = true` by default, toggle at the top of the script
- **Auto-downsize** — automatically steps down resolution if your video would exceed Roblox's safe block limit (200,000 total blocks)
- **5 resolution presets** from ultra-sharp to ultra-light
- **3D orientation gizmo** — drag rings to rotate the screen in 3D space before export
- **Placement coordinates** — set exact Upper-Left / Lower-Right world coordinates
- **Transparency support** — skips fully transparent pixels for WebM/alpha videos



https://github.com/user-attachments/assets/142923c1-f8c4-42a2-a224-d8cf60adb55e


<img width="789" height="903" alt="image" src="https://github.com/user-attachments/assets/3d3192fb-b4e0-493a-8c7f-a169f25936f3" />


---

##  Resolution Presets

| Preset | px/stud | Best for |
|--------|---------|----------|
| QUALITY | 2 px | Tiny clips only (< 5s, small resolution) |
| DEFAULT  | 3 px | Recommended balance |
| IDEAL | 5 px | Most 720p clips |
| LIGHT | 8 px | 1080p footage |
| ULTRA | 12 px | Maximum compatibility, minimum detail |

**Recommended settings:** 10 fps, DEFAULT preset, clips under 10 seconds.

---

##  Quick Start (Python)

**Requirements:** Python 3.9+

```bash
# Dependencies are auto-installed on first run, but you can pre-install:
pip install pillow opencv-python numpy
```

```bash
python main.py
```

Then:
1. Click **SELECT VIDEO** and choose your file
2. Set FPS (default 10, max recommended 20)
3. Pick a resolution preset (DEFAULT is fine for most videos)
4. Optionally set placement coordinates or rotate with the gizmo
5. Click **GENERATE LUA SCRIPT**
6. Find your `.lua` file in `~/Downloads`

---

##  Using in Roblox Studio

1. Open Roblox Studio → **Baseplate** template
2. In the **Explorer**, go to `ServerScriptService` → right-click → **Insert Script**
3. Delete the default `print("Hello world!")` line
4. Open your generated `.lua` file in Notepad → **Select All** → **Copy**
5. Paste into the Script editor
6. Press **Play** — all blocks appear simultaneously, then playback begins
7. Loops automatically (set `loopVideo = false` at the top to play once)

---

##  Recommended Video Lengths

| FPS | Max Length | Frames | Notes |
|-----|-----------|--------|-------|
| 10 fps | 5s | 50 | Very safe |
| 10 fps | 10s | 100 | Recommended ceiling |
| 20 fps | 5s | 100 | Max safe at 20 fps |

Keep **total blocks under 200,000** for smooth playback. The estimator in the UI shows you live before you generate.

---

##  Running as an EXE (Windows)

If you just want the `.exe` without building it yourself, check the [Releases](../../releases) page.

To build it yourself:

```bash
pip install pyinstaller
python -m PyInstaller --onefile --windowed --name "VoxelForge" main.py
```

The exe will be in the `dist/` folder. Note: antivirus may flag PyInstaller exes as a false positive — this is a known issue with PyInstaller on Windows, not malware.

---

##  How It Works

1. **Frame extraction** — OpenCV reads the video and samples frames at your target FPS
2. **Color quantization** — each frame's colors are reduced to a 255-color palette using median-cut
3. **Hex encoding** — pixel palette indices are packed into a hex string per frame
4. **Lua generation** — a single `.lua` script is written containing all frame data and a playback engine that:
   - Creates all `Part` objects once, un-parented
   - Parents them all to a `Folder` simultaneously (entire screen appears at once)
   - Waits for `GetDescendants()` count to confirm full load
   - Updates only `Part.Color` and `Part.Transparency` per frame (never destroy/recreate)
   - Yields every N rows during frame updates so Roblox's engine stays responsive

---

##  Troubleshooting

**"list index out of range"** — make sure you're on the latest version; this was a Pillow palette bug that has been fixed.

**Script is too laggy in Studio** — reduce FPS, switch to a lighter preset (IDEAL, LIGHT, or ULTRA), or shorten the clip. Keep total blocks under 200,000.

**Antivirus blocks the .exe** — expected with PyInstaller builds. Run from source with `python main.py` if you prefer.

**Video won't open** — make sure `opencv-python` installed correctly. Try running `python main.py` once from the terminal to see install output.

---

