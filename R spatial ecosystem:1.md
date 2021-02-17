# Spatial Ecosystem in R
<h3>Part 1 </h3>

*Kiranmayi Vadlamudi*

If your starting out as a beginner in learning to handle spatial data, you might find the open source spatial ecosystem in R vast and bit overwhelming to navigate. Choosing the right tool for your analysis can save a lot of time and make ones life much easier. In this article I will cover the bare minimum needed to get away with some simple yet beautiful mapping and some spatial data wrangling. If you are here for a quick walk through, scroll to the example else if you don't mind reading - have a go!:zap:

The history of R spatial ecosystem dates back to early 2000s with some of earliest versions of the packages being first written in S [1]. While some packages for spatial statistical analysis written then are still available on CRAN, there are several more newer packages on offer now. Academics from different fields such as ecology, transport, computer science, urban planning, quantitative geography etc have always dealt with spatial data but over the last few years, academics from these fields came together to develop the R spatial ecosystem and formalise it.

The increased computational power, access to cloud computing resources and the vast space-time varying data being generated on a daily basis has made spatial data analysis and geocomputation popular & *a need to know* if you want to extract meaningful insights.


1. [sf package (short for Simple Features)](https://r-spatial.github.io/sf/articles/sf1.html)[2] : The release of `sf` in 2016 made working with spatial vector data significantly easier and faster. The `sf` class offers a sticky geometry column for the data which doesn't drop while performing data operations.
Its build on its predecessor `sp` and offers the functionalities of 3 packages - `sp`, `rgeos` and `rgdal` [1] making it the only package you need to deal with most of your vector data handling. The other advantage of `sf` is its integration with `tidyverse` which allows for using of data wrangling functions from the `tidyverse` universe to handle the `sf` spatial data class. `sf` much like `sp` is also becoming integral in development of other spatial packages and integration in R providing better interoperability.

2. [sp package](https://cran.r-project.org/web/packages/sp/sp.pdf): The `sp` is one of the earliest packages from 2000s which was developed to handle spatial data types in R. It standardises spatial objects in R by defining a foundational `Spatial` class with a bounding box and crs projection. It then allows for it to be extended for vector data types - points, lines, polygons and grid, creating for example `SaptialLines` object and if needed an additional optional attributes could be added to this object. This useful property of creating your own spatial objects has allowed it be used (directly dependent) in 350+ packages in R.[2]

3. [tmap](https://cran.r-project.org/web/packages/tmap/vignettes/tmap-getstarted.html): I am a big fan of this package personally and find it so easy to use. It takes it's inspiration from `ggplot` and has layered and flexible structure to its syntax. If you use `ggplot`, want to make quick [thematic maps](https://en.wikipedia.org/wiki/Thematic_map) and have associated location (or geometry) in your data it's your go to package. If your a user and want to contribute to it's development, you can contribute your user feedback [here](https://github.com/mtennekes/tmap/issues/495) towards `tmap`'s next upgrade.  

<h4> Putting it all together</h4>
In the example below I used `tmap` and `sf` alone to show how you could read in a spatial file, visualise it and perform some basic geometric operations. You could do all of the above with just `sf` as well and thats one of the most impressive things about the package, but here I will be using just these too which is pretty good too!

Using traffic counters in Sydney data, I first visualise the counters in Sydney using `tmap`.

``` r
library(sf)
#> Linking to GEOS 3.7.2, GDAL 2.4.2, PROJ 5.2.0
library(tmap)
library(dplyr)
#>
#> Attaching package: 'dplyr'
#> The following objects are masked from 'package:stats':
#>
#>     filter, lag
#> The following objects are masked from 'package:base':
#>
#>     intersect, setdiff, setequal, union
counters_shp = st_read("/Volumes/harddisk/kiran usb_backup/Assignment 2 - ITLS6107/Traffic Count/Sydney_Counters/Sydney_Counters.shp")
#> Reading layer `Sydney_Counters' from data source `/Volumes/harddisk/kiran usb_backup/Assignment 2 - ITLS6107/Traffic Count/Sydney_Counters/Sydney_Counters.shp' using driver `ESRI Shapefile'
#> Simple feature collection with 1399 features and 20 fields
#> geometry type:  POINT
#> dimension:      XY
#> bbox:           xmin: 9600514 ymin: 4395169 xmax: 9701652 ymax: 4480099
#> CRS:            3308
c_trial = st_transform(counters_shp, 7844)

tmap_mode("view")
#> tmap mode set to interactive viewing
tm_basemap(leaflet::providers$Stamen.Toner)+
  tm_shape(c_trial) +
  tm_symbols(col="red", scale = 0.05)
```

![](https://i.imgur.com/lZLIntf.png)

Then I read in NSW locality shapefile that gives me all suburbs in NSW. I then filter the shapefile for suburbs that fall in Greater Sydney, transform its projection, and using `sf::st_join` to find the number of counters within each suburb of Sydney.

``` r

nsw_local = st_read("/Volumes/harddisk/kiran usb_backup/Assignment 2 - ITLS6107/nsw_locality_polygon_shp_gda2020/NSW_LOCALITY_POLYGON_shp.shp")
#> Reading layer `NSW_LOCALITY_POLYGON_shp' from data source `/Volumes/harddisk/kiran usb_backup/Assignment 2 - ITLS6107/nsw_locality_polygon_shp_gda2020/NSW_LOCALITY_POLYGON_shp.shp' using driver `ESRI Shapefile'
#> Simple feature collection with 4588 features and 10 fields
#> geometry type:  POLYGON
#> dimension:      XY
#> bbox:           xmin: 140.9993 ymin: -37.50506 xmax: 159.1054 ymax: -28.15701
#> proj4string:    +proj=longlat +ellps=GRS80 +no_defs
nsw_local$NSW_LOCALI  = tolower(nsw_local$NSW_LOCALI)
SydneySubs = c("Abbotsbury",
               "Abbotsford",
               "Acacia Gardens",
               "Agnes Banks","Airds",
               "Alexandria",
               "Allawah",
               "Alfords Point","Allambie Heights","Allawah","Ambarvale","Angus","Annandale","Annangrove","Arcadia","Arncliffe","Arndell Park","Artarmon","Ashbury","Ashcroft","Ashfield","Asquith","Auburn","Austral","Avalon Beach",
               "Badgerys Creek","Balgowlah","Balgowlah Heights","Balmain","Balmain East","Bangor","Banksia","Banksmeadow","Bankstown","Bankstown Aerodrome","Barangaroo","Barden Ridge","Bardia","Bardwell Park","Bardwell Valley","Bass Hill","Baulkham Hills","Bayview","Beacon Hill","Beaconsfield","Beaumont Hills","Beecroft","Belfield","Bella Vista","Bellevue Hill","Belmore","Belrose","Berala","Berkshire Park","Berowra","Berowra Creek","Berowra Heights","Berowra Waters","Berrilee","Beverley Park","Beverly Hills","Bexley","Bexley North","Bickley Vale","Bidwill","Bilgola Beach","Bilgola Plateau","Birchgrove","Birrong","Blackett","Blacktown","Blair Athol","Blairmount","Blakehurst","Bligh Park","Bondi","Bondi Beach","Bondi Junction","Bonnet Bay","Bonnyrigg","Bonnyrigg Heights","Bossley Park","Botany","Bow Bowing","Box Hill","Bradbury","Breakfast Point","Brighton","Le","Sands","Bringelly","Bronte","Brooklyn","Brookvale","Bundeena","Bungarribee","Burraneer","Burwood","Burwood Heights","Busby",
               "Cabarita","Cabramatta","Cabramatta West","Caddens","Cambridge Gardens","Cambridge Park","Camden","Camden South","Camellia","Cammeray","Campbelltown","Camperdown","Campsie","Canada Bay","Canley Heights","Canley Vale","Canoelands","Canterbury","Caringbah","Caringbah South","Carlingford","Carlton","Carnes Hill","Carramar","Carss Park","Cartwright","Castle Cove","Castle Hill","Castlecrag","Castlereagh","Casula","Catherine Field","Cattai","Cawdor","Cecil Hills","Cecil Park","Centennial Park","Central Business District","Chatswood","Chatswood West","Cheltenham","Cherrybrook","Chester Hill","Chifley","Chippendale","Chipping Norton","Chiswick","Chullora","Church Point","Claremont Meadows","Clarendon","Clareville","Claymore","Clemton Park","Clontarf","Clovelly","Clyde","Coasters Retreat","Cobbitty","Colebee","Collaroy","Collaroy Plateau","Colyton","Como","Concord","Concord West","Condell Park","Connells Point","Constitution Hill","Coogee","Cornwallis","Cottage Point","Cowan","Cranebrook","Cremorne","Cremorne Point","Cromer","Cronulla","Crows Nest","Croydon","Croydon Park","Cumberland Reach","Curl Curl","Currans Hill","Currawong Beach",
               "Daceyville","Dangar Island","Darling Point","Darlinghurst","Darlington","Davidson","Dawes Point","Dean Park","Dee Why","Denham Court","Denistone","Denistone East","Denistone West","Dharruk","Dolans Bay","Dolls Point","Doonside","Double Bay","Dover Heights","Drummoyne","Duffys Forest","Dulwich Hill","Dundas","Dundas Valley","Dural",
               "Eagle Vale","Earlwood","East Gordon","East Hills","East Killara","East Kurrajong","East Lindfield","East Ryde","Eastern Creek","Eastgardens","Eastlakes","Eastwood","Ebenezer","Edensor Park","Edgecliff","Edmondson Park","Elanora Heights","Elderslie","Elizabeth Bay","Elizabeth Hills","Ellis Lane","Elvina Bay","Emerton","Emu Heights","Emu Plains","Enfield","Engadine","Englorie Park","Enmore","Epping","Ermington","Erskine Park","Erskineville","Eschol Park","Eveleigh",
               "Fairfield","Fairfield East","Fairfield Heights","Fairfield West","Fairlight","Fiddletown","Five Dock","Flemington","Forest Glen","Forest Lodge","Forestville","Freemans Reach","Frenchs Forest","Freshwater",
               "Gables","Galston","Georges Hall","Gilead","Girraween","Gladesville","Glebe","Gledswood Hills","Glen Alpine","Glendenning","Glenfield","Glenhaven","Glenmore Park","Glenorie","Glenwood","Glossodia","Gordon","Grantham Farm","Granville","Grasmere","Grays Point","Great Mackerel Beach","Green Valley","Greenacre","Greendale","Greenfield Park","Greenhills Beach","Greenwich","Gregory Hills","Greystanes","Grose Vale","Grose Wold","Guildford","Guildford West","Gymea","Gymea Bay",
               "Haberfield","Hammondville","Harrington Park","Harris Park","Hassall Grove","Haymarket","Heathcote","Hebersham","Heckenberg","Henley","Hillsdale","Hinchinbrook","Hobartville","Holroyd","Holsworthy","Homebush","Homebush West","Horningsea Park","Hornsby","Hornsby Heights","Horsley Park","Hoxton Park","Hunters Hill","Huntingwood","Huntleys Cove","Huntleys Point","Hurlstone Park","Hurstville","Hurstville Grove",
               "Illawong","Ingleburn","Ingleside",
               "Jamisontown","Jannali","Jordan Springs",
               "Kangaroo Point","Kareela","Kearns","Kellyville","Kellyville Ridge","Kemps Creek","Kensington","Kenthurst","Kentlyn","Killara","Killarney Heights","Kings Langley","Kings Park","Kingsford","Kingsgrove","Kingswood","Kingswood Park","Kirkham","Kirrawee","Kirribilli","Kogarah","Kogarah Bay","Ku","ring","gai Chase","Kurmond","Kurnell","Kurraba Point","Kurrajong","Kurrajong Hills","Kyeemagh","Kyle Bay",
               "La Perouse","Lakemba","Lalor Park","Lane Cove","Lane Cove North","Lane Cove West","Lansdowne","Lansvale","Laughtondale","Lavender Bay","Leets Vale","Leichhardt","Len Waters Estate","Leonay","Leppington","Lethbridge Park","Leumeah","Lewisham","Liberty Grove","Lidcombe","Lilli Pilli","Lilyfield","Lindfield","Linley Point","Little Bay","Liverpool","Llandilo","Loftus","Londonderry","Long Point","Longueville","Lovett Bay","Lower Portland","Lucas Heights","Luddenham","Lugarno","Lurnea",
               "Macquarie Fields","Macquarie Links","Macquarie Park","Maianbar","Malabar","Manly","Manly Vale","Maraylya","Marayong","Maroota","Maroubra","Marrickville","Marsden Park","Marsfield","Mascot","Matraville","Mays Hill","McCarrs Creek","McGraths Hill","McMahons Point","Meadowbank","Melonba","Melrose Park","Menai","Menangle Park","Merrylands","Merrylands West","Middle Cove","Middle Dural","Middleton Grange","Miller","Millers Point","Milperra","Milsons Passage","Milsons Point","Minchinbury","Minto","Minto Heights","Miranda","Mona Vale","Monterey","Moore Park","Moorebank","Morning Bay","Mortdale","Mortlake","Mosman","Mount Annan","Mount Colah","Mount Druitt","Mount Kuring","Gai","Mount Lewis","Mount Pritchard","Mount Vernon","Mulgoa","Mulgrave",
               "Narellan","Narellan Vale","Naremburn","Narrabeen","Narraweena","Narwee","Nelson","Neutral Bay","Newington","Newport","Newtown","Nirimba Fields","Normanhurst","North Balgowlah","North Bondi","North Curl Curl","North Epping","North Kellyville","North Manly","North Narrabeen","North Parramatta","North Richmond","North Rocks","North Ryde","North St Ives","North St Marys","North Strathfield","North Sydney","North Turramurra","North Wahroonga","North Willoughby","Northbridge","Northmead","Northwood","Norwest",
               "Oakhurst","Oakville","Oatlands","Oatley","Old Guildford","Old Toongabbie","Oran Park","Orchard Hills","Oxford Falls","Oxley Park","Oyster Bay",
               "Paddington","Padstow","Padstow Heights","Pagewood","Palm Beach","Panania","Parklea","Parramatta","Peakhurst","Peakhurst Heights","Pemulwuy","Pendle Hill","Pennant Hills","Penrith","Penshurst","Petersham","Phillip Bay","Picnic Point","Pitt Town","Pitt Town Bottoms","Pleasure Point","Plumpton","Point Piper","Port Botany","Port Hacking","Potts Hill","Potts Point","Prairiewood","Prestons","Prospect","Punchbowl","Putney","Pymble","Pyrmont",
               "Quakers Hill","Queens Park","Queenscliff",
               "Raby","Ramsgate","Ramsgate Beach","Randwick","Redfern","Regents Park","Regentville","Revesby","Revesby Heights","Rhodes","Richards","Richmond","Richmond Lowlands","Riverstone","Riverview","Riverwood","Rockdale","Rodd Point","Rookwood","Rooty Hill","Ropes Crossing","Rose Bay","Rosebery","Rosehill","Roselands","Rosemeadow","Roseville","Roseville Chase","Rossmore","Rouse Hill","Royal National Park","Rozelle","Ruse","Rushcutters Bay","Russell Lea","Rydalmere","Ryde",
               "Sackville","Sackville North","Sadleir","Sandringham","Sandy Point","Sans Souci","Scheyville","Schofields","Scotland Island","Seaforth","Sefton","Seven Hills","Shalvey","Shanes Park","Silverwater","Singletons Mill","Smeaton Grange","Smithfield","South Coogee","South Granville","South Hurstville","South Maroota","South Penrith","South Turramurra","South Wentworthville","South Windsor","Spring Farm","St Andrews","St Clair","St Helens Park","St Ives","St Ives Chase","St Johns Park","St Leonards","St Marys","St Peters","Stanhope Gardens","Stanmore","Strathfield","Strathfield South","Summer Hill","Surry Hills","Sutherland","Sydenham","Sydney Olympic Park","Sylvania","Sylvania Waters","Sydney",
               "Tallawong","Tamarama","Taren Point","Telopea","Tempe","Tennyson","Tennyson Point","Terrey Hills","The Ponds","The Rocks","The Slopes","Thornleigh","Toongabbie","Tregear","Turramurra","Turrella",
               "Ultimo",
               "Varroville","Vaucluse","Villawood","Vineyard","Voyager Point",
               "Wahroonga","Waitara","Wakeley","Wallacia","Wareemba","Warrawee","Warriewood","Warwick Farm","Waterfall","Waterloo","Watsons Bay","Wattle Grove","Waverley","Waverton","Wedderburn","Wentworth Point","Wentworthville","Werrington","Werrington County","Werrington Downs","West Hoxton","West Killara","West Lindfield","West Pennant Hills","West Pymble","West Ryde","Westleigh","Westmead","Wetherill Park","Whalan","Whale Beach","Wheeler Heights","Wilberforce","Wiley Park","Willmot","Willoughby","Willoughby East","Windsor","Windsor Downs","Winston Hills","Wisemans Ferry","Wolli Creek","Wollstonecraft","Woodbine","Woodcroft","Woodpark","Woollahra","Woolloomooloo","Woolooware","Woolwich","Woronora","Woronora Heights",
               "Yagoona","Yarramundi","Yarrawarrah","Yennora","Yowie Bay",
               "Zetland")
SydneySubs = tolower(SydneySubs)
sydney = filter(nsw_local, NSW_LOCALI %in% SydneySubs) %>% st_as_sf(.)
sydney = st_transform(sydney, 7844)
counter_in_sub = st_join(c_trial, sydney, join = st_within)
#> although coordinates are longitude/latitude, st_within assumes that they are planar
count(as_tibble(counter_in_sub), NSW_LOCALI)
#> # A tibble: 395 x 2
#>    NSW_LOCALI           n
#>  * <chr>            <int>
#>  1 abbotsbury           1
#>  2 agnes banks          2
#>  3 alexandria          10
#>  4 alfords point        1
#>  5 allambie heights     1
#>  6 allawah              1
#>  7 ambarvale            1
#>  8 annandale            3
#>  9 annangrove           1
#> 10 arncliffe            3
#> # … with 385 more rows

tmap_mode("view")
#> tmap mode set to interactive viewing
tm_basemap(leaflet::providers$Stamen.Toner)+
  tm_shape(sydney) +
  tm_polygons()+
  tm_shape(c_trial) +
  tm_symbols(col="red", scale = 0.01)
```

![](https://i.imgur.com/hQKNE2z.png)

<sup>Created on 2021-02-17 by the [reprex package](https://reprex.tidyverse.org) (v1.0.0)</sup>

Since the NSW locaclity shapefile I used isnt clean (watchout for duplicates), tmap automatically focusses it's view to include those grey bits too.
Here's what it looks like zoomed in:
![](images/rspatial1.1.png)

![](images/rspatial1.2.png)
<h3> Bibliography </h3>

1. [Geocomputation in R](https://geocompr.robinlovelace.net/intro.html#the-history-of-r-spatial)
2. [Using Spatial Data with R](https://cengel.github.io/R-spatial/intro.html)
