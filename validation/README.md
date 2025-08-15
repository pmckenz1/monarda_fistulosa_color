### `validation`

This directory contains the manually binned validation data from coauthors S.H.C. and R.H., and the code used to produce the template. This data is analyzed in the notebooks for Figure 3EF.  

* `preparing_validation_subsets.ipynb`  

Contains code documenting our method for randomly selecting observations from the east vs. west regions, shuffling them, and organizing them into a .csv file to be distributed to coauthors.

* `validation_subset.csv`  

The .csv file to be distributed to coauthors, as output from `preparing_validation_subsets.ipynb`. Extraneous columns were removed, and additional columns were added to this before distributing (see `validation_subset_empty.csv`).  

#### Variables:  
`image_idxs`: (int) The corresponding image.  
`hex_colors`: (str) The phenotyped hex color of the corresponding image.  
`gbifID`: (int) The GBIF identifier for the corresponding observation.  
`latitude`: (flt) The latitude of the corresponding observation (decimal degrees).  
`longitude`: (flt) The longitude of the corresponding observation (decimal degrees).  


* `validation_subset_Robin.csv`  

RH's manual binning of the validation subset into lighting categories.  

#### Variables:  
`image_idxs`: (int) The corresponding image.  
`lighting`: (cat) Labeled by the validator. Either "s" (sun), "h" (shade), "p" (partial), or "n" (nan).  
`monarda`: (cat) Labeled by the validator. Either "n" (indicating no Monarda flower present) or empty (indicating Monarda flower present). *Note, all are empty as validators assessed that Monarda flowers were present in all images.*  

* `validation_subset_Sam.csv`  

SHC's manual binning of the validation subset into lighting categories.  

#### Variables:  
`image_idxs`: (int) The corresponding image.  
`lighting`: (cat) Labeled by the validator. Either "s" (sun), "h" (shade), "p" (partial), or "n" (nan).  
`monarda`: (cat) Labeled by the validator. Either "n" (indicating no Monarda flower present) or empty (indicating Monarda flower present). *Note, all are empty as validators assessed that Monarda flowers were present in all images.*  

* `validation_subset_empty.csv`  

The .csv file to be distributed to coauthors, but with additional headings added.  

#### Variables:  
`image_idxs`: (int) The corresponding image.  
`lighting`: (cat) To be labeled by the validators. Either "s" (sun), "h" (shade), "p" (partial), or "n" (nan).  
`monarda`: (cat) To be labeled by the validators. Either "n" (indicating no Monarda flower present) or empty (indicating Monarda flower present). *Note, all are empty as validators assessed that Monarda flowers were present in all images.*  