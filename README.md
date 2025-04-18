 # Overture Data to pgRouting Topology Creation

 This `PL/pgSQL` script will convert the Overture segment data set into a routing topology table suitable for pgRouting and pgrServer usage.

This is actually based on the [blog](https://www.crunchydata.com/blog/vehicle-routing-with-postgis-and-overture-data) written by Paul Ramsey, and uses the [overture.sql](https://github.com/CrunchyData/crunchy-bridge-for-analytics-examples/blob/main/overture/overture.sql) source code mentioned in the blog. 

This source though is a modified version to use JSON based Overture table usually created by GDAL instead of a `FDW` for `DuckDB` that was used in the original source. 

To convert Overture segment data into a routing topology, the following has to be done: 

* install the python based tool to download Overture maps. Information on this tool is found [here](https://docs.overturemaps.org/getting-data/overturemaps-py/).

* Select the bounding box of the desired area to be downloaded. This [Bounding Box tool](https://boundingbox.klokantech.com/) can be used to get the csv based bounding box.  

* Download Overture segment data using the bounding box and save the data in `GeoJSON` data format. 

```
overturemaps download --bbox=138.39,34.88,140.97,36.7 -f geojson --type=segment -o tokyo.geojson
```

* Import the GeoJson file containing the Overture segment data into PostgreSQL using GDAL's `ogr2ogr` application. 

```
ogr2ogr -f "PostgreSQL" PG:"dbname=pgr user=mbasa" "tokyo.geojson" -nln overture -overwrite
```
* ***NOTE:*** the dbname and user parameters should be modified to suit the local postgreSQL values.

* Download and install `overture_to_pgr.sql` into the PostgreSQL database that contains the Overture data. 

```
psql -f overture_to_pgr.sql <dbname>
```

* Finally, convert the Overture data into a routing topology table

```
select overture_to_pgr();
```

