🚗 DashcamToGPX — OCR-Only, Live Map GPX Extractor

Version: v30-ocr-live
Extract GPS coordinates from dashcam video purely via OCR (no metadata needed).
Coordinates are parsed live from the on-screen overlay (Garmin style), displayed on a live mini-map, and exported to a clean GPX track.

✨ Features
OCR-Based Coordinate Extraction
Reads latitude/longitude directly from the dashcam overlay (Garmin style lower-bar text).

Live Preview
Shows the active video frame in real time while scanning.

Integrated Live Map
Displays the current recognized coordinate on an OpenStreetMap tile.

Multi-video Batch Processing
Select any number of videos; each processed sequentially.

GPX Output
Saved to:
~/Desktop/OCR/<video_basename>_extracted.gpx

ROI Pre-Configured for Garmin Dashcams
Reads the bottom 10% of the screen, between 32–54% horizontal width.

Date/Time Options
Auto-extract date from filename (yyyy.mm.dd)

Or manually set datetime

OCR Optimization Pipeline
The tool tries:
- Upscaling
- Histogram equalization
- Median+Otsu thresholding
- Adaptive threshold
- Morphological correction
- Inverted variants
- Multiple Tesseract PSM modes

Safety Filters
Removes unrealistic point jumps (>300 km/h)
Removes near-duplicate points (<2 meters apart)

📝 Requirements
You need:
* Python
- Python 3.9 or newer

* Python Libraries
Install with:
pip install opencv-python numpy pillow pytesseract staticmap tkintermapview

* Tesseract OCR
You must have Tesseract installed and accessible:
  - Windows:
    Install from: https://github.com/UB-Mannheim/tesseract/wiki
    Add to PATH.
  - Linux:
    sudo apt install tesseract-ocr
  - macOS:
    brew install tesseract

📁 Output
Extracted GPX files go to:
~/Desktop/OCR/

Example:
video001_extracted.gpx
2024.06.12_drive_extracted.gpx

Each GPX track includes:
- Timestamp per point (from filename or user input)
- Latitude, longitude (6 decimals)
- Zero elevation
- 1-second sampling

🖥️ Usage
1. Start the application
python DashcamToGPX.py

2. Choose your videos
Click "Select Video(s) & Start"

3. Select date/time mode
- Use date from filename (default)
  Expects yyyy.mm.dd inside filename (if file is named 2025.11.28_dashcam, then import 28/11/2025)
- Manual

4. Let it run
For each second of video:
- OCR is applied to the ROI
- Coordinates appear in the center log
- Map updates with the latest point, on the right
- GPX is written at end of file

📷 UI Overview

<img width="1748" height="927" alt="image" src="https://github.com/user-attachments/assets/e520216b-a20f-482f-b3a6-fa6c020a3476" />



STRUCTURE:

<img width="407" height="125" alt="image" src="https://github.com/user-attachments/assets/5c1627c7-03af-43c9-b701-735544ddc912" />


🎯 ROI (Region of Interest)
Hard-coded for Garmin dashcams:
ROI_DEFAULT = (0.32, 0.54, 0.90, 1.00)

Meaning:
Horizontal: from 32% to 54%
Vertical: lower 10%

You may adjust if your overlay differs.

📦 Code Structure
OCR Pipeline
/preprocess_attempts_for_ocr, /ocr_try_all, /extract_coords_from_text

Map Rendering
Uses staticmap to fetch OSM tiles and draw single red marker

Video Reader
OpenCV, frame-by-frame seeking

GUI
Tkinter (frames, scrolled logs, progress bar, radiobutton options)

GPX Writer
Simple inline XML output

🔍 OCR Limitations & Tips
- High-contrast overlays work best
- If coordinates flicker, use multiple shifted frames (0–8 px)
- Upscaled & thresholded variants significantly improve recognition
- Avoid videos with shaky overlays or fonts below 10px height

🧩 Example Filename Parsing
Filename:
2025.01.03_mytrip.mp4

App extracts:
Start time = 2025-01-03 00:00:00

🛠️ Roadmap (optional)
- Multi-point map preview (currently disabled to maximize app stability)
- Full GPX smoothing filter (developing separate app)
- GPX elevation interpolation (developing separate app)

📜 License
Free to use.
