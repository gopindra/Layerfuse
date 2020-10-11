# Layerfuse

The `layerfuse` function was developed to merge data between overlapping `geopandas.GeoDataFrame` objects. Unlike the typical merge (or `JOIN` in the SQL lingo) function that matches data between data tables based on a common shared key value, the function in this module uses overlap between polygons of the GeoDataFrame objects to match attributes. If the attributes of a GeoDataFrame `A` are being merged into a GeoDataFrame `B` (the SQL analogy would be `B LEFT JOIN A`), the attribute value of each polygon in `B` will be a function of the attribute values of the overlapping polygons in GeoDataFrame `A`. The layerfuse function can handle two types of attributes – size attributes and density attributes – each of which is handled differently.

Read the Wiki pages for more information: </br>
[Methodology](https://github.com/skynet93/Layerfuse/wiki/Layerfuse-Methodology) </br>
[Example](https://github.com/skynet93/Layerfuse/wiki/Layerfuse-Example)

# Version
At the time of development, Python version 3.7.6 was used with Pandas version 1.0.4 and Geopandas version 0.7.0.
