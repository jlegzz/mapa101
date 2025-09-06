# Shapefile Data Guide

This guide explains how to prepare and use shapefile data with the GIS Parcel Viewer application.

## Shapefile Components

A complete shapefile consists of multiple files with the same base name but different extensions:

### Required Files

- **`.shp`** - Main file containing geometry data (points, lines, polygons)
- **`.shx`** - Shape index file for quick access to geometry
- **`.dbf`** - Database file containing attribute data

### Optional but Recommended Files

- **`.prj`** - Projection file defining coordinate system
- **`.cpg`** - Code page file for character encoding
- **`.sbn/.sbx`** - Spatial index files (for performance)

## Supported Geometry Types

The application works best with:

1. **Polygons** - For parcel boundaries (recommended)
2. **Points** - For parcel centroids or markers
3. **Lines** - For property boundaries or utilities

## Attribute Data Structure

### Common Parcel Attributes

Your `.dbf` file should contain relevant parcel information:

```
Field Name    | Type    | Description
------------- | ------- | ---------------------
PARCEL_ID     | Text    | Unique parcel identifier
OWNER_NAME    | Text    | Property owner name
ADDRESS       | Text    | Property address
AREA_SQFT     | Number  | Area in square feet
AREA_ACRES    | Number  | Area in acres
ZONE_TYPE     | Text    | Zoning classification
LAND_VALUE    | Number  | Assessed land value
TOTAL_VALUE   | Number  | Total assessed value
YEAR_BUILT    | Number  | Year structure built
USE_CODE      | Text    | Property use code
TAX_DISTRICT  | Text    | Tax district
```

### Field Naming Best Practices

- Use descriptive field names (max 10 characters for .dbf compatibility)
- Avoid special characters and spaces
- Use underscores instead of spaces
- Keep names consistent across datasets

## Data Preparation Tips

### 1. Coordinate System

- **Recommended**: WGS84 (EPSG:4326) for web compatibility
- **Alternative**: Web Mercator (EPSG:3857)
- Include a `.prj` file to define the coordinate system

### 2. Geometry Simplification

For large datasets:
- Simplify complex polygons to reduce file size
- Remove unnecessary vertices while preserving shape
- Consider using tools like QGIS or ArcGIS for simplification

### 3. File Size Optimization

- **Small datasets** (< 1MB): Upload individual files
- **Large datasets** (> 1MB): Create ZIP archive
- **Very large datasets** (> 50MB): Consider splitting into regions

## Creating Test Data

### Sample Parcel Structure

Here's an example of how your data might look:

```
PARCEL_ID  | OWNER_NAME      | ADDRESS           | AREA_ACRES | ZONE_TYPE
---------- | --------------- | ----------------- | ---------- | ---------
001-123-45 | John Smith      | 123 Main St      | 0.25       | R1
001-123-46 | Jane Doe        | 125 Main St      | 0.30       | R1
001-124-01 | ABC Corp        | 200 Commerce Ave | 2.50       | C1
001-124-02 | City of Example | 150 Park Rd      | 5.00       | P1
```

## Common Data Sources

### Public Data Sources

1. **County Assessor Offices**
   - Often provide parcel data as shapefiles
   - May require registration or fee

2. **State GIS Portals**
   - Many states offer free parcel data
   - Usually updated annually

3. **Municipal Open Data**
   - City/town websites often have GIS data
   - Check local government websites

4. **Federal Sources**
   - Census Bureau (TIGER files)
   - USGS (National Map)

### Commercial Sources

- Esri Data & Maps
- CoreLogic
- First American
- Local GIS vendors

## Data Quality Checklist

Before uploading your shapefile:

- [ ] All required files present (.shp, .shx, .dbf)
- [ ] Files have matching base names
- [ ] Coordinate system defined (.prj file)
- [ ] Attribute data is complete and accurate
- [ ] No missing or null geometries
- [ ] Field names follow naming conventions
- [ ] File size is reasonable for web use

## Troubleshooting Data Issues

### Common Problems

1. **"No .shp file found"**
   - Ensure .shp file is included
   - Check file extension spelling

2. **"Invalid geometry"**
   - Repair geometry in GIS software
   - Check for self-intersecting polygons

3. **"No attributes displayed"**
   - Verify .dbf file is included
   - Check that .dbf contains data

4. **"Projection issues"**
   - Include .prj file
   - Verify coordinate system is correct

### Data Validation Tools

- **QGIS**: Free, open-source GIS software
- **ArcGIS**: Commercial GIS platform
- **FME**: Data transformation tool
- **GDAL/OGR**: Command-line utilities

## Example Workflows

### Converting from Other Formats

#### From KML/KMZ:
```bash
ogr2ogr -f "ESRI Shapefile" parcels.shp parcels.kml
```

#### From GeoJSON:
```bash
ogr2ogr -f "ESRI Shapefile" parcels.shp parcels.geojson
```

#### From CAD (DWG/DXF):
1. Open in QGIS or ArcGIS
2. Select polygon layers
3. Export as shapefile

### Creating ZIP Archive

1. Select all shapefile components:
   - parcels.shp
   - parcels.shx
   - parcels.dbf
   - parcels.prj (if available)

2. Create ZIP file:
   - Right-click → "Send to" → "Compressed folder" (Windows)
   - Use Archive Utility (Mac)
   - Use `zip` command (Linux)

3. Upload the ZIP file to the application

## Performance Considerations

### File Size Guidelines

- **Optimal**: < 5MB (fast loading)
- **Good**: 5-20MB (moderate loading time)
- **Large**: 20-50MB (slower loading)
- **Very Large**: > 50MB (consider splitting)

### Optimization Strategies

1. **Geometry Simplification**
   - Reduce vertex count
   - Maintain visual accuracy

2. **Attribute Reduction**
   - Remove unnecessary fields
   - Shorten text values where possible

3. **Spatial Filtering**
   - Extract only needed geographic areas
   - Split large datasets by region

## Advanced Tips

### Multi-Layer Datasets

If you have multiple related layers:
1. Upload one primary layer (parcels)
2. Use separate instances for additional layers
3. Consider combining layers in GIS software first

### Custom Styling

Prepare attribute data for custom styling:
- Add classification fields (e.g., VALUE_CLASS)
- Use consistent coding schemes
- Document field meanings for users

### Metadata Documentation

Include information about:
- Data source and date
- Coordinate system
- Field definitions
- Known limitations
- Contact information

This helps users understand and properly use your data.