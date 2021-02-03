# Spatial Ecosystem in R
<h3>Part 1 </h3>

*Kiranmayi Vadlamudi*

If your starting out as a beginner in learning to handle spatial data, you might find the open source spatial ecosystem in R vast and bit overwhelming to navigate. Choosing the right tool for your analysis can save a lot of time and make ones life much easier. In this article I will cover the bare minimum needed to get away with some simple yet beautiful mapping and some spatial data wrangling. If you are here for a quick walk through, scroll to the example else if you don't mind reading - have a go!:zap:

The history of R spatial ecosystem dates back to early 2000s with some of earliest versions of the packages being first written in S [1]. While some packages for spatial statistical analysis written then are still available on CRAN, there are several more newer packages on offer now. Academics from different fields such as ecology, transport, computer science, urban planning, quantitative geography etc have always dealt with spatial data but over the last few years, academics from these fields came together to develop the R spatial ecosystem and formalise it.

The increased computational power, access to cloud computing resources and the vast space-time varying data being generated on a daily basis has made spatial data analysis and geocomputation popular & *a need to know* if you want to extract meaningful insights from your spatial data.


1. [sf package (short for Simple Features)](https://r-spatial.github.io/sf/articles/sf1.html)[2] : The release of `sf` in 2016 made working with spatial vector data significantly easier and faster. The `sf` class offers a sticky geometry column for the data which doesn't drop while performing data operations.
Its build on its predecessor `sp` and offers the functionalities of 3 packages - `sp`, `rgeos` and `rgdal` [1] making it the only package you need to deal with most of your vector data handling. The other advantage of `sf` is its integration with `tidyverse` which allows for using of data wrangling functions from the `tidyverse` universe to handle the `sf` spatial data class. `sf` much like `sp` is also becoming integral in development of other spatial packages and integration in R providing better interoperability.

2. [sp package](https://cran.r-project.org/web/packages/sp/sp.pdf): The `sp` is one of the earliest packages from 2000s which was developed to handle spatial data types in R. It standardises spatial objects in R by defining a foundational `Spatial` class with a bounding box and crs projection. It then allows for it to be extended for vector data types - points, lines, polygons and grid, creating for example `SaptialLines` object and if needed a additional optional attributes could be added to this object. This useful property of creating your own spatial objects has allowed it be used (directly dependent) in 350+ packages in R.[2]

3. [tmap](https://cran.r-project.org/web/packages/tmap/vignettes/tmap-getstarted.html): I am a big fan of this package personally and find it so easy to use. It takes it's inspiration from `ggplot` and has layered and flexible structure to its syntax. If you use `ggplot`, want to make quick [thematic map](https://en.wikipedia.org/wiki/Thematic_map) and have a geometry column in your data it's your go to package. If your a user and want to contribute to it's development, you can contribute your user feedback [here](https://github.com/mtennekes/tmap/issues/495) towards `tmap`'s next upgrade.  

<h4> Putting it all together</h4>

- Vector data mapping/viz

- making buffers

<h3> Bibliography </h3>

1. [Geocomputation in R](https://geocompr.robinlovelace.net/intro.html#the-history-of-r-spatial)
2. [Using Spatial Data with R](https://cengel.github.io/R-spatial/intro.html)
