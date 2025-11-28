# Mountain Time - Interactive Peak Alignment Viewer

An interactive web application that displays mountain photographs aligned by peak positions, allowing users to scrub through images by chronological order, time of day, or time of year.

## Features

- **Peak-Based Alignment**: Images are dynamically aligned using similarity transformation (rotation + uniform scaling + translation) based on two reference peaks
- **Multiple Timeline Views**:
  - 📅 **Chronological**: Browse images in date/time order
  - 🌅 **Time of Day**: See images ordered by hour:minute (e.g., all sunrise shots together)
  - 🍂 **Time of Year**: View images ordered by month-day (e.g., all January shots together)
- **Zoom & Pan**: Zoom in/out and pan around while maintaining alignment across images
- **Auto-Play Mode**: Watch images cycle through automatically
- **Synchronized Scrubbers**: All three timeline scrubbers update together as you navigate

## Controls

### Mouse/Trackpad
- **Scrub**: Drag any of the three timeline sliders
- **Zoom**: Mouse wheel or use zoom buttons
- **Pan**: Click and drag on the canvas
- **Navigate**: Click Previous/Next buttons

### Keyboard Shortcuts
- `←` / `→` Arrow keys - Navigate between images
- `Space` - Play/Pause auto-play
- `+` / `-` - Zoom in/out
- `0` - Reset zoom and pan

### Touch (Mobile)
- Single finger drag to pan
- Use on-screen controls for navigation and zoom

## Local Development

To run locally:

```bash
cd mountaintime_website
python3 -m http.server 8000
```

Then open http://localhost:8000 in your browser.

## Deploying to GitHub Pages

1. Create a new GitHub repository
2. Push the contents of the `mountaintime_website` folder to the repository:
   ```bash
   cd mountaintime_website
   git init
   git add .
   git commit -m "Initial commit: Mountain Time viewer"
   git branch -M main
   git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
   git push -u origin main
   ```
3. Go to repository Settings → Pages
4. Under "Source", select "Deploy from a branch"
5. Select branch: `main` and folder: `/ (root)`
6. Click Save
7. Your site will be published at `https://YOUR_USERNAME.github.io/YOUR_REPO/`

## File Structure

```
mountaintime_website/
├── index.html              # Main viewer page
├── peaks_database.json     # Image metadata and peak coordinates
├── processed_photos/       # Aligned mountain photographs
│   ├── [64 images]
└── README.md              # This file
```

## How It Works

### Alignment Algorithm

The viewer uses the same similarity transformation algorithm from the Python processing script (`make_database.py`):

1. Each image has two peak coordinates stored in `peaks_database.json`
2. The first image's peak positions are used as reference points
3. For each subsequent image, the algorithm computes:
   - **Scale**: Ratio of distances between peaks
   - **Rotation**: Angle difference between peak vectors
   - **Translation**: Offset to align centroids
4. The transformation is applied in real-time using HTML5 Canvas
5. Zoom and pan are applied as viewport transformations on top of the alignment

### Timeline Sorting

- **Chronological**: Standard datetime sorting
- **Time of Day**: Sorts by `hours * 60 + minutes`, ignoring the date
- **Time of Year**: Sorts by day of year (1-365), ignoring the year

When you switch between views, the current image stays the same, but its position in the timeline changes to reflect where it falls in that sorting order.

## Credits

Data collected from mountain observations. Alignment algorithm implements similarity transformation to preserve aspect ratios and angles.

## Technical Details

- Pure HTML/CSS/JavaScript (no frameworks required)
- Canvas-based rendering with hardware acceleration
- Mobile-friendly responsive design
- Works offline once loaded (all assets are static)

