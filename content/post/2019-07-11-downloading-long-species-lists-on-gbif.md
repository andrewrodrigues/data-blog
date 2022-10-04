---
title: Downloading occurrences from a long list of species in R and Python
author: John Waller and Marie Grosjean
date: '2019-09-04'
slug: downloading-long-species-lists-on-gbif
categories:
  - GBIF
tags:
  - API
  - downloads
  - tree list
lastmod: '2019-08-28T16:26:18+02:00'
draft: no
keywords: []
description: ''
authors: ''
comment: no
toc: ''
autoCollapseToc: no
postMetaInFooter: no
hiddenFromHomePage: no
contentCopyright: no
reward: no
mathjax: no
mathjaxEnableSingleDollar: no
mathjaxEnableAutoNumber: no
hideHeaderAndFooter: no
flowchartDiagrams:
  enable: no
  options: ''
sequenceDiagrams:
  enable: no
  options: ''
---

> It is now possible to download up to **100,000** names on **GBIF!**

Until recently it was not possible to download occurrences for more than a few hundred species at the same time, but it is now possible to request more species names (**up to 100,000 taxonkeys**). 

<!--more-->

> **NOTE:** If your request can easily be summarized into higher taxon groups, it still makes more sense to download just that taxon group. For example, if you just want to download all [dragonflies](https://www.gbif.org/occurrence/search?taxon_key=789), all <br> [mammals](https://www.gbif.org/occurrence/search?taxon_key=359), or all [vascular plants](https://www.gbif.org/occurrence/search?taxon_key=7707728). These requests don't require anything special.

Downloads through the [web interface](https://www.gbif.org/occurrence/search) are still **limited to around 200 names (taxonkeys)** (species, genus, family, kingdom). This is due to [limitations in browser](https://stackoverflow.com/questions/812925/what-is-the-maximum-possible-length-of-a-query-string). A search for [300 bird species](https://www.gbif.org/occurrence/search?taxon_key=2103261&taxon_key=2473325&taxon_key=2473332&taxon_key=2473336&taxon_key=2473337&taxon_key=2473339&taxon_key=2473341&taxon_key=2473353&taxon_key=2473356&taxon_key=2473362&taxon_key=2473365&taxon_key=2473421&taxon_key=2473442&taxon_key=2473446&taxon_key=2473447&taxon_key=2473454&taxon_key=2473455&taxon_key=2473463&taxon_key=2473465&taxon_key=2473477&taxon_key=2473479&taxon_key=2473482&taxon_key=2473483&taxon_key=2473484&taxon_key=2473492&taxon_key=2473507&taxon_key=2473510&taxon_key=2473514&taxon_key=2473529&taxon_key=2473534&taxon_key=2473535&taxon_key=2473539&taxon_key=2473540&taxon_key=2473543&taxon_key=2473544&taxon_key=2473545&taxon_key=2473559&taxon_key=2473562&taxon_key=2473574&taxon_key=2473577&taxon_key=2473588&taxon_key=2473599&taxon_key=2473605&taxon_key=2473606&taxon_key=2473612&taxon_key=2473613&taxon_key=2473614&taxon_key=2473621&taxon_key=2473622&taxon_key=2473628&taxon_key=2473629&taxon_key=2473633&taxon_key=2473641&taxon_key=2473642&taxon_key=2473645&taxon_key=2473646&taxon_key=2473647&taxon_key=2473648&taxon_key=2473649&taxon_key=2473650&taxon_key=2473654&taxon_key=2473655&taxon_key=2473657&taxon_key=2473659&taxon_key=2473663&taxon_key=2473678&taxon_key=2473680&taxon_key=2473682&taxon_key=2473695&taxon_key=2473696&taxon_key=2473700&taxon_key=2473702&taxon_key=2473719&taxon_key=2473732&taxon_key=2473733&taxon_key=2473744&taxon_key=2473776&taxon_key=2473958&taxon_key=2474021&taxon_key=2474029&taxon_key=2474045&taxon_key=2474051&taxon_key=2474055&taxon_key=2474060&taxon_key=2474074&taxon_key=2474075&taxon_key=2474077&taxon_key=2474078&taxon_key=2474079&taxon_key=2474084&taxon_key=2474087&taxon_key=2474089&taxon_key=2474095&taxon_key=2474096&taxon_key=2474098&taxon_key=2474099&taxon_key=2474100&taxon_key=2474101&taxon_key=2474107&taxon_key=2474113&taxon_key=2474120&taxon_key=2474127&taxon_key=2474128&taxon_key=2474138&taxon_key=2474139&taxon_key=2474141&taxon_key=2474145&taxon_key=2474156&taxon_key=2474162&taxon_key=2474163&taxon_key=2474165&taxon_key=2474170&taxon_key=2474238&taxon_key=2474251&taxon_key=2474252&taxon_key=2474259&taxon_key=2474263&taxon_key=2474264&taxon_key=2474320&taxon_key=2474328&taxon_key=2474332&taxon_key=2474337&taxon_key=2474343&taxon_key=2474345&taxon_key=2474351&taxon_key=2474354&taxon_key=2474356&taxon_key=2474360&taxon_key=2474363&taxon_key=2474370&taxon_key=2474372&taxon_key=2474377&taxon_key=2474381&taxon_key=2474382&taxon_key=2474383&taxon_key=2474386&taxon_key=2474388&taxon_key=2474389&taxon_key=2474390&taxon_key=2474391&taxon_key=2474392&taxon_key=2474393&taxon_key=2474398&taxon_key=2474400&taxon_key=2474404&taxon_key=2474407&taxon_key=2474416&taxon_key=2474443&taxon_key=2474448&taxon_key=2474453&taxon_key=2474455&taxon_key=2474460&taxon_key=2474468&taxon_key=2474472&taxon_key=2474478&taxon_key=2474479&taxon_key=2474481&taxon_key=2474482&taxon_key=2474484&taxon_key=2474486&taxon_key=2474489&taxon_key=2474493&taxon_key=2474520&taxon_key=2474521&taxon_key=2474522&taxon_key=2474523&taxon_key=2474524&taxon_key=2474525&taxon_key=2474533&taxon_key=2474535&taxon_key=2474540&taxon_key=2474542&taxon_key=2474543&taxon_key=2474544&taxon_key=2474547&taxon_key=2474548&taxon_key=2474558&taxon_key=2474575&taxon_key=2474585&taxon_key=2474588&taxon_key=2474589&taxon_key=2474590&taxon_key=2474596&taxon_key=2474597&taxon_key=2474598&taxon_key=2474599&taxon_key=2474603&taxon_key=2474605&taxon_key=2474607&taxon_key=2474611&taxon_key=2474612&taxon_key=2474614&taxon_key=2474615&taxon_key=2474616&taxon_key=2474619&taxon_key=2474620&taxon_key=2474622&taxon_key=2474627&taxon_key=2474628&taxon_key=2474636&taxon_key=2474638&taxon_key=2474639&taxon_key=2474640&taxon_key=2474648&taxon_key=2474653&taxon_key=2474656&taxon_key=2474657&taxon_key=2474658&taxon_key=2474659&taxon_key=2474669&taxon_key=2474670&taxon_key=2474671&taxon_key=2474681&taxon_key=2474684&taxon_key=2474685&taxon_key=2474702&taxon_key=2474703&taxon_key=2474707&taxon_key=2474711&taxon_key=2474715&taxon_key=2474724&taxon_key=2474728&taxon_key=2474732&taxon_key=2474736&taxon_key=2474738&taxon_key=2474747&taxon_key=2474748&taxon_key=2474750&taxon_key=2474754&taxon_key=2474795&taxon_key=2474796&taxon_key=2474797&taxon_key=2474801&taxon_key=2474825&taxon_key=2474830&taxon_key=2474831&taxon_key=2474836&taxon_key=2474837&taxon_key=2474842&taxon_key=2474844&taxon_key=2474846&taxon_key=2474848&taxon_key=2474850&taxon_key=2474855&taxon_key=2474863&taxon_key=2474864&taxon_key=2474868&taxon_key=2474872&taxon_key=2474873&taxon_key=2474874&taxon_key=2474878&taxon_key=2474880&taxon_key=2474882&taxon_key=2474884&taxon_key=2474887&taxon_key=2474890&taxon_key=2474896&taxon_key=2474897&taxon_key=2474898&taxon_key=2474902&taxon_key=2474903&taxon_key=2474904&taxon_key=2474909&taxon_key=2474913&taxon_key=2474918&taxon_key=2474921&taxon_key=2474925&taxon_key=2474936&taxon_key=2474937&taxon_key=2474941&taxon_key=2474942&taxon_key=2474943&taxon_key=2474944&taxon_key=2474945&taxon_key=2474949&taxon_key=2474950&taxon_key=2474953&taxon_key=2474960&taxon_key=2474961&taxon_key=2474963&taxon_key=2474965&taxon_key=2474966&taxon_key=2474967&taxon_key=2474968&taxon_key=2474971&taxon_key=2474976&taxon_key=2474981&taxon_key=2474982&taxon_key=2475001&taxon_key=2475012&taxon_key=2475016&taxon_key=2475017&taxon_key=2475033&taxon_key=2475035&taxon_key=2475036&taxon_key=2475038&taxon_key=2475040&taxon_key=2475049&taxon_key=2475050&taxon_key=2475051) **fails**. 


# Downloading occurrences for 60,000 tree species using rgbif and taxize

> **NOTE:** This post has been updated on Feb 9 2022. Please use the lastest version of rgbif.

One good reason to download data using a long list of names, would be if your group of interest is non-monophyletic. **Trees** are a non-monophyletic group that include a variety of plant species that have independently evolved a trunk and branches. The 
[Botanic Gardens Conservation <br> International](https://www.bgci.org/) maintains a [long list](https://tools.bgci.org/global_tree_search.php) of **>60,000** tree species. You can download a csv [here](https://tools.bgci.org/global_tree_search_trees_1_3.csv) or the one I used for the example below [here](/post/2019-07-11-downloading-long-species-lists-on-gbif_files/global_tree_search_trees_1_3.csv). 

Matching and downloading takes around **30 minutes**, so run with fewer names if you don't want to wait that long. This requires the latest version of `rgbif`. 

```R
install.packages("rgbif")
```

The important part here is to use `rgbif::occ_download` with `pred_in` and to **fill in your gbif credentials**. Or follow instructions [here](https://docs.ropensci.org/rgbif/articles/gbif_credentials.html).


```R
# fill in your gbif.org credentials 
user <- "" # your gbif.org username 
pwd <- "" # your gbif.org password
email <- "" # your email 
```

```R
library(dplyr)
library(readr)  
library(rgbif) # for occ_download

# The 60,000 tree names file I downloaded from BGCI
file_url <- "https://data-blog.gbif.org/post/2019-07-11-downloading-long-species-lists-on-gbif_files/global_tree_search_trees_1_3.csv"

# match the names 
gbif_taxon_keys <- 
readr::read_csv(file_url) %>%
head(1000) %>% # use only first 1000 names for testing
pull("Taxon name") %>% # use fewer names if you want to just test 
name_backbone_checklist()  %>% # match to backbone
filter(!matchType == "NONE") %>% # get matched names
pull(usageKey) # get the gbif taxonkeys

# gbif_taxon_keys should be a long vector like this c(2977832,2977901,2977966,2977835,2977863)

# !!very important here to use pred_in!!
occ_download(
pred_in("taxonKey", gbif_taxon_keys),
format = "SIMPLE_CSV",
user=user,pwd=pwd,email=email
)
```
The results should now be on your downloads user page https://www.gbif.org/user/download.

**More complex downloads** are also possible with the new `rgbif` `occ_download` interface: 

In the example below, I download all **tree species** which are 

* Matched tree species from above `pred_in("taxonKey", gbif_taxon_keys)`
* With these basis of record `pred_in("basisOfRecord", c('PRESERVED_SPECIMEN','HUMAN_OBSERVATION','OBSERVATION','MACHINE_OBSERVATION'))`
* Greater than 5000 meters elevation `pred_gt("elevation", 5000)` 
* Is found in the United States `pred("country", "US")`
* Has coordinates `pred("hasCoordinate", TRUE)`
* Has no gbif geospatial issues `pred("hasGeospatialIssue", FALSE)`
* Is greater than or equal to the year 1990 `pred_gte("year", 1990)`

```
# use matched gbif_taxon_keys from above 
occ_download(
pred_in("taxonKey", gbif_taxon_keys),
pred_in("basisOfRecord", c('PRESERVED_SPECIMEN','HUMAN_OBSERVATION','OBSERVATION','MACHINE_OBSERVATION')),
pred_gt("elevation", 5000),
pred("country", "US"),
pred("hasCoordinate", TRUE),
pred("hasGeospatialIssue", FALSE),
pred_gte("year", 1990),
format = "SIMPLE_CSV",
user=user,pwd=pwd,email=email
)

```

# Building a complex download requests without rgbif in R 

You also make complex downloads without using rgbif. In this example I download all tree species that occur inside Chile(CL). Some other predicate examples can be found here https://www.gbif.org/developer/occurrence#download.


Fill in your gbif.org credentials 

```R
user <- "" # your gbif.org username 
pwd <- "" # your gbif.org password
email <- "" # your email 
```

```R
library(taxize)
library(purrr)
library(tibble)
library(dplyr)
library(magrittr) # fot the %T>% pipe
library(roperators) # for %+% string operator

# pipeline for processing sci names -> downloads 

# The 60,000 tree names file I downloaded from BGCI
file_url <- "https://data-blog.gbif.org/post/2019-07-11-downloading-long-species-lists-on-gbif_files/global_tree_search_trees_1_3.csv"

# match the names 
readr::read_csv(file_url) %>% 
pull("Taxon name") %>% # use fewer names if you want to just test 
taxize::get_gbifid_(method="backbone") %>% # match names to the GBIF backbone to get taxonkeys
imap(~ .x %>% mutate(original_sciname = .y)) %>% # add original name back into data.frame
bind_rows() %T>% # combine all data.frames into one
readr::write_tsv(path = "all_matches.tsv") %>% # save as side effect for you to inspect if you want
filter(matchtype == "EXACT" & status == "ACCEPTED") %>% # get only accepted and matched names
filter(kingdom == "Plantae") %>% # remove anything that might have matched to a non-plant
pull(usagekey) %>% # get the gbif taxonkeys
paste(collapse=",") %>% 
paste('{
"creator": "' %+% user %+%'",
"notification_address": [
"' %+% email %+% '"
],
"sendNotification": true,
"format": "SIMPLE_CSV",
"predicate": {
"type": "and",
"predicates": [
{
"type": "in",
"key": "COUNTRY",
"values": ["CL"]
},
{
"type": "in",
"key": "TAXON_KEY",
"values": [',.,']
}
]}}',collapse="") %>% # create json sring 
writeLines(file("request.json")) # save the json request to use in httr::POST
```

**request.json** will look like this but with many more values for `TAXON_KEY`:

```json

{
"creator": "jwaller",
"notification_address": [
"jwaller@gbif.org"
],
"sendNotification": true,
"format": "SIMPLE_CSV",
"predicate": {
"type": "and",
"predicates": [
{
"type": "in",
"key": "COUNTRY",
"values": ["CL"]
},
{
"type": "in",
"key": "TAXON_KEY",
"values": [ 2977832,2977901,2977966,2977835,2977863,2977814,8322626 ]
}
]}}


```

Now run the download job.

```R
url = "http://api.gbif.org/v1/occurrence/download/request"

library(httr)

POST(url = url, 
config = authenticate(user, pwd), 
add_headers("Content-Type: application/json"),
body = upload_file("request.json"), # path to your local file
encode = 'json') %>% 
content(as = "text")
```

The results should now be on your downloads user page https://www.gbif.org/user/download. 

# Example using Python

The same example is also available in Python [here](https://github.com/ManonGros/Small-scripts-using-GBIF-API/tree/master/query_species_list). Note that this particular code doesn't use the `pygbif` library but `request` and the GBIF API. It calls two functions available in the same folder, just download <br>[this file](https://github.com/ManonGros/Small-scripts-using-GBIF-API/blob/master/query_species_list/functions_query_from_species_list.py) before running the following code:

```Python
import pandas as pd
import requests
import json
import io
from functions_query_from_species_list import *

login = ""
password = ""
URL_species_file = "https://data-blog.gbif.org/post/2019-07-11-downloading-long-species-lists-on-gbif_files/global_tree_search_trees_1_3.csv"

# Get Taxon Keys
species_list = pd.read_csv(URL_species_file, encoding='latin-1')
taxon_keys = match_species(species_list, "Taxon name")

# filter keys however you see fit
key_list = taxon_keys.loc[(taxon_keys["matchType"]=="EXACT") & (taxon_keys["status"]=="ACCEPTED")].usageKey.tolist()

# Make download query
download_query = {}
download_query["creator"] = ""
download_query["notificationAddresses"] = [""]
download_query["sendNotification"] = False # if set to be True, don't forget to add a notificationAddresses above
download_query["format"] = "SIMPLE_CSV"
download_query["predicate"] = {
    "type": "in",
    "key": "TAXON_KEY",
    "values": key_list
}

# Generate download
create_download_given_query(login, password, download_query)
```

# Citing your download

If you end up using your **download in a research paper**, you will want to cite the download's **DOI**. Please see these [citation guidelines](https://www.gbif.org/citation-guidelines) for properly citing your download. Good citation practices ensure scientific transparency and reproducibility by guiding other researchers to the original sources of information. 

# Acknowledgements 

GBIF thanks the project [Tracking Invasive Alien Species (TrIAS)](https://www.gbif.org/network/b153643d-735a-440f-a0e9-428b4f9d1cd2) for funding the work required to provide these enhancements to all users of GBIF. TrIAS is a Belgian open science project that makes extensive use of GBIF services, see e.g. the [methodology for their national checklist of alien species](https://www.gbif.org/dataset/6d9e952f-948c-4483-9807-575348147c7e#methodology).



<!--more-->
