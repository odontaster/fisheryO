
<!-- README.md is generated from README.Rmd. Please edit that file -->
[<img align="right" alt="ICES Logo" width="17%" height="17%" src="http://ices.dk/_layouts/15/1033/images/icesimg/iceslogo.png">](http://ices.dk)

fisheryO
========

The fisheryO package is offered to provide documentation of the processes used to download, aggregate, and analyze data for ICES Fisheries Overviews. Further, the package contains R functions to facilitate the standard plotting of these data. ICES data are available to use according to the [ICES Data policy](http://ices.dk/marine-data/guidelines-and-policy/Pages/ICES-data-policy.aspx).

ICES Fisheries Overviews are available for the following ecoregions:

-   [Fisheries Overview of Baltic Sea Ecoregion](http://ices.dk/sites/pub/Publication%20Reports/Advice/2017/2017/Baltic_Sea_Ecoregion_Fisheries_Overview.pdf) ([fisheryO v0.2.1](https://github.com/ices-tools-prod/fisheryO/releases/tag/v0.2.1))
-   [Fisheries Overview of the Greater North Sea Ecoregion](http://ices.dk/sites/pub/Publication%20Reports/Advice/2017/2017/Greater_North_Sea_Ecoregion_Fisheries_Overview.pdf) ([fisheryO v0.2.1](https://github.com/ices-tools-prod/fisheryO/releases/tag/v0.2.1))

Installation
------------

You can install the most recent fisheryO build from github with:

``` r
# install.packages("devtools")
# devtools::install_github("ices-tools-prod/fisheryO")
# library(fisheryO)
```

You can also install the raw data and code used for specific Fisheries Overviews with a "version" tag:

``` r
# install.packages("devtools")
# devtools::install_github("ices-tools-prod/fisheryO", ref = "v0.2")
# library(fisheryO)
```

Work flow
---------

1.  Before the package is built, fisheryO downloads source data from ICES web services and databases and saves the raw data as .rdata files in the /data folder. This serves to create a final version of the data used to create each Fisheries Overview, thatis, ([fisheryO v0.2](https://github.com/ices-tools-prod/fisheryO/releases/tag/v0.2)) can be used to explore the data processing steps for the [Greater North Sea ecoregion Fisheries Overview](http://ices.dk/sites/pub/Publication%20Reports/Advice/2017/2017/Greater_North_Sea_Ecoregion_Fisheries_Overview.pdf). The raw data are available as a "promise" and can be explored extracted using the `data()` function. The nuts and bolts of these download steps can be found in the load\_raw\_data.R file in the /data-raw folder and links to the raw data can be found in the description files.

2.  Raw data processing is dependent on how the data will ultimately be displayed (e.g., figure or table) and several functions modify the raw data. These functions can be viewed in the clean\_raw\_data.R file in the /R folder to see the assumptions and data wrangling steps to move from raw data to figures and tables.

3.  Data aggregating functions are called from within the standard plotting functions, but can be run independently to explore the intermediate data.

The list of data can be found using:

``` r
knitr::kable(as.data.frame(data(package = "fisheryO")$results[,c("Item", "Title")]))
```

| Item                         | Title                                                                              |
|:-----------------------------|:-----------------------------------------------------------------------------------|
| date\_data                   |                                                                                    |
| eco\_shape                   | ICES Ecoregions                                                                    |
| europe\_shape                | Europe map                                                                         |
| ices\_catch\_historical\_raw | Historical Nominal Catches 1950-2010                                               |
| ices\_catch\_official\_raw   | Official Nominal Catches 2006-2015                                                 |
| ices\_shape                  | ICES Statistical Areas                                                             |
| sag\_keys\_raw               | ICES Stock Assessment Graphs database - keys                                       |
| sag\_refpts\_raw             | ICES Stock Assessment Graphs database - reference points                           |
| sag\_stock\_status\_raw      | ICES Stock Assessment Graphs database - stock status output                        |
| sag\_summary\_raw            | ICES Stock Assessment Graphs database - summary information from assessment output |
| species\_list\_raw           | ASFIS list of species                                                              |
| stecf\_effort\_raw           | STECF nominal effort                                                               |
| stecf\_landings\_raw         | STECF landings and discards                                                        |
| stock\_list\_raw             | ICES Stock database                                                                |

If you want more information about the data source for each data file, use the "?<data_name>" notation, e.g., `?ices_catch_historical_raw` function to explore the description and to find a url for the source.

Plots
-----

Some of the more complex plots have the option to be dynamic .html graphics with the `dynamic = TRUE` argument.

If you want more information about the data source used for each plot, use the "?<plot_function>" notation, e.g., `?plot_kobe` function to explore the description.

``` r

area_definition_map("Baltic Sea Ecoregion",
                    data_caption = FALSE,
                    return_plot = TRUE,
                    save_plot = FALSE)
```

![ICES Ecoregions are not quite the same as the ICES areas that most assessments are based on. In fact, much of the historic catch data (`?ices_catch_historical_raw`) is aggregated across multiple ICES areas that are may extend into other ecoregions. The following function will show the discrepancies between the ICES Ecoregions and ICES areas.](README-map_example-1.png)

``` r

ices_catch_plot("Baltic Sea Ecoregion",
                data_caption = FALSE,
                type = "COUNTRY",
                line_count = 9,
                plot_type = "area",
                save_plot = FALSE,
                return_plot = TRUE,
                text.size = 9)
```

![Baltic Sea finfish landings (thousand tonnes) by (current) country from ICES Official Catch Statistics (Official Historical Catches 1950-2005 and Official Nominal Commercial Catches 2006-2015). The top 10 countries with the greatest aggregate catch are displayed separately and the remaining countries are aggregated and displayed as “other”.](README-fig2_example-1.png)

``` r

stecf_plot("Baltic Sea Ecoregion",
           data_caption = FALSE,
           metric = "EFFORT",
           type = "COUNTRY",
           line_count = 6,
           plot_type = "line",
           save_plot = FALSE,
           return_plot = TRUE,
           text.size = 9)
```

![Baltic Sea fishing effort (1000 kW days at sea) by country. There is uncertainty about the effort data available for Finland and Estonia, so fishing effort for these two countries have been omitted from the figure.](README-fig3_example-1.png)

For plots using ICES Stock Assessment data, the `active_year` argument can be used to choose the assessment year. Baltic Sea advice for 2017 is already published, so we can use the most recent data.

``` r
guild_discards_fun("Baltic Sea Ecoregion",
                   data_caption = TRUE,
                   active_year = 2017,
                   save_plot = FALSE,
                   return_plot = TRUE)
```

![Left panel (a): Discard rates as a percentage (%) of the total Baltic Sea catch of benthic, demersal and pelagic species (for all years for which ICES has data). Right panel (b): Landings (green) and discards (orange) in weights (1000 tonnes) of the most recent year, 2016](README-fig7_example-1.png)

Some stocks are fished right at F<sub>MSY</sub> and the number of decimal places can determine the status (e.g., good or bad). `calculate_status = TRUE` calculates the ratio of stock status relative to reference points and might result in a slightly different status than what is found in published advice. From 2017, ICES Stock Assessment Graphs database archives the stock status for each stock as a factor level (e.g., red, green, grey, orange... etc), includes qualitative and "proxy" reference points and `calculate_status = FALSE` should be used.

``` r

stockPie_fun("Baltic Sea Ecoregion",
             fisheries_guild = c("benthic", "demersal", "pelagic"),
             data_caption = FALSE,
             calculate_status = FALSE,
             active_year = 2017,
             save_plot = FALSE,
             return_plot = TRUE)
```

![Status summary of Baltic Sea stocks in 2017 relative to the ICES Maximum Sustainable Yield (MSY) approach and precautionary approach (PA) (excluding salmon and sea-trout). Grey represents unknown reference points. For MSY: green represents a stock that is fished below F<sub>MSY</sub> or the stock size is greater than MSY B<sub>trigger</sub>; red represents a stock status that is fished above FMSY or the stock size is lower than MSY Btrigger. For PA: green represents a stock that is fished below Fpa or the stock size is greater than Bpa; orange represents a stock that is fished between Fpa and Flim or the stock size is between Blim and Bpa; red represents a stock that is fished above Flim or the stock size is less than Blim. Stocks having a fishing mortality below or at Fpa and a stock size above Bpa are defined as being inside safe biological limits. F is in the table denoting the fishing pressure and SSB is in the table denoting the stock size. A detailed classification by stocks is available in Annex 1.](README-fig10_example-1.png)

Plot functions also have a `data_caption` argument that will add the data source to the lower right corner of the margin. If you want to plot the stocks above a certain catch, the `catch_limit` argument can be used. This is particularly useful for ecoregions with many stocks (e.g., Greater North Sea Ecoregion).

``` r

fisheryO::plot_kobe("Greater North Sea Ecoregion", 
                    catch_limit = 10000,
                    guild = "all",
                    active_year = 2016,
                    data_caption = TRUE,
                    return_plot = TRUE,
                    save_plot = FALSE)
```

![](README-kobe_example-1.png)

Stock trends can be grouped by different parameters. `object` specifies the group you want displayed. For the time being, `group_var` is necessary to point the code in the right direction [to do](https://github.com/ices-tools-prod/fisheryO/issues/20).

``` r

fisheryO::stock_trends_fun(object = "Greater North Sea Ecoregion", 
                           plotting_var = "StockCode",
                           grouping_var = "EcoRegion",
                           metric = "MSY",
                           active_year = 2017,
                           data_caption = TRUE,
                           return_plot = TRUE,
                           save_plot = FALSE)
```

![](README-stocktrends_example-1.png)

``` r

# fisheryO::stock_trends_fun(object = "Greater North Sea Ecoregion - demersal stocks", 
#                            plotting_var = "StockCode",
#                            grouping_var = "EcoGuild",
#                            metric = "MSY",
#                            active_year = 2017,
#                            data_caption = TRUE,
#                            return_plot = TRUE,
#                            save_plot = FALSE)
# 
# fisheryO::stock_trends_fun(object = "demersal",
#                            plotting_var = "StockCode",
#                            grouping_var = "FisheriesGuild",
#                            metric = "MSY",
#                            active_year = 2017,
#                            data_caption = TRUE,
#                            return_plot = TRUE,
#                            save_plot = FALSE)
# 
# fisheryO::stock_trends_fun(object = "Greater North Sea Ecoregion", 
#                            grouping_var = "EcoRegion",
#                            plotting_var = "FisheriesGuild",
#                            metric = "MEAN",
#                            active_year = 2017,
#                            data_caption = TRUE,
#                            return_plot = TRUE,
#                            save_plot = FALSE)
```

Notes
-----

References and sources
----------------------

ICES. 2017a. Historical Nominal Catches 1950–2010. Version 30-11-2011. Available at ICES website <http://ices.dk/marine-data/dataset-collections/Pages/Fish-catch-and-stock-assessment.aspx>. Accessed 04-07-2017.

ICES. 2017b. Official Nominal Catches 2006–2015. Version 12-06-2017. Available at ICES website <http://ices.dk/marine-data/dataset-collections/Pages/Fish-catch-and-stock-assessment.aspx>. Accessed 04-07-2017.

ICES. 2017c. [Baltic Sea Ecoregion – Fisheries overview](http://ices.dk/sites/pub/Publication%20Reports/Advice/2017/2017/Baltic_Sea_Ecoregion_Fisheries_Overview.pdf). In Report of the ICES Advisory Committee, 2017. ICES Advice 2017, Book 4, Section 4.2.

ICES. 2017d. [Greater North Sea Ecoregion – Fisheries overview](http://ices.dk/sites/pub/Publication%20Reports/Advice/2017/2017/Greater_North_Sea_Ecoregion_Fisheries_Overview.pdf). In Report of the ICES Advisory Committee, 2017. ICES Advice 2017, Book 9, Section 9.2.

ICES Stock Assessment Graphs database: <http://sg.ices.dk>

ICES Stock Assessment Graphs web services: <http://sg.ices.dk/webservices.aspx>

ICES Stock Database: <http://sd.ices.dk>

ICES Stock Database web services: <http://sd.ices.dk/services/>

STECF. 2016. Scientific, Technical and Economic Committee for Fisheries (STECF) – Fisheries Dependent Information (STECF-16-20). Publications Office of the European Union, Luxembourg; EUR 27758 EN. 858 pp. [doi:10.2788/502445](https://stecf.jrc.ec.europa.eu/documents/43805/1443008/2016-11_STECF+16-20_FDI_JRC104212.pdf).

Development
-----------

fisheryO is developed openly on [GitHub](https://github.com/ices-tools-prod/fisheryO).

Feel free to open an [issue](https://github.com/ices-tools-prod/fisheryO/issues) there if you encounter problems or have suggestions for future versions.
