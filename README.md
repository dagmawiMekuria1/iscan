# TAGSCAN (ITADScanner)

**IT Asset Disposition Scanner** — A browser-based tool for capturing, cataloging, and tracking IT assets through their disposition lifecycle. Uses simulated AI to extract model numbers and serial numbers from device photos.

## What It Does

- **Capture Flow**: Guided 5-step wizard to photograph and catalog IT assets
- **AI Extraction (Mock)**: Simulates Gemini AI to auto-detect model and serial numbers from photos
- **Inventory Management**: Searchable, filterable table with inline editing
- **Disposition Tracking**: Track assets from intake through final disposition with chain-of-custody fields

## How to Run

This is a **static site with zero build steps**. Just serve the files:

### Option 1: Open directly
Double-click `index.html` in your file manager. Note: camera features require HTTPS or localhost.

### Option 2: Local server (recommended)
```bash
# Python 3
python3 -m http.server 8000

# Node.js (if available)
npx serve .
```
Then open [http://localhost:8000](http://localhost:8000)

### Option 3: VS Code Live Server
Install the "Live Server" extension and click "Go Live" from the status bar.

## Project Structure
