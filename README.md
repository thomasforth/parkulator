# Parkulator
See how much space in your city is taken up by parking.

For eight [OECD functional urban areas](https://www.oecd.org/cfe/regional-policy/functionalurbanareasbycountry.htm) of the UK (Birmingham, Manchester, Leeds, Bradford, Liverpool, Leicester, Nottingham, and Sheffield) you can explore this data at [imactivate.com/parkulator](http://www.imactivate.com/parkulator) now.

## Method
This tool uses [osmconvert](https://wiki.openstreetmap.org/wiki/Osmconvert#Exclude_Information_or_Contents_from_the_Output_File), [osmfilter](https://wiki.openstreetmap.org/wiki/Osmfilter#Keep_only_specific_Tags), and [raw open street map downloads from Geofabrik](http://download.geofabrik.de/). As such all data is Copyright of The Open Street Contributors.

I do six things,
1. Download an osm.pbf file from Geofabrik (I picked Great Britain but this method will work for anywhere). Extraction date was 2019-05-10. The process will need re-running to include updates to OSM.
2. Convert it from pbf to o5m format using `osmconvert great-britain-latest.osm.pbf -o=great-britain-latest.osm.o5m`.
3. Use osmfilter to extract "amenity=parking" tags only using `osmfilter great-britain-latest.osm.o5m --keep="amenity=parking" -o=great-britain-parking.osm`.
4. Load the osm file into [QGIS](https://www.qgis.org/en/site/) and resave as GEOJSON.
5. Download The OECD functional economic area [shapefile for the UK](https://www.oecd.org/cfe/regional-policy/functionalurbanareasbycountry.htm) and select my eight cities of interest.
6. Use the `clip` function in QGIS to retain only the parking in those cities. A full file of all parking Great Britain is also provided. Both are exported in GEOJSON format and available for download in this repository.

## License
All mapping data is Copyright of The Open Street Contributors.