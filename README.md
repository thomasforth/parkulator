# Parkulator
See how much space in your city is taken up by parking.

You can explore this data for GB (and soon the world) at [imactivate.com/parkulator](http://www.imactivate.com/parkulator) now.

## FAQs
Why isn't my place included?
Sorry, it will be soon. I'm still learning and I need a few more weeks.

Why is a car park missing or wrong?
Because it's missing or wrong on Open Street Map. If you go and fix that on Open Street Map it'll eventually be fixed on my tool.

## Method
This tool uses [osmconvert](https://wiki.openstreetmap.org/wiki/Osmconvert#Exclude_Information_or_Contents_from_the_Output_File), [osmfilter](https://wiki.openstreetmap.org/wiki/Osmfilter#Keep_only_specific_Tags), and [raw open street map downloads from Geofabrik](http://download.geofabrik.de/). As such all data is Copyright of The Open Street Contributors.

I do six things,
1. Download an osm.pbf file from Geofabrik (I picked Great Britain but this method will work for anywhere). Extraction date was 2019-05-10. The process will need re-running to include updates to OSM.
2. Convert it from pbf to o5m format using `osmconvert great-britain-latest.osm.pbf -o=great-britain-latest.osm.o5m`.
3. Use osmfilter to extract "amenity=parking" tags only using `osmfilter great-britain-latest.osm.o5m --keep="amenity=parking" -o=great-britain-parking.osm`.
4. Load the osm file into [QGIS](https://www.qgis.org/en/site/) and resave as GEOJSON.
5. Download The OECD functional economic area [shapefile for the UK](https://www.oecd.org/cfe/regional-policy/functionalurbanareasbycountry.htm) and select my eight cities of interest.
6. Use the `clip` function in QGIS to retain only the parking in those cities. A full file of all parking Great Britain is also provided. Both are exported in GEOJSON format and available for download in this repository.

## Next step (expanding to the whole of the UK, Europe, the world etc...)
1. Validate the parking file in QGIS (`Vector>Geometry Tools>Check Validity...`).
2. Save the valid portion in SpatialLite format (including a spatial index) from within QGIS.
3. Use ogr2ogr.exe running on a webserver to serve in geojson only the features within the current viewbox in leaflet. Examples commands are `ogr2ogr.exe -f "geojson" -spat -2 52 0 53 "/vsistdout/" GBParking_valid_indexed.sqlite` (output geojson to the console) and `ogr2ogr.exe -spat -2 52 0 53 clipped.geojson GBParking_valid_indexed.sqlite` (output geojson to a file).

## License
All mapping data is Copyright of The Open Street Contributors.
