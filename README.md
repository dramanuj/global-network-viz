# Customizable Institutions Globe

An interactive, rotating 3D globe that highlights a network of institutions (universities, research centers, offices — anything with a location) with animated labels, country highlighting, and a built-in **Control Panel** for customizing everything without touching code. Includes one-click **screenshot** and **video export** of the rotation.

Demo: [http://dramanuj.github.io/global-network-viz/index.html](http://dramanuj.github.io/global-network-viz/index.html)

Built on [globe.gl](https://globe.gl) / [three.js](https://threejs.org).

> **Attribution required.** This project is free to use for **non-commercial purposes only**, provided you credit the original author. See [LICENSE.md](LICENSE.md) for the full terms.
>
> Original design & build: **[github.com/dramanuj](https://github.com/dramanuj)** with support from Claude Code.

---

## 1. Contents

- `index.html` — the whole app (globe + control panel). No build step, no dependencies to install.
- `config.example.json` — an example configuration you can copy to `config.json` to customize the globe **without editing any code**.
- A live **Control Panel** (gear icon, top-right) with tabs for:
  - **Locations** — add/remove/edit universities or any custom locations (name, lat/lng, country, priority, colors).
  - **Countries** — auto-populated from your locations' Country fields; recolor or toggle each one's highlight on/off.
  - **Style** — globe colors, atmosphere, marker colors, marker icon.
  - **Rotation** — the guided tour: travel speed, how locations get grouped into stops, camera angle, label timing.
  - **Text** — page title and an optional on-screen caption/watermark (position, color, size).
  - **Export** — pause/orbit the camera manually, capture a PNG screenshot at any angle, or record an MP4/WebM-style video of the rotation.
  - **Config** — download/upload your whole setup as a `config.json` file, save/load it in the browser, or paste JSON directly.

---

## 2. Quick start 

1. Download this folder (or clone the repo — see hosting instructions below).
2. Double-click `index.html` to open it in your browser (Chrome, Edge, or Firefox recommended).
3. Click the **⚙ gear icon** top-right to open the Control Panel.
4. Go to **Locations** and edit the sample universities, or add your own with **+ Add Location**. (The included demo data is DTU's real alliance and strategic-partner universities, as a working example — replace it with your own network.)
   - The globe visits every location in a guided tour: it travels to each one (or each nearby *group* of locations — see "Cluster Grouping Distance" below) and pauses there, camera properly centered, before moving to the next. This guarantees every location actually gets shown, however they're spread across the map. Labels fade in based on how close they are to wherever the camera currently is — every nearby location shows at once, not one at a time.
   - `Priority`:
     - **Normal** / **Always** — shows whenever it's near the current view (identical behavior; "Always" is just a naming convenience for your main/highlighted location).
     - **Sequential** — takes turns with other "Sequential" locations instead of showing simultaneously. Use this only for a handful of locations packed too closely together to show all their label boxes at once without overlapping.
5. Go to **Countries** — this list is automatic, built from your locations' Country fields; use it to recolor or toggle a country's highlight on/off.
6. Click **✔ Apply Changes to Globe** (bottom of the panel) to see your edits.
7. When you're happy, go to **Config → ⬇ Download config.json** and save that file **in the same folder as `index.html`**. From now on, the page will load your saved setup automatically for anyone who opens it — no panel editing needed each time.

That's it — you now have a fully customized globe.

---

## 3. Customizing in detail

### Adding a location
Control Panel → **Locations** → **+ Add Location**, then fill in:
- **Name** — label text shown on the globe.
- **Latitude / Longitude** — decimal degrees (e.g. Copenhagen ≈ `55.68, 12.57`). Look coordinates up on Google Maps: right-click a spot → the numbers shown are lat, lng.
- **Country** — must match the country's official English name as it appears on the world map (e.g. `"United States of America"`, not `"USA"`) if you also want that country highlighted. This also drives the Countries tab (see below) — it's built entirely from whatever's typed here.
- **Priority** — `normal`, `always`, or `sequential` (see above).
- **Highlight** — check this to make the location stand out using the "Highlighted Marker Style" colors (Style tab) — e.g. for your own institution.
- Optional per-location color overrides (point/label/border) if you don't want it to use the theme defaults.

### Highlighting countries
Control Panel → **Countries**. This list isn't edited directly — it's automatically built from every location's Country field, and stays in sync as you add, remove, or retype one (add/remove a location with that country, or edit a location's Country field, to change what's listed here). For each country listed, you can:
- Pick a different highlight **color**.
- Uncheck the box next to it to **hide its highlight** without deleting any locations — the country stays listed (in case you re-enable it later), it just won't be tinted on the globe.

### Changing the look
Control Panel → **Style** — background, atmosphere glow, globe surface color, default vs. highlighted marker colors, and the marker icon (defaults to 📍, but can be any emoji or short text).

### Rotation: the guided tour
Control Panel → **Rotation**. The globe doesn't just spin continuously — it runs a guided tour: group nearby locations into "stops," travel eastward from one stop to the next, and pause at each one with the camera centered on it (both longitude *and* latitude) before moving on. This is what guarantees every location — Northern or Southern Hemisphere, isolated or clustered — actually gets shown, rather than sweeping past too fast to see.

- **Travel Speed** — how fast the globe spins while moving between stops (degrees/second).
- **Cluster Grouping Distance** — locations within this many degrees of longitude of each other are grouped into one stop and shown together, taking turns. This is the setting most worth tuning for your own data: a smaller value gives every location its own dedicated stop (good if your locations are spread out); a larger value groups more into shared stops (good if you have several locations clustered in one region, so the tour doesn't pause at each one individually).
- **Starting Camera Latitude / Longitude + Camera Altitude** — where the camera starts before the tour reaches its first stop, and how zoomed in the view is throughout.
- **Label Duration Per Location** — when a stop has multiple locations, how long each one is shown before cycling to the next (a stop's total dwell time is this × however many locations share it).
- **Reset Tour to Start** — restarts the tour from the beginning (handy after changing these settings).

With many worldwide locations, most of a full tour is spent paused at stops rather than in transit — that's expected, and is exactly what keeps every location from being missed.

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
