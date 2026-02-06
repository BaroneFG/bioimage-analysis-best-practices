# 🔬 Fiji/ImageJ Quickstart for Bioimage Analysis
A beginner-friendly, quantitative-first guide to common Fiji workflows, essential shortcuts, and frequent pitfalls in microscopy image analysis. This repository is meant to be practical: what to click, what to avoid, and how to keep your measurements reproducible.

---

## Who this is for
- New users learning Fiji/ImageJ for fluorescence microscopy.
- Anyone doing **quantification** (intensity, puncta, area, colocalization).
- Labs that want a shared “house style” for analysis.

## What this is (and isn’t)
**This is:** shortcuts, workflow checklists, reproducibility tips, macro snippets, and common failure modes.
**This is not:** a full textbook, segmentation theory, or a substitute for proper experimental design.

---

## A minimal “safe” workflow (quantification-first)
Use this as your default until you know exactly why you’re deviating.

1.  **Open image → duplicate it**
    * Never work on the raw file.
    * `Image > Duplicate...` (`Ctrl+Shift+D` / `⌘+Shift+D`)
2.  **Confirm metadata**
    * Pixel size, bit depth, channels, Z-stacks, timepoints.
    * `Image > Show Info...`
3.  **Set scale (only if needed)**
    * Use metadata if present; avoid manual scale guesses.
    * `Analyze > Set Scale...`
4.  **Define the measurement plan**
    * Decide *beforehand* what you measure: area, mean intensity, integrated density, puncta count, etc.
    * Set it once: `Analyze > Set Measurements...`
5.  **Do segmentation / ROI selection**
    * Save ROIs (ROI Manager) for reproducibility.
6.  **Measure**
    * Measure on **raw grayscale channels**, not RGB.
7.  **Export**
    * Save results tables + ROIs + analysis settings (threshold values, filters, versions).

---

## Essential Shortcuts (Windows & macOS)
**Note for macOS users:** Most `Ctrl` shortcuts map to `⌘ Command`.

### Core Productivity
| Action | Windows / Linux | macOS | Why it matters |
| :--- | :--- | :--- | :--- |
| **Command Finder** | **L** | **L** | Fastest way to find anything ("Gaussian", "Watershed", "Max"). |
| **Add to ROI Manager** | **T** | **T** | Saves current selection as an ROI. |
| **Duplicate** | **Ctrl+Shift+D** | **⌘+Shift+D** | Protect your raw data (never edit the original). |
| **Brightness/Contrast** | **Ctrl+Shift+C** | **⌘+Shift+C** | Display adjustment (does not change pixels unless you Apply). |
| **Measure** | **M** | **M** | Measures current ROI using your defined settings. |
| **Zoom in / out** | **+ / -** | **+ / -** | Faster navigation than tools. |
| **Channels Tool** | **Shift+J** | **Shift+J** | Quickly toggle channels in composite images. |
| **Flatten** | **Shift+F** | **Shift+F** | Burns overlay into RGB for figures (do NOT use for quantification). |
| **Histogram** | **Ctrl+H** | **⌘+H** * | *On macOS, if `⌘+H` hides the app, use `L` -> "Histogram" instead. |

### Navigation & Viewing
| Action | Shortcut | Notes |
| :--- | :--- | :--- |
| **Scroll to zoom** | Mouse wheel / Trackpad | Very fast for inspection. |
| **Pan (move)** | Space + Drag | Hold Spacebar, then click and drag image. |
| **Next / Prev Slice** | `<` / `>` | Moves through Z-stack (or use sliders). |

### Macro & Automation
| Action | Shortcut | Why it matters |
| :--- | :--- | :--- |
| **Macro Recorder** | Use `L` → "Record..." | Records your clicks as code. The best way to learn scripting. |
| **Run Macro** | Use menu | `Plugins > Macros > Run...` |
| **Batch Process** | Use menu | `Process > Batch > Macro...` |

> **Pro Tip:** If a shortcut doesn't work or conflicts with your OS, press **`L`** and type the command name directly.

---

## Common Pitfalls (and how to avoid them)

### 1) Measuring intensity on RGB images
* **Problem:** RGB is for display. It merges channels and rescales values (0-255), destroying raw data.
* **Fix:** Quantify on **raw grayscale** images per channel (ideally 16-bit). Use **Composite** view for looking, split channels for measuring.

### 2) JPEG for analysis
* **Problem:** JPEG compression adds artifacts that bias segmentation and feature detection.
* **Fix:** Use **TIFF** for analysis. Only use **PNG** for presentation slides if needed.

### 3) Brightness & Contrast confusion
* **Problem:** `Ctrl+Shift+C` changes how the image *looks* on your screen, not the actual pixel values—unless you click **Apply**.
* **Fix:** Adjust B&C freely for viewing. **Never click Apply** if you intend to measure intensity later.

### 4) "Auto" threshold without documentation
* **Problem:** "Auto" picks a method (Otsu, Yen, Li) that may change based on image histogram.
* **Fix:** Record the specific method used (e.g., "Thresholded using Otsu").

### 5) Measuring without "Set Measurements"
* **Problem:** The default settings might not include what you need (e.g., Integrated Density).
* **Fix:** Always run `Analyze > Set Measurements...` before starting. Common defaults: *Area, Mean gray value, Integrated density, Min/Max gray value*.

### 6) Saturation (Clipping)
* **Problem:** If pixels are maxed out (e.g., 255 in 8-bit or 65535 in 16-bit), you have lost data. Intensity comparisons become invalid.
* **Fix:** Check `Ctrl+H` (Histogram). Avoid saturated acquisitions at the microscope.

### 7) Manually drawing ROIs without saving
* **Problem:** Your analysis is not reproducible.
* **Fix:** Always use ROI Manager. Click `More > Save...` to export a `.zip` file of your ROIs.

---

## ROI Manager Best Practices
1.  **Name consistently:** Rename ROIs (e.g., `cell_01`, `cell_02`) for easier data matching later.
2.  **Save the Zip:** Always save the ROI set (`.zip`) in the same folder as your results.
3.  **Show All:** Enable "Show All" in ROI Manager to see your selections over the image.

---

## Exporting Results (The Right Way)
* **Results Table:** `File > Save As...` (Save as `.csv` for Excel/Python).
* **ROIs:** ROI Manager → `More > Save...` (Save as `.zip`).
* **Figures:** Duplicate image → Add Scale Bar → `Shift+F` (Flatten) → Save as `.png` or `.tif`.

---

## Macro Snippets (Copy/Paste)
Use the Script Editor (`[` key) to run these one-liners.

**Close All Windows (Panic Button):**
```java
run("Close All");
