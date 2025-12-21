# Montreal LiDAR 3D Viewer

Interactive 3D visualization of Montreal LiDAR point cloud data in your browser.

![LiDAR Viewer](https://img.shields.io/badge/LiDAR-3D%20Viewer-00d9ff?style=for-the-badge)
![Three.js](https://img.shields.io/badge/Three.js-WebGL-ff00aa?style=for-the-badge)
![License](https://img.shields.io/badge/License-CC--BY%204.0-green?style=for-the-badge)

## 🚀 Live Demo

**[View Live Demo](https://yourusername.github.io/montreal-lidar-viewer/)**

## 📋 About

This project visualizes high-resolution LiDAR (Light Detection and Ranging) point cloud data from Montreal, Quebec. The data is sourced from the Quebec Government's open data portal and rendered in an interactive 3D web viewer.

### Features

- ✨ **Interactive 3D Navigation** - Rotate, pan, and zoom through the point cloud
- 🎨 **Multiple Color Modes** - View by elevation, RGB, intensity, or classification
- 🎛️ **Customizable Rendering** - Adjust point size, density, and height exaggeration
- 📱 **Responsive Design** - Works on desktop, tablet, and mobile
- ⚡ **Fast Loading** - Optimized binary format for quick load times
- 🌈 **Color Palettes** - Choose from multiple scientific color schemes

## 🗂️ Repository Structure

```
montreal-lidar-viewer/
├── index.html              # Main viewer application
├── web_data/              # Processed LiDAR data (generated)
│   ├── points.bin         # Binary point cloud data
│   ├── metadata.json      # Dataset metadata
│   └── points_sample.json.gz  # Small sample for testing
├── laz_to_web.py          # Data conversion script
├── README.md              # This file
└── LICENSE                # CC-BY 4.0 License
```

## 📦 Setup & Installation

### Option 1: Use Pre-Converted Data (Easiest)

1. **Clone this repository:**
   ```bash
   git clone https://github.com/yourusername/montreal-lidar-viewer.git
   cd montreal-lidar-viewer
   ```

2. **Enable GitHub Pages:**
   - Go to your repository Settings
   - Navigate to "Pages"
   - Source: Deploy from branch "main"
   - Folder: / (root)
   - Click Save

3. **Access your viewer:**
   - Your site will be at: `https://yourusername.github.io/montreal-lidar-viewer/`

### Option 2: Convert Your Own LAZ Files

If you want to process your own LiDAR data:

1. **Install Python dependencies:**
   ```bash
   pip3 install laspy numpy
   ```

2. **Convert your LAZ file:**
   ```bash
   # Basic conversion
   python3 laz_to_web.py 23_2995040F08_DC.laz
   
   # With downsampling (recommended for large files)
   python3 laz_to_web.py 23_2995040F08_DC.laz --downsample 5
   
   # Limit to specific number of points
   python3 laz_to_web.py 23_2995040F08_DC.laz --max-points 500000
   ```

3. **The script will create a `web_data/` folder with:**
   - `points.bin` - Binary point cloud (efficient)
   - `metadata.json` - Dataset information
   - `points_sample.json.gz` - Compressed sample

## 🚫 Handling GitHub's 100MB File Size Limit

GitHub has a 100MB file size limit. Here's how to handle large LiDAR files:

### Method 1: Downsample the Data (Recommended)

```bash
# Keep every 5th point (reduces file size by 80%)
python3 laz_to_web.py your_file.laz --downsample 5

# Or limit to a specific number of points
python3 laz_to_web.py your_file.laz --max-points 1000000
```

### Method 2: Use Git LFS (Large File Storage)

For files 100MB - 5GB:

```bash
# Install Git LFS
git lfs install

# Track large files
git lfs track "*.laz"
git lfs track "web_data/points.bin"

# Add and commit
git add .gitattributes
git add your_files
git commit -m "Add LiDAR data"
git push
```

⚠️ **Note:** Git LFS has bandwidth limits on free accounts (1GB/month)

### Method 3: Split into Chunks

For very large files:

```bash
# Create chunked version
python3 laz_to_web.py your_file.laz --chunked

# This creates multiple smaller files that load progressively
```

### Method 4: Host Externally

For files > 5GB:
- Upload to: Google Drive, Dropbox, AWS S3, or CloudFlare R2
- Update `index.html` to load from external URL
- Enable CORS on your hosting service

Example for S3:
```javascript
const dataUrl = 'https://your-bucket.s3.amazonaws.com/web_data/points.bin';
```

## 🎮 Controls

| Action | Control |
|--------|---------|
| Rotate view | Left click + drag |
| Pan view | Right click + drag |
| Zoom | Mouse wheel |
| Reset camera | Click "Reset View" button |

## 🎨 Customization

### Changing Colors

Edit the color palettes in `index.html`:

```javascript
const palettes = {
    viridis: ['#440154', '#414487', '#2a788e', '#22a884', '#7ad151', '#fde725'],
    // Add your custom palette here
    custom: ['#yourcolor1', '#yourcolor2', '#yourcolor3']
};
```

### Adjusting Performance

In `index.html`, modify these settings:

```javascript
// Reduce point count for better performance
const maxPoints = 500000;  // Default: 1000000

// Adjust point size
pointCloud.material.size = 0.05;  // Smaller = faster

// Reduce fog distance
scene.fog = new THREE.Fog(0x0a0e14, 50, 500);  // Shorter range
```

## 📊 Data Source

The LiDAR data comes from:
- **Source:** Gouvernement du Québec - Ministère des Ressources naturelles et des Forêts
- **Dataset:** Données lidar du Québec
- **License:** CC-BY 4.0
- **Link:** https://www.donneesquebec.ca/recherche/dataset/donnees-lidar-du-quebec

### Citation

```
MINISTÈRE DES RESSOURCES NATURELLES ET DES FORÊTS. Données lidar du Québec, 
[Jeu de données], dans Données Québec, 2025, mis à jour le 04 décembre 2025. 
[https://www.donneesquebec.ca/recherche/dataset/donnees-lidar-du-quebec]
```

## 🛠️ Technical Details

### Data Format

The viewer uses a custom binary format for efficiency:
- **Positions:** Float32 (x, y, z) per point
- **Colors:** UInt8 (r, g, b) per point (optional)
- **Classification:** UInt8 per point (optional)
- **Intensity:** UInt16 per point (optional)

### Browser Requirements

- Modern browser with WebGL support
- Recommended: Chrome 90+, Firefox 88+, Safari 14+
- Minimum 4GB RAM for large datasets

### Performance Tips

1. **Downsample data** - Keep every Nth point
2. **Use point size carefully** - Larger points = slower rendering
3. **Reduce density** - Adjust in control panel
4. **Close other tabs** - Free up GPU memory

## 🤝 Contributing

Contributions welcome! Feel free to:
- Report bugs
- Suggest features
- Submit pull requests
- Add new visualization modes

## 📄 License

- **Code:** MIT License
- **Data:** CC-BY 4.0 (Quebec Government Open Data)

When using this viewer or data, please cite the original data source.

## 🔗 Links

- [Quebec Open Data Portal](https://www.donneesquebec.ca/)
- [Three.js Documentation](https://threejs.org/docs/)
- [LiDAR Data Download Map](https://lidar-telechargement.portailcartographique.gouv.qc.ca/)

## 💡 Tips for Other Cities

Want to create a similar viewer for your city?

1. **Find LiDAR data:**
   - Check your local/state open data portal
   - USGS (USA): https://www.usgs.gov/3d-elevation-program
   - OpenTopography: https://opentopography.org/

2. **Download LAZ/LAS files**

3. **Run the conversion script:**
   ```bash
   python3 laz_to_web.py your_city_data.laz --downsample 3
   ```

4. **Update `index.html` metadata:**
   - Change title and location info
   - Adjust camera initial position if needed

5. **Deploy to GitHub Pages!**

## 📞 Support

Questions? Issues?
- Open an issue in this repository
- Email: your-email@example.com

---

**Made with ❤️ for open data visualization**

*Powered by Three.js and Quebec Open Data*
