# Layerfuse

The layerfuse function was developed to merge data between overlapping geopandas.GeoDataFrame objects. Unlike the typical merge (or JOIN in the SQL lingo) function that matches data between data tables based on a common shared key value, the function in this module uses overlap between polygons of the GeoDataFrame objects to match attributes. If the attributes of a GeoDataFrame A are being merged into a GeoDataFrame B (the SQL analogy would be B LEFT JOIN A), the attribute value of each polygon in B will be a function of the attribute values of the overlapping polygons in GeoDataFrame A. The layerfuse function can handle two types of attributes – size attributes and density attributes – each of which is handled differently.

## Layerfuse Size Attributes

The attributes that may be considered to be proportional to the area of the polygon are referred to as size attributes. These are attributes that are typically the counts of unit features within the polygon. If we assume that the unit features are evenly distributed within the polygon, the attribute counting the feature would be proportional to the size of the polygon. Examples of such attributes in the context of geo-spatial analysis are population, number of trees, area of water bodies etc. The area proportionality assumption implies that if the original polygon has an area of $a$ and attribute value $x$, then a piece of the original polygon with area $fa$ (where $f$ lies between $0$ and $1$) will have an attribute value of $fx$. Then, the merged attribute for a polygon $p$ can be computed from the attributes of the set of overlapping polygons in $B$ using the following formula.
$$
x_p = \sum_{q \in B}\frac{Area(Overlap(p,q))}{Area(q)}\times x_q
$$
where $x_p$ is the value of a size attribute for polygon $p$, $Overlap(p,1)$ is a function that generates the polygon formed by the intersection of polygons $p$ and $q$ , and $Area(p)$ is a function that computes the area of polygon $p$. Note that when a set of attributes is merged from a GeoDataFrame B into a GeoDataFrame A, the above operation will be used to calculate the attributes for each polygon in the GeoDataFrame A.

## Layerfuse Density Attributes

The attributes that measure the density of features in a polygon are referred to as density attributes. Examples of such attribute include, population density, length of road in unit area, percentage of built-up area etc. It is assumed that any piece of the original polygon will have an attribute value that is the same as that of the whole polygon. In other words, it is assumed that the features whose density is measured by the attribute is spread evenly within the polygon. Then, the merged attribute for a polygon $p$ can be computed as the average of the attributes of all the overlapping polygons, weighted based on the area of overlap.
$$
y_p = \sum_{q \in B}\frac{Area(Overlap(p,q))}{Area(p)}\times y_q
$$
where $y_p$ is a density attribute for polygon $p$.

## Example

Consider the case of the two GeoDataFrame A and B shown below. A has 5 polygons and B has 3.

![Image of GeoDataFrames A and B](.\Res\Polygons.png)

GeoDataFrame A has a size attribute $x$ and a density attribute $y$. These attributes have to be merged into the GeoDataFrame B. The areas of the polygons in A, areas of polygons in B and the area of overlap between polygons of A and polygons of B are known. The attribute values of the two GeoDataFrames A are given below.

```markdown
| Polygon | Area | x  | y |
|---------|------|----|---|
| A1      | 14   | 70 | 5 |
| A2      | 20   | 80 | 4 |
| A3      | 15   | 30 | 2 |
| A4      | 25   | 75 | 3 |
| A5      | 10   | 60 | 6 |
```

The areas of the polygons in GeoDataFrame B and their overlap with the polygons in GeoDataFrame A are
