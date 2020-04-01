---
layout: post
title: "[Notes] CSCI 585 Spatial DBs
"
tags: [csci585, eng, note]
excerpt_separator: ---
---

*Credit to: Prof. Saty Raghavachary, CSCI 585, Spring 2020*

**Outline**
- spatial DBs: definition, characteristics, need, creation.
- spatial datatypes
- spatial operators
- spatial indices
- implementations
- miscellany

---
## Spatial Database (SD)

### What is a spatial database?
"A spatial database is a database that is optimized to store and query data related to objects in space, including points, lines and polygons."

In other words, it includes objects that have a SPATIAL location (and extent). A chief category of spatial data is geospatial data - derived from the geography of our earth.

Characteristics of geographic data:

- has location
- has size
- is auto-correlated
- scale dependent
- might be temporally dependent too

Geographic data is NOT 'business as usual'!

### Entity view vs field view
In spatial data analysis, we distinguish between two conceptions of space:

- entity view: space as an area filled with a set of discrete objects
- field view: space as an area covered with essentially continuous surfaces

For our purposes, we will adopt the 'entity' view, where space is populated by discrete objects (roads, buildings, rivers..).

### Components
![Components](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/Components.jpg)

So a spatial DB is a collection of the following, specifically built to handle spatial data:

- types
- operators
- indices

### What can be plotted on to a map?
- crime data
- spread of disease, risk of disease [look at this too]
- drug overdoses - over time
- census data
- income distribution, home prices
- locations of Starbucks (!)
- (real-time) traffic
- agricultural land use, deforestation

### Who creates/uses spatial data?

![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/SDBMS_UseCases.jpg)

Various government agencies routinely coordinate spatial data collection and use, operating in effect, a national spatial data infrastructure (NSDI) - these include federal, state and local agencies. At the federal level, participating agencies include:

- Department of Commerce
  - Bureau of the Census
  - NIST
  - NOAA
- Department of Defense
  - Army Corps of Engineers
  - Defense Mapping Agency
- Department of the Interior
  - Bureau of Land Management
  - Fish and Wildlife Service
  - U.S Geological Survey
- Department of Agriculture
  - Agricultural Stabilization and Conservation Service
  - Economic Research Service
  - Forest Service
  - National Agriculture Statistical Service
  - Soil Conservation Service
- Department of Transportation
  - Federal Highway Administration
- Environmental Protection Agency
- NASA
As you can see, spatial data is a SERIOUS resource, vital to national interests.

### Where does spatial data come from?
Spatial data is created in a variety of ways:

- CAD: user creation
- CAD: reverse engineering
- maps: cartography (surveying, plotting)
- maps: satellite imagery
- maps: 'copter, drone imagery
- maps: driving around
- maps: walking around

### What to store?
All spatial data can be described via the following entities/types:

- points/vertices/nodes
- polylines/arcs/linestrings
- polygons/regions
- pixels/raster

### Points, lines, polys => models and non-spatial attrs
Once we have spatial data (points, lines, polygons), we can:

- 'model' features such as lakes, soil type, highways, buildings etc, using the geometric primitives as underlying types
- add 'extra', non-spatial attributes/features to the underlying spatial data

![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/LayeredData1.jpg)

![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/LayeredData2.jpg)


## SDBMS architecture

![SDBMS architecture](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/SDBMS_3LayerStructure.jpg)

![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/SDBMS_3LayerArch2.jpg)

## GIS vs SDBMS
GIS is a specific application architecture built on top of a [more general purpose] SDBMS.

GIS typically tend to be used for:
![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/GISAnalysisExamples.jpg)

## Spatial relationships
In 1D (and higher), spatial relationships can be expressed using 'intersects', 'crosses', 'within', 'touches' (these are T/F predicates).

Here is a sampling of spatial relationships in 2D:
![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/SpatialRelations.jpg)
![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/BinTopo.png)

Minimum Bounding Rectangles (MBRs) are what are used to compute the results of operations shown above:

![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/MBR.jpg)

### Spatial relations - categories
Spatial relationships can be:

- topology-based [using defns of boundary, interior, exterior]
- metric-based [distance/Euclidian, angle measures]
- direction-based
- network-based [eg. shortest path]

Topological relationships could be further grouped like so:
- proximity
- overlap
- containment

### How can we put these relations to use?
We can perform the following, on spatial data:
- spatial measurements: find the distance between points, find polygon area..
- spatial functions: find nearest neighbors..
- spatial predicates: test for proximity, containment..

![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/EntityCreation.jpg)

![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/EntityCreation2.jpg)

![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/QueryEx1.jpg)

### Spatial operators, functions
![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/SpatialFns.jpg)

![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/SpatialOps.jpg)

[more](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/doc/SpatialRelations.pdf)

## Oracle Spatial
Oracle offers a 'Spatial' library for spatial queries - this includes UDTs and custom functions to process them.
![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/Or/SpatialDatabaseWhatIs.jpg)

![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/Or/OracleSpatial10g.jpg)

![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/Or/OracleSpatialTypes.jpg)

![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/Or/SDO_Geom.jpg)

![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/Or/SDO_GType.jpg)

![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/Or/SDO_GType2.jpg)

![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/Or/OracleConstructingGeoms.jpg)

![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/Or/SpatialOps2.jpg)

![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/Or/SDO_Filter.jpg)

![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/Or/SDO_Relate.jpg)

![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/Or/RelOps.jpg)

![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/Or/ProxOps.jpg)

![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/Or/ContOps.jpg)

## Spatial Indexing
- Used ti optimize spatial query performance
- R-tree Indexing
  - Based on minimum bounding rectangles ((MBRs) for 2D data or minimum bounding volume(MBVs) for 3D data
  - Indexes 2, 3, or 4 dimensions
- Provides an exclusive and exhaustive coverage of spatial objects
- Indexes all elements withing geometry including points, lines, and polygons

![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/Or/OracleOptimizedQueryModel.jpg)

### Postgres PostGIS
The function names for queries differ across geodatabases.The following list contains commonly used functions built into PostGIS, a free geodatabase which is a Postgre SOL wztension (the term 'geometry' refers to a point, line, box or other two or three dimensional shape):
1. Distance (geometry, geometry): number
2. Equals (geometry, geometry): boolean
3. Disjoint (geometry, geometry): boolean
4. Intersects (geometry, geometry): boolean
5. Touches (geometry, geometry): boolean 
6. Crosses (geometry, geometry): boolean 
7. Overlap (geometry, geometry): boolean 
8. Contains (geometry, geometry): boolean
9. Intersects (geometry): boolean 
10. Length (geometry): number
11. Area (geometry): number 
12. Centroid ((geometry) : geometry

### Creating spatial indexes
As (more so than) with non-spatial data, the creation and use of spatial indexes VASTLY speed up processing!

### Can B Trees index spatial data?
In short, YES, if we pair it up with a 'z curve' indexing scheme (using a space-filling curve):

![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/BTrees.jpg)

The idea is to quantize every (x,y) location into a recursively-divided 'quadtree' cell, and use the cell's binary (x,y) location to create a (binary) 'z' key, which is ordered along the unit (0..1) interval - in other words, 2D (x,y) points get mapped (indexed) to ordered 1D 'z' locations.

But, this is of academic interest mostly, not commonly practiced in industry - Apple's FoundationDB is an exception.

### R trees
![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/RTree.png)

R trees use MBRs to create a hierarchy of bounds.

Variations, FYI: R+ tree, R* tree, Buddy trees, Packed R trees..

### k-d trees

![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/kdTree.jpg)

### K-D-B trees
![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/kdbTree.jpg)

### Quadtrees (and octrees):

![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/quadTree.jpg)

Each node is either a leaf node, with indexed points or null, or an internal (non-leaf) node that has exactly 4 children. The hierarchy of such nodes forms the quadtree.

### indexing evolution 
![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/IndexingEvol.jpg)

## Query processing
![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/QueryProcEx.jpg)

## Visualizing spatial data
A variety of non-spatial attrs can be mapped on to spatial data, providing an intuitive grasp of patterns, trends and abnormalities. Following are some examples.

Dot map:

![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/Viz_DotMap.jpg)

Proportional symbol map:

![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/Viz_PropSymbolMap.jpg)

Diagram map:

![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/Viz_DiagramMap.jpg)

Another diagram map:

![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/Viz_DiagramMap2.jpg)

Also possible to plot multivariate data this way.

**Choropleth** maps (plotting of a variable of interest, to cover an entire region of a map):

![](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/Spatial/pics/Viz_ChoroplethMap.jpg)


### So who (else) has spatial extensions?
Everyone!

Thanks to SQL's facility for custom datatype ('UDT') and function creation ('functional extension'), "spatial" has been implemented for every major DB out there:

Oracle: Locator, Spatial, SDO
Postgres: PostGIS
DB2: Spatial Datablade
Informix: Geodetic Datablade
SQL Server: Geometric and Geodetic Geography types
MySQL: spatial library comes 'built in'
SQLite: SpatiaLite
..
### Google KML
Google's KML format is used to encode spatial data for Google Earth, etc. Here is a page on importing other geospatial dataset formats into Google Earth.

### OpenLayers
OpenLayers is an open GIS platform.
### ESRI: Arc*
ESRI is the home of the powerful, flexible family of ArcGIS products - and they are local!
### QGIS etc.
There is a variety of inexpensive/open source mapping platforms, competing with more pricey commercial offerings (from ESRI etc). Here are several:
- QGIS
- MapBox
- Carto
- Boundless
- GIS Cloud