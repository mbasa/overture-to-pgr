 # Overture Data to pgRouting Topology Creation

 This `PL/pgSQL` script will convert the Overture segment data set into a routing topology table suitable for pgRouting and pgrServer usage.

This is actually based on the [blog](https://www.crunchydata.com/blog/vehicle-routing-with-postgis-and-overture-data) written by Paul Ramsey, and uses the [overture.sql](https://github.com/CrunchyData/crunchy-bridge-for-analytics-examples/blob/main/overture/overture.sql) source code. 

This source though is a modified version to use JSON based Overture table instead of a `FDW` for `DuckDB`. 

