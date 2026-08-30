# Customizable Institutions Globe

An interactive, rotating 3D globe that highlights a network of institutions (universities, research centers, offices — anything with a location) with animated labels, country highlighting, and a built-in **Control Panel** for customizing everything without touching code. Includes one-click **screenshot** and **video export** of the rotation.

Built on [globe.gl](https://globe.gl) / [three.js](https://threejs.org).

> **Attribution required.** This project is free to use for **non-commercial purposes only**, provided you credit the original author. See [LICENSE.md](LICENSE.md) for the full terms.
>
> Original design & build: **[github.com/dramanuj](https://github.com/dramanuj)**

---

## 1. Contents

- `index.html` — the whole app (globe + control panel). No build step, no dependencies to install.
- `config.example.json` — an example configuration you can copy to `config.json` to customize the globe **without editing any code**.
- A live **Control Panel** (gear icon, top-right) with tabs for:
  - **Locations** — add/remove/edit universities or any custom locations (name, lat/lng, country, priority, colors).
  - **Countries** — which countries get highlighted on the map, and in what color.
  - **Style** — globe colors, atmosphere, marker colors, marker icon.
  - **Rotation** — rotation speed, the "slow zone" longitude band, camera angle, label timing.
  - **Text** — page title and an optional on-screen caption/watermark (position, color, size).
  - **Export** — pause/orbit the camera manually, capture a PNG screenshot at any angle, or record an MP4/WebM-style video of the rotation.
  - **Config** — download/upload your whole setup as a `config.json` file, save/load it in the browser, or paste JSON directly.

---

## 2. Quick start 

1. Download this folder (or clone the repo — see hosting instructions below).
2. Double-click `index.html` to open it in your browser (Chrome, Edge, or Firefox recommended).
3. Click the **⚙ gear icon** top-right to open the Control Panel.
4. Go to **Locations** and edit the sample universities, or add your own with **+ Add Location**. (The included demo data is DTU's real alliance and strategic-partner universities, as a working example — replace it with your own network.)
   - `Priority`:
     - **Always** — the label is shown whenever that point is in view (use for your main/highlighted location).
     - **Sequential** — locations in the same "sequential" group take turns being shown one at a time as the globe passes over them (handy for a cluster of nearby institutions, e.g. several in the same country).
     - **Normal** — shown whenever in view, like any other label.
5. Go to **Countries** to control which countries get tinted on the map.
6. Click **✔ Apply Changes to Globe** (bottom of the panel) to see your edits.
7. When you're happy, go to **Config → ⬇ Download config.json** and save that file **in the same folder as `index.html`**. From now on, the page will load your saved setup automatically for anyone who opens it — no panel editing needed each time.

That's it — you now have a fully customized globe.

---

## 3. Customizing in detail

### Adding a location
Control Panel → **Locations** → **+ Add Location**, then fill in:
- **Name** — label text shown on the globe.
- **Latitude / Longitude** — decimal degrees (e.g. Copenhagen ≈ `55.68, 12.57`). Look coordinates up on Google Maps: right-click a spot → the numbers shown are lat, lng.
- **Country** — must match the country's official English name as it appears on the world map (e.g. `"United States of America"`, not `"USA"`) if you also want that country highlighted.
- **Priority** — `normal`, `always`, or `sequential` (see above).
- **Highlight** — check this to make the location stand out using the "Highlighted Marker Style" colors (Style tab) — e.g. for your own institution.
- Optional per-location color overrides (point/label/border) if you don't want it to use the theme defaults.

### Highlighting countries
Control Panel → **Countries** → **+ Add Country**, type the country's name and pick a color.

### Changing the look
Control Panel → **Style** — background, atmosphere glow, globe surface color, default vs. highlighted marker colors, and the marker icon (defaults to 📍, but can be any emoji or short text).

### Rotation speed & camera
Control Panel → **Rotation**:
- **Base Speed** — normal rotation speed (degrees/second).
- **Slow-Zone Speed** + **Slow Zone Start/End** — the globe slows down while a chosen longitude band is centered in view, so viewers get more time to read labels in a dense region (defaults to Europe).
- **Camera Latitude / Altitude** — how "zoomed in" and tilted the view is.
- **Label Visibility Window** — how many degrees of longitude a label stays visible for as it enters/leaves view.
- **Sequential Label Duration** — how long each "sequential" location is shown before cycling to the next.

### On-screen text
Control Panel → **Text** — set the browser tab title, and optionally add a caption/watermark (e.g. "Global Research Network 2026") that appears on screen and is included in exports.

---

## 4. Exporting screenshots and video

Open Control Panel → **Export**.

### Screenshot (PNG), at any exact angle
1. Click **Pause Rotation**.
2. Drag with your mouse/touch to orbit the globe to the exact view you want, **or** type values into the Lat/Lng/Alt fields and click **Apply View** (click **Read Current View ↑** first if you want to fine-tune from where you currently are).
3. Pick a **Resolution Multiplier** (2× is a good default for sharp images; 3× for print-quality, but is slower on older machines).
4. Click **📸 Capture Screenshot (PNG)**. The screen will briefly go black while it captures at full resolution, then a PNG downloads automatically.

### Video of the rotation
1. Pick a **Video Resolution** (this is separate from the screenshot resolution above — video re-composites every single frame in real time, so it defaults to 1×/screen-size for smoothness; only raise it if 1× plays back smoothly for you first).
2. Pick **Frame Rate** (24fps is the default — the lowest CPU load and the least likely to stutter; try 30 or 60 only if 24 is smooth).
3. Pick **Duration**: either a **Full rotation (360°)** or a **Custom** length in seconds.
4. Click **⏺ Start Recording**. The live preview hides itself during recording (this is normal — it's rendering off-screen) and the panel shows live progress.
5. It stops automatically at the end of the rotation/duration, or click **■ Stop & Save Video** any time. The video file downloads automatically.

**If recording still looks choppy:** video capture works by re-drawing the globe and every label onto a canvas in real time, which is inherently more demanding than just displaying the globe — how well it keeps up depends on your machine. In order of impact:
1. Close other tabs/apps (especially other GPU-heavy tabs) while recording.
2. Make sure **Video Resolution** is at 1× and **Frame Rate** at 24fps (the smoothest combination).
3. Try a shorter **Custom** duration rather than a full rotation, so you can review a clip quickly and re-record.
4. If it's still not smooth, the recording machine's CPU/GPU is likely the limit — the video quality will still be correct (labels sized and positioned properly), just not perfectly even in playback.

**Output format:** recordings save as **WebM** (the format `MediaRecorder` supports natively in Chrome/Edge/Firefox). If you need an **MP4** for PowerPoint, Instagram, etc., convert it for free with [ffmpeg](https://ffmpeg.org/):

```bash
ffmpeg -i your-video.webm -c:v libx264 -pix_fmt yuv420p -c:a aac your-video.mp4
```

Or use a free web converter (search "webm to mp4 converter") if you don't want to install anything.

**Notes:**
- The control panel itself is never included in screenshots or video — only the globe, labels, and your on-screen caption.
- Recording works best in **Chrome, Edge, or Firefox** on a desktop/laptop. Safari's video recording support is inconsistent — take screenshots there instead, or use another browser for video.

---

## 5. Saving and sharing your setup

Control Panel → **Config**:
- **⬇ Download config.json** — saves everything (locations, colors, speeds, text) into one file. Put it next to `index.html` (same folder) — the page auto-loads it for every visitor, no panel editing required.
- **⬆ Upload config.json** — load a previously saved config back in.
- **💾 Save to This Browser** / **↺ Load From This Browser** — a quick local save (this device only) while you're experimenting, separate from the `config.json` file.
- **Reset to Default Example** — restores the built-in example data (does not touch your `config.json` file).
- The **JSON preview** box shows your current setup and can be pasted elsewhere or edited by hand — see `config.example.json` for the full field reference.

---



## 6. License & attribution

Free for **non-commercial use**, with **attribution required**. Full terms in [LICENSE.md](LICENSE.md) (Creative Commons BY-NC 4.0).

In short: if you use, fork, or adapt this project, please keep a visible credit such as:

> Globe visualization based on work by [github.com/dramanuj](https://github.com/dramanuj)

For commercial use, please reach out to discuss permission.
