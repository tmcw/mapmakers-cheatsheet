# What kind of data do you have?

## Points

- How much data?
  - Just enough
    - Convert the data to [GeoJSON](http://geojson.org/) & make a simple [Leaflet](http://leafletjs.com/) map
  - Too much in a confusing way, but each points data is important?
    - Cluster your points with [Leaflet.markercluster](https://github.com/Leaflet/Leaflet.markercluster)
  - Too much and the points have some value that can be aggregated
    - Create hexbins of your points with the [QGIS hexbin](https://www.mapbox.com/blog/binning-alternative-point-maps/) plugin, to make
      polygons. Start again at Polygons
  - Too much and the points just represent presence - like tweets
    - Create a heatmap with [Leaflet.heat](https://github.com/Leaflet/Leaflet.heat) or [QGIS heatmap](http://qgis.spatialthoughts.com/2012/07/tutorial-making-heatmaps-using-qgis-and.html) plugin. If you
      use QGIS heatmap, start again at Raster.
  - Tons of data, and you don't need labels? Use [datamaps](https://github.com/ericfischer/datamaps).

## Polygons

- How much data?
  - Just enough
    - Convert the data to [GeoJSON](http://geojson.org/) & make a simple Leaflet map
  - Too much, the polygons have necessary detail
    - Use [TileMill](https://www.mapbox.com/tilemill/) to render an interactive map with [UTFGrid](https://www.mapbox.com/developers/utfgrid/)
    - Use [GeoServer](http://geoserver.org/) with WMS layers and GetFeatureInfo
  - Too much, the polygons have unnecessary details
    - Simplify them with [TopoJSON](https://github.com/mbostock/topojson) or [QGIS](http://www.qgis.org/)

## Attributes

- What kind of attributes?
  - Absolute numbers
    - Convert the points to centroids with [QGIS](http://www.qgis.org/) and start from Points
    - Normalize absolutes to rates by dividing over polygon area,
      and start from Rates
  - Rates or Categories
    - Make a choropleth map with [Leaflet](http://leafletjs.com/) for small data, [TileMill](https://www.mapbox.com/tilemill/)
      for big data
  - Temporal data - values over time
    - If there are fewer than 100 samples - like 50 years of data grouped by year, make [small multiples](http://www.nytimes.com/interactive/2012/07/20/us/drought-footprint.html): a map per sample.
    - If you can code, make an animation with [Leaflet](http://leafletjs.com/) or [d3.js](http://d3js.org/)
    - If it's tons of data, use [CartoDB and torque](http://blog.cartodb.com/post/66687861735/torque-is-live-try-it-on-your-cartodb-maps-today)
  - Multivariate data: like counts of different species or ethnicities
    - Make a [dot density map](http://demographics.coopercenter.org/DotMap/index.html) with [englewood](https://github.com/newsapps/englewood)
  - Names of places, like countries
    - With IDs:
      - ISO2 or ISO3 codes
        - Download [Natural Earth](http://www.naturalearthdata.com/) data at the right level, join with QGIS,
          and start again at Polygons
      - ZIP codes
        - Download [ZCTAs](https://www.census.gov/geo/reference/zctas.html) and join
    - Without IDs
      - Find data with IDs, or manually join with polygons
  - Addresses
    - You can't map addresses directly. Geocode them with [OpenRefine](http://openrefine.org/) or
      [Geo for Google Docs](https://www.mapbox.com/geo-for-google-docs/), and then start at Points
    - Other Geocoding options:
      - US: [US Census](http://geocoding.geo.census.gov/geocoder/Geocoding_Services_API.pdf)
      - Canada: [Geogratis](http://geogratis.gc.ca/site/eng/geoloc)
      - OpenStreetMap: [Nominatim](http://nominatim.openstreetmap.org/)
      - [Data Science Toolkit](https://github.com/petewarden/dstk) can be useful for local bulk geocoding that would be too much for a hosted service.

## Lines

- Small amounts of data: use [Leaflet](http://leafletjs.com/)
- Lots of data, or need line labels (are they streets?)? Use [TileMill](https://www.mapbox.com/tilemill/)
- Tons of data, and you don't need line labels? Use [datamaps](https://github.com/ericfischer/datamaps).

# Raster

- Already georectified & cleaned (from satellites or fixed-up sources)
  - Render a map with [TileMill](https://www.mapbox.com/tilemill/) and use the tiles in Leaflet
  - Read [processing satellite imagery](https://www.mapbox.com/foundations/processing-satellite-imagery/) to understand [GDAL](http://www.gdal.org/)/[ImageMagick](http://www.imagemagick.org/) workflow.
- Raster images from drones
- Raster images from scanned maps
  - Use [MapKnitter](http://mapknitter.org/) to georeference and georectify

## A format that I can't read

- Install [GDAL](http://www.gdal.org/) and use ogr2ogr to convert the file. If you can't install
  this, you can use it online with [Ogre](http://ogre.adc4gis.com/)
- Commercial tools:
  - [SAFE FME](http://www.safe.com/)
- Ask your source for a better file format

## I don't have data yet

- Government Data
  - Contact the town or federal GIS dept you need
  - Use [FOIAMachine.org](https://www.foiamachine.org/) to request data via FOIA
- Personal Data
  - If you want to create data, use [geojson.io](http://geojson.io/) and draw it.
- Global Data
  - For basic data like countries, cities, use [naturalearthdata.com](http://www.naturalearthdata.com/)
  - Use [OpenStreetMap extracts](https://mapzen.com/metro-extracts/) for higher-detail local data
- Historical Data
  - [NYPL MapWarper](http://maps.nypl.org/) for historical, scanned (raster) maps
  - Greek & Roman: [Pleiades](http://pleiades.stoa.org/home)

## Visualization defaults

- Projection:
  - If it's a web map with tiles, use [Spherical Mercator](http://epsg.io/3857)
  - If using [d3](http://d3js.org/) and not using tiles anywhere, use whatever fits best. Bonus projections are in [d3-geo-projection](https://github.com/d3/d3-geo-projection).
  - Have a project and not sure what it is? Use [epsg.io](http://epsg.io/3857).
- Colors:
  - When in doubt, use [ColorBrewer](http://colorbrewer2.org/)
  - Want to know more? Read [Subtleties of Color](http://earthobservatory.nasa.gov/blogs/elegantfigures/2013/08/05/subtleties-of-color-part-1-of-6/)
- Scales:
  - For any data
    - Try linear first
    - Then quantile
  - For data of rates or compounding values
    - Try log and power scales
- Points:
  - Start with normal circles with no strokes
  - Scale points by area, not diameter
- Flair:
  - Only add a north arrow if north isn't up
  - Always attribute your data, especially [OpenStreetMap](http://www.openstreetmap.org/), to avoid the nerd wrath
  - If it zooms, add visible zoom controls. Pan isn't necessary, but not everyone has a scroll wheel / multitouch
