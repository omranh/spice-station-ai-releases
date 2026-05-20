# SPICE Station AI

SPICE Station AI is an AI-native waveform viewer for ngspice and LTspice built for a fast, fluid analysis workflow. Open one or more `.raw` files and immediately get a clean, interactive plot: zoom and pan with the mouse, style traces, place dual cursors, and read off precise values with engineering-prefix formatting.

Parametric sweep files are handled natively: all sweep steps appear grouped under a single file header, with labels in the legend for easy comparison. The math expression bar lets you derive new traces on the fly — gain in dB, phase, differential signals, FFTs — directly from the simulation data. The signal browser has a live filter box: type a substring to narrow the list, then use **Plot visible** to plot only the matching signals in one click.

Sessions and scripts make your analysis reproducible. Save the full workspace to a `.sssession` file, or export a `.ssscript` command log and replay it headlessly to regenerate PNG or CSV outputs automatically. 

A built-in AI chat assistant lets you control the view in plain English — *"plot v(out) and v(in)"*, *"plot the gain in dB and color it red"*, *"add cursor A at 10n"* — with support for local (Ollama), Claude Code, and cloud (Groq, Anthropic, Gemini) providers.

![SPICE Station AI](https://raw.githubusercontent.com/omranh/spice-station-ai-releases/main/ssai.png)

---

## Download

| Platform | File |
|---|---|
| **Linux** (x86-64) | `spice_station_ai-vX.Y.Z-linux-x86_64.tar.gz` |
| **Windows** (x86-64) | `spice_station_ai-vX.Y.Z-windows-x86_64.zip` |

→ **[Latest release](https://github.com/omranh/spice-station-ai-releases/releases/latest)**

No Python installation required. Each archive is fully self-contained.

---

## Install

### Linux

```bash
tar -xzf spice_station_ai-vX.Y.Z-linux-x86_64.tar.gz
cd spice_station_ai-vX.Y.Z-linux-x86_64
./spice_station_ai
```

### Windows

1. Right-click the zip → **Extract All…**
2. Open the extracted folder and double-click **`spice_station_ai.exe`**

> **Windows SmartScreen:** click **More info → Run anyway** on first launch. The executable is not code-signed.

---

## What's inside the archive

```
spice_station_ai-vX.Y.Z-linux-x86_64/
├── spice_station_ai      ← run this
├── USER_GUIDE.html       ← full user guide (open in any browser)
├── examples/             ← sample .raw files to try immediately
│   ├── ltspice_17_1_6/
│   ├── ltspice_26_0_2/
│   └── ngspice/
└── _internal/            ← runtime libraries (Qt, numpy, etc.)
```

Open `USER_GUIDE.html` for the full reference, or use **Help → User guide** inside the app.

---

## Features

**Plotting**
- ngspice and LTspice `.raw` files — format detected automatically
- Multiple files open simultaneously; traces labelled by filename
- Parametric sweeps grouped under a single file header with `[param=value]` labels
- Signal browser with live filter box and **Plot visible** for one-click bulk plotting
- Multiple plots, stacked vertically, with independent Y axes and shared X

**Interaction**
- Scroll to zoom X · Shift/Ctrl+scroll to zoom Y · left-drag to pan · right-drag for rubber-band zoom
- `Shift+X` / `Shift+Y` to autoscale a single axis without affecting the other
- Dual cursors (A / B) spanning all plots — ΔX, 1/ΔX, and Y readout per trace
- Cursor exact positioning: double-click the X tag to type a value with SI suffixes (`5n`, `1k`, `2.5meg`)
- Cursor Y-value positioning: double-click a Y label on the cursor to move it to a target Y value (Nearest / Left / Right); cursor B also accepts ΔY from A
- Trace style editor: color, line style, width, marker, opacity

**Math expressions**
```
db(V(out) / V(in))
V(out) - V(in)
diff(V(out)) / diff(time)
fft(V(out))
```

**Export**
- PNG and CSV via **File → Export PNG…** / **File → Export CSV…** (all plots) or right-click a plot for single-plot export
- CSV range-clipped to cursor window when both cursors are visible
- Session save/restore (`.sssession`)
- Script export and headless replay (`.ssscript`)

**Crash recovery** — a recovery script is written after every action; use **File → Replay last session** to restore after a crash.

---

## Rendering

Open **Edit → Settings… → Rendering** to adjust rendering. Changes take effect after restarting the app.

**OpenGL rendering** — dramatically faster zoom and pan, especially on large datasets. Requires working GPU drivers. Enable this if zoom and pan feel sluggish. Not recommended on WSL2 or virtual machines.

**Antialiasing** — smoother trace lines. Runs well with OpenGL; noticeably slower on the software renderer.

---

## AI assistant

Open **Edit → Settings… → AI Provider** to choose a provider and save the configuration. Changes take effect immediately without restarting.

### Ollama (recommended — local, no API key, no internet)

1. Install Ollama from [ollama.com](https://ollama.com)
2. Pull a model:
   ```bash
   ollama pull granite4.1:3b   # default, best tool-calling at ~2 GB VRAM
   ```
3. Start the server (Linux/macOS):
   ```bash
   ollama serve
   ```
   On Windows, Ollama runs as a background service after install — no manual step needed.
4. Launch the app — Ollama is the default provider, no further setup needed.

### Groq (cloud, free limited tier, no GPU)

1. Get a free key at [console.groq.com](https://console.groq.com)
2. Set the `GROQ_API_KEY` environment variable so the app can read it (add to `~/.bashrc` or `~/.zshrc` to persist):
   ```bash
   export GROQ_API_KEY=gsk_...
   ```
   Then launch the app from a new terminal so the variable is in the environment.
3. Open **Edit → Settings…**, select **Groq**, and click OK.

### Claude Code (requires Claude Max or Pro subscription)

1. Install Node.js, then:
   ```bash
   npm install -g @anthropic-ai/claude-code
   claude auth login
   ```
2. Open **Edit → Settings…**, select **Claude Code (claude CLI)**, and click OK.

### Example prompts

```
Plot v(out).
Plot v(out) and v(in) each on their own plot.
Add a dB gain trace: db(v(out)/v(in)).
Set the X axis to log scale.
Place cursor A at 1000 Hz.
Make the v(out) trace red.
Split traces.
Export all plots as PNG.
```

---

## Keyboard shortcuts

| Key | Action |
|---|---|
| `Ctrl+O` | Open .raw file |
| `Ctrl+R` | Reload all files |
| `Ctrl+S` | Save session |
| `Ctrl+Q` | Quit |
| `F` | Zoom fit (X + Y) |
| `Shift+X` | Autoscale X only |
| `Shift+Y` | Autoscale Y only |
| `A` / `B` | Place / toggle cursor A / B |
| `Ctrl+Shift+N` | Add plot |
| `Delete` | Remove selected plot |
| `Backspace` | Remove selected trace |
| `Ctrl+Backspace` | Clear all traces (active plot) |
| `Ctrl+Shift+D` | Toggle dark mode |

All shortcuts are customizable via **Edit → Settings… → Keyboard Shortcuts**.
