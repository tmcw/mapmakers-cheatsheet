# What kind of data do you have?

## Points

- How much data?
  - Just enough
    - Convert the data to [GeoJSON](http://geojson.org/) & make a simple [Leaflet](http://leafletjs.com/) map
  - Lots of points, and each point has data that you want to be able to explore. For instance, apartment listings which might number 10 per city block, but you want to be able to click on them and see photos and links.
    - Cluster your points with [Leaflet.markercluster](https://github.com/Leaflet/Leaflet.markercluster)
  - Too much and the points have some value that can be aggregated
    - Create hexbins of your points with the [QGIS hexbin](https://www.mapbox.com/blog/binning-alternative-point-maps/) plugin, to make
      polygons. Start again at Polygons
  - Too much and the points just represent presence - like tweets
    - Create a heatmap with [Leaflet.heat](https://github.com/Leaflet/Leaflet.heat) or [QGIS heatmap](http://qgis.spatialthoughts.com/2012/07/tutorial-making-heatmaps-using-qgis-and.html) plugin. If you
      use QGIS heatmap, start again at Raster.
  - Tons of data, and you don't need labels? Use [tippecanoe](https://github.com/mapbox/tippecanoe).

## Polygons

- How much data?
  - Just enough
    - Convert the data to [GeoJSON](http://geojson.org/) & make a simple Leaflet map
  - Too much, the polygons have necessary detail
    - Use [Mapbox Studio](https://www.mapbox.com/mapbox-studio/).
    - Use [GeoServer](http://geoserver.org/) with WMS layers and GetFeatureInfo
  - Too much, the polygons have unnecessary details or many of the polygons have shared borders, like state or province maps
    - Simplify them with [TopoJSON](https://github.com/mbostock/topojson) or [QGIS](http://www.qgis.org/)

## Attributes

- What kind of attributes?
  - Absolute numbers
    - Convert the points to centroids with [QGIS](http://www.qgis.org/) and start from Points
    - Normalize absolutes to rates by dividing over polygon area,
      and start from Rates
  - Rates or Categories
    - Make a choropleth map with [Leaflet](http://leafletjs.com/) for small data, [Mapbox Studio](https://mapbox.com/mapbox-studio/)
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
    - geocoding libraries
      - node - [node geocoder](http://nchaulet.github.io/node-geocoder/)
      - Perl - [Geo::Coder::Many](https://metacpan.org/pod/Geo::Coder::Many)
      - PHP - [Geocoder PHP](http://geocoder-php.org)
      - Python - [geopy](https://github.com/geopy/geopy)
      - Ruby - [Ruby Geocoder](http://www.rubygeocoder.com)

## Lines

- Small amounts of data: use [Leaflet](http://leafletjs.com/)
- Lots of data, or need line labels (are they streets?)? Use [Mapbox Studio](https://www.mapbox.com/mapbox-studio/)
- Tons of data, and you don't need line labels? Use [datamaps](https://github.com/ericfischer/datamaps).

# Raster

- Already georectified & cleaned (from satellites or fixed-up sources)
  - If you want to host it yourself
    - Render tiles with [MapTiler](http://www.maptiler.com/), publish them on S3 or some other service, view them in Leaflet
  - If you want someone else to host & process
    - Upload to [Mapbox](https://mapbox.com/) and view in Mapbox GL JS or any client
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

## OpenStreetMap

- I want raw data right from the source, up to the minute, in its original form? [planet.osm](http://planet.openstreetmap.org/)
  - Drawbacks: downloads are very large and require specialized tools to process
- I want raw data for subsets of the world: [Geofabrik extracts](http://www.geofabrik.de/data/download.html) or [Mapzen metro extracts](https://mapzen.com/data/metro-extracts/)
  - Drawbacks: only includes predefined areas, not as up-to-date as Planet.osm
- I want data useful for **fast basemaps**, already processed into vector tiles: [Mapbox](https://mapbox.com/)
  - Drawbacks: doesn't include all features or all tags on features, only those appropriate for visualization
- I want raw data as tiles, which include more data and complete tags: [OSM QA Tiles](http://osmlab.github.io/osm-qa-tiles/)
  - Drawbacks: much larger & slower than tiles designed for visualization
- I want a specific subset of data by area, filter, and want the newest data possible: [Overpass](http://wiki.openstreetmap.org/wiki/Overpass_API)
  - Drawbacks: can't return country-sized chunks of data, only smaller subsets
- I want filtered, up-to-date extracts in extra formats like KMZ, Garmin Image, etc: [HOT Export Tool](http://export.hotosm.org/en/)
  - Drawbacks: can't do arbitrary regions
 

## I don't have data yet

- Government Data
  - Contact the town or federal GIS dept you need
  - Use [FOIAMachine.org](https://www.foiamachine.org/) to request data via FOIA
- Personal Data
  - If you want to create data, use [geojson.io](http://geojson.io/) and draw it.
- Global Data
  - For basic data like countries, cities, use [naturalearthdata.com](http://www.naturalearthdata.com/)
- Historical Data
  - [NYPL MapWarper](http://maps.nypl.org/) for historical, scanned (raster) maps
  - Greek & Roman: [Pleiades](http://pleiades.stoa.org/home)

## Visualization defaults

- Projection:
  - If it's a web map with tiles, use [Spherical Mercator](http://epsg.io/3857). This is the default for Leaflet, Mapbox GL JS, and most other clients.
  - If using [d3](http://d3js.org/) and not using tiles anywhere, use whatever fits best. Bonus projections are in [d3-geo-projection](https://github.com/d3/d3-geo-projection).
    - If it's a map of America, use the [Albers projection](https://en.wikipedia.org/wiki/Albers_projection)
    - If it's a map of a pole, use an [Azimuthal equidistant projection](https://en.wikipedia.org/wiki/Azimuthal_equidistant_projection)
  - Have a projection and not sure what it is? Use [epsg.io](http://epsg.io/3857).
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
    - There are few cases where north shouldn't be up: for instance, [Montreal, Canada is often mapped at an angle](https://www.google.com/search?tbs=imgo%3A1&tbm=isch&sa=1&btnG=Search&q=montreal+map).
  - Always attribute your data, especially [OpenStreetMap](https://www.openstreetmap.org/), to avoid the nerd wrath
  - If it zooms, add visible zoom controls. Pan isn't necessary, but not everyone has a scroll wheel / multitouch

---

### See also

* [mapmakers-cheatsheet in Japanese](https://github.com/sw897/mapmakers-cheatsheet)
