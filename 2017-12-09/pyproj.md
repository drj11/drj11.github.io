[brutal]: #title "Using Python pyproj"
[brutal]: #author "David Jones"
[brutal]: #date "2017-12-09"

So I have a CSV with a bunch of coordinates on the
[British National Grid](https://www.ordnancesurvey.co.uk/resources/maps-and-geographic-resources/the-national-grid.html)
and I want to display these shapes on a map on the web.
Almost all web mapping tools expect coordinates in WGS84.

So I needed to convert the coordinates.
I [cheekily asked](https://twitter.com/darkgreener/status/939116556516315138) data scientist
[Anna Powell-Smith](https://anna.ps/) who suggested [GeoPandas](http://geopandas.org/).

So, in a fresh `conda` environment:

    conda install geopandas

# Wait

I would love to say that in the time it took to download the
2.3 Gigabytes that GeoPandas needs, I found
[pyproj](https://jswhit.github.io/pyproj/) and wrote my
conversion tool using it.

Life isn't quite so poetic,
but it is the case that
a conda env containing _GeoPandas_ is 2.3 Gigabytes, and
a conda env containing _pyproj_ is 156 Megabytes.

(Incidentally, I came across pyproj because I saw it whizz by as
all the dependencies for GeoPandas were scrolling by,
and it sounded like the thing that _actually converted
coordinates_)

The pyproj documentation is not exactly introductory,
but [the docstrings in the source](https://jswhit.github.io/pyproj/pyproj-pysrc.html#transform)
were pretty useful in working out how to use it.

# Project and transform

I create an object for the source projection,
which is British National Grid:

    os_proj = pyproj.Proj(init='epsg:27700')

and an object for the target projection, which is WGS 84:

    wgs_proj = pyproj.Proj(init='epsg:4326')

The numbers after the `epsg` are seemingly random,
but you can discover them by Googling `epsg British National Grid`
or similar.

So with a pair of `x`, `y` coordinates in hand
(which are Eastings and Northings in metres for
British National Grid), you can transform them from one
projection to another with:

    lon, lat = pyproj.transform(os_proj, wgs_proj, x, y)

which (because our target project is WGS84) gives a longitude
and a latitude (in degrees).

You can mass convert by passing in arrays of coordinates apparently,
but I convert a CSV file row-by-row calling `pyproj.transform`
once per row.
That converts a 15000 row file essentially instantaneously
(< 1 second), so that's fine.

The files and the code are in the
[sheffield-lsoa](https://github.com/SheffieldDFG/sheffield-lsoa)
repo.

So, despite not using GeoPandas in the end,
I do thank Anna Powell-Smith for the recommendation.
