# Layerfuse

The `layerfuse` function was developed to merge data between overlapping `geopandas.GeoDataFrame` objects. Unlike the typical merge (or `JOIN` in the SQL lingo) function that matches data between data tables based on a common shared key value, the function in this module uses overlap between polygons of the GeoDataFrame objects to match attributes. If the attributes of a GeoDataFrame `A` are being merged into a GeoDataFrame `B` (the SQL analogy would be `B LEFT JOIN A`), the attribute value of each polygon in `B` will be a function of the attribute values of the overlapping polygons in GeoDataFrame `A`. The layerfuse function can handle two types of attributes – size attributes and density attributes – each of which is handled differently.

## Layerfuse Size Attributes

The attributes that may be considered to be proportional to the area of the polygon are referred to as size attributes. These are attributes that are typically the counts of unit features within the polygon. If we assume that the unit features are evenly distributed within the polygon, the attribute counting the feature would be proportional to the size of the polygon. Examples of such attributes in the context of geo-spatial analysis are population, number of trees, area of water bodies etc. The area proportionality assumption implies that if the original polygon has an area of `a` and attribute value `x`, then a piece of the original polygon with area `f*a` (where `f` lies between 0 and 1) will have an attribute value of `f*x`. 

## Layerfuse Density Attributes

The attributes that measure the density of features in a polygon are referred to as density attributes. Examples of such attribute include, population density, length of road in unit area, percentage of built-up area etc. It is assumed that any piece of the original polygon will have an attribute value that is the same as that of the whole polygon. In other words, it is assumed that the features whose density is measured by the attribute is spread evenly within the polygon.

## Example

Consider the case of the two GeoDataFrame `A` and `B` shown below. `A` has 5 polygons and `B` has 3.

![Image of GeoDataFrames A and B](.\Res\Polygons.png)

GeoDataFrame `A` has a size attribute `x` and a density attribute `y`. These attributes have to be merged into the GeoDataFrame `B`. The areas of the polygons in `A`, areas of polygons in `B` and the area of overlap between polygons of `A` and polygons of `B` are known. The attribute values of the two GeoDataFrames `A` are given below.

| Polygon | Area | x  | y |
|---------|------|----|---|
| A1      | 14   | 70 | 5 |
| A2      | 20   | 80 | 4 |
| A3      | 15   | 30 | 2 |
| A4      | 25   | 75 | 3 |
| A5      | 10   | 60 | 6 |

The areas of the polygons in GeoDataFrame `B` and their overlap with the polygons in GeoDataFrame `A` are as follows.

| Polygon | Area  |  Overlap A1 | Overlap A2 | Overlap A3 | Overlap A4 | Overlap A5 |
|---------|-------|----|----|----|----|----|
| B1      | 20    | 5  | 9  | 6  | 0  | 0  |
| B1      | 15    | 0  | 6  | 4  | 4  | 1  |
| B3      | 25    | 0  | 0  | 0  | 16 | 4  |

Note that some part of Polygon `B3` lies outside of all the polygons in `A`.

