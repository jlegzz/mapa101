# GIS Parcel Viewer

A complete web-based GIS application for visualizing parcel data from shapefiles. This application runs entirely in the browser without requiring any server-side components, making it perfect for static hosting platforms.

## Features

- 🗺️ **Interactive Map**: Built with Leaflet.js and OpenStreetMap tiles
- 📁 **Shapefile Support**: Load .shp files with associated .shx, .dbf, and .prj files
- 🌐 **Coordinate Transformation**: Automatic reprojection from projected coordinate systems (like WKID 3125) to WGS84
- 🔍 **Search Functionality**: Search parcels by any attribute (ID, owner, etc.)
- 📱 **Responsive Design**: Works on desktop and mobile devices
- 🎨 **Interactive Popups**: Click parcels to view detailed information
- 🎯 **Visual Highlighting**: Hover effects and search result highlighting
- 📊 **Legend**: Clear visual legend for map layers

## Quick Start

1. **Open the Application**: Simply open `index.html` in any modern web browser
2. **Upload Shapefile**: Use the file input to upload your shapefile data
3. **Explore**: Click parcels to view details, use search to find specific parcels

## File Upload Options

### Option 1: Single ZIP File (Recommended)
- Create a ZIP file containing your shapefile components (.shp, .shx, .dbf, .prj)
- Upload the single ZIP file using the file input

### Option 2: Multiple ZIP Files
- Select multiple ZIP files, each containing a complete shapefile
- The application will automatically merge all shapefiles into a single map layer
- Perfect for loading multiple regions, districts, or different datasets
- Each parcel will show its source file in the popup for easy identification

### Option 3: GeoJSON Files (New!)
- Upload single or multiple .geojson or .json files
- Supports FeatureCollection, Feature, and Geometry objects
- No coordinate transformation needed if already in WGS84
- Perfect for web-based GIS data and modern mapping workflows
- Multiple GeoJSON files are automatically merged with source tracking

### Option 4: Individual Shapefile Components
- Select multiple files: the .shp file and any associated files (.shx, .dbf, .prj)
- The application will automatically combine them

## File Requirements

### For Shapefiles:
- **Required**: `.shp` file (contains the geometry)
- **Recommended**: 
  - `.dbf` file (contains attribute data for popups)
  - `.shx` file (shape index file)
  - `.prj` file (projection information)

### For GeoJSON:
- **Required**: `.geojson` or `.json` file with valid GeoJSON structure
- **Supported Types**: FeatureCollection, Feature, or Geometry objects
- **Coordinates**: Should be in WGS84 (EPSG:4326) for best results
- **Properties**: Feature properties will be displayed in popups

## Deployment Instructions

### GitHub Pages

1. Create a new GitHub repository
2. Upload `index.html` to the repository
3. Go to repository Settings → Pages
4. Select "Deploy from a branch" and choose "main"
5. Your app will be available at `https://yourusername.github.io/repository-name`

### Netlify

1. Visit [netlify.com](https://netlify.com)
2. Drag and drop the folder containing `index.html`
3. Your app will be instantly deployed with a custom URL
4. Optional: Connect to GitHub for automatic deployments

### Vercel

1. Visit [vercel.com](https://vercel.com)
2. Import your project from GitHub or upload files
3. Deploy with zero configuration
4. Get a custom domain instantly

### Other Static Hosting Options

- **Firebase Hosting**: `firebase deploy`
- **Surge.sh**: `surge .`
- **AWS S3**: Upload to S3 bucket with static website hosting
- **Azure Static Web Apps**: Deploy directly from GitHub

## Usage Guide

### Loading Data

1. **Prepare Your Shapefile**:
   - Ensure you have at least a .shp file
   - For best results, include .dbf for attribute data
   - ZIP all files together for easier upload

2. **Upload Process**:
   - Click "Choose Files" or drag files to the upload area
   - Select your ZIP file or individual shapefile components
   - Wait for processing (status will show progress)

3. **View Results**:
   - Map will automatically zoom to show all parcels
   - Parcels appear as blue polygons with semi-transparent fill

### Interacting with Parcels

- **Click**: View detailed information in a popup
- **Hover**: Highlight individual parcels
- **Search**: Use the search bar to find specific parcels
- **Zoom/Pan**: Standard map navigation controls

### Search Functionality

- Enter any text to search across all parcel attributes
- Examples: parcel ID, owner name, address, zone type
- Matching parcels will be highlighted in red
- Map automatically zooms to show search results
- Use "Clear" to reset search and view all parcels

## Coordinate System Support

### Automatic Transformation

The application automatically handles coordinate system transformations:

- **Detects** projected coordinate systems from .prj files or coordinate ranges
- **Transforms** data to WGS84 (EPSG:4326) for web display
- **Supports** common Philippine projections including WKID 3125 (PRS92 Philippines Zone III)
- **Handles** UTM zones and other projected systems

### Supported Coordinate Systems

- **EPSG:3125** - PRS92 / Philippines zone III (WKID 3125)
- **EPSG:4326** - WGS84 Geographic (default web standard)
- **EPSG:32651** - WGS84 / UTM zone 51N (Philippines)
- **Auto-detection** for other common projections

### How It Works

1. **CRS Detection**: Reads .prj file or analyzes coordinate ranges
2. **Transformation**: Uses proj4js to reproject coordinates to WGS84
3. **Display**: Shows properly positioned data on the web map
4. **Fallback**: If transformation fails, displays original data with warning

## Technical Details

### Libraries Used

- **Leaflet.js 1.9.4**: Interactive mapping library
- **shpjs**: Shapefile parsing in the browser
- **proj4js 2.9.0**: Coordinate system transformations
- **OpenStreetMap**: Base map tiles

### Browser Compatibility

- Chrome 60+
- Firefox 55+
- Safari 12+
- Edge 79+

### File Size Limitations

- Browser memory limits apply (typically 100MB+ for modern browsers)
- Large shapefiles may take longer to process
- Consider simplifying geometry for very large datasets

## Customization

### Styling Parcels

Modify the `getParcelStyle` function in the JavaScript:

```javascript
getParcelStyle(feature) {
    return {
        fillColor: '#your-color',    // Fill color
        weight: 2,                   // Border width
        opacity: 1,                  // Border opacity
        color: '#border-color',      // Border color
        fillOpacity: 0.3            // Fill opacity
    };
}
```

### Adding Custom Attributes

The popup automatically displays all attributes from the .dbf file. To customize which attributes are shown, modify the `createPopupContent` function.

### Changing Base Map

Replace the OpenStreetMap tiles with other providers:

```javascript
// Example: Satellite imagery
L.tileLayer('https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}', {
    attribution: 'Esri, DigitalGlobe, GeoEye, Earthstar Geographics'
}).addTo(this.map);
```

## Troubleshooting

### Common Issues

1. **"No .shp file found"**
   - Ensure you're uploading a .shp file
   - Check file extensions are correct

2. **"Error processing files"**
   - Try uploading as a ZIP file instead
   - Ensure all files are from the same shapefile dataset

3. **Map doesn't show parcels**
   - Check browser console for errors
   - Verify shapefile contains valid geometry
   - Try a different shapefile to test

4. **Parcels appear in wrong location**
   - Include .prj file with coordinate system information
   - The app automatically detects and transforms common projections (WKID 3125, UTM zones)
   - Check that your coordinate system is supported

5. **Search not working**
   - Ensure shapefile includes .dbf file with attributes
   - Check that search term matches attribute values

### Performance Tips

- Use simplified geometries for large datasets
- Consider splitting very large shapefiles into smaller regions
- ZIP files typically load faster than individual files

## License

This project is open source and uses the following libraries:
- Leaflet.js (BSD 2-Clause License)
- shpjs (MIT License)
- OpenStreetMap data (Open Database License)

## Support

For issues or questions:
1. Check the browser console for error messages
2. Verify your shapefile works in other GIS software
3. Test with a simple, small shapefile first

## Example Shapefiles

For testing, you can use publicly available shapefiles:
- [Natural Earth Data](https://www.naturalearthdata.com/)
- [OpenStreetMap Extracts](https://download.geofabrik.de/)
- Local government open data portals

Remember to respect data licenses and usage terms when using third-party shapefiles.