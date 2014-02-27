- What kind of data do you have?
  - Points
    - How much data?
      - Just enough
        - Convert the data to GeoJSON & make a simple Leaflet map
      - Too much in a confusing way, but each point's data is important?
        - Cluster your points with Leaflet.markercluster
      - Too much and the points have some value that can be aggregated
        - Create hexbins of your points with the QGIS hexbin plugin, to make
          polygons. Start again at Polygons
      - Too much and the points just represent presence - like tweets
        - Create a heatmap with Leaflet.heat or QGIS heatmap plugin. If you
          use QGIS heatmap, start again at Raster.
  - Polygons
    - How much data?
      - Just enough
        - Convert the data to GeoJSON & make a simple Leaflet map
      - Too much, the polygons have necessary detail
        - Use TileMill to render an interactive map with UTFGrid
      - Too much, the polygons have unnecessary details
        - Simplify them with TopoJSON or QGIS
    - What kind of attributes do they have?
      - Absolute numbers
        - Convert the points to centroids with QGIS and start from Points
        - Normalize absolutes to rates by dividing over polygon area,
          and start from Rates
      - Rates or Categories
        - Make a choropleth map with Leaflet for small data, TileMill
          for big data
  - Raster
    - Render a map with TileMill and use the tiles in Leaflet
  - Names of places, like countries
    - With IDs, like ISO3 codes
      - Download Natural Earth data at the right level, join with QGIS,
        and start again at Polygons
    - Without IDs
      - Find data with IDs, or manually join with polygons
  - Addresses
    - You can't map addresses directly. Geocode them with OpenRefine or
      Geo for Google Docs, and then start at Points
  - A format that I can't read
    - Install GDAL and use ogr2ogr to convert the file
    - Ask your source for a better file format
  - I don't have data yet
    - Contact the town or federal GIS dept you need
    - Use FOIAMachine.org to request data via FOIA

- Visualization defaults
  - Projection:
    - If it's a web map with tiles, use Mercator
    - If using d3 and not using tiles anywhere, use whatever fits best
  - Colors:
    - When in doubt, use ColorBrewer
  - Scales:
    - For any data
      - Try linear first
      - Then quantile
    - For data of rates or compounding
      - Try log and power scales
  - Points:
    - Start with normal circles with no strokes
    - Scale points by area, not diameter
