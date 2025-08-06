# Range-wide analysis of *Monarda fistulosa* flower color with [iNaturalist](https://www.inaturalist.org/) data

[![DOI](https://zenodo.org/badge/967532298.svg)](https://doi.org/10.5281/zenodo.15485462)  

# ACCESS INFORMATION

## 1. Licenses/restrictions placed on the data or code

MIT License  
Copyright (c) 2025 Patrick McKenzie  
See the [license desciption](https://github.com/pmckenz1/monarda_fistulosa_color/blob/main/LICENSE) for more information.  

## 2. Data derived from other sources

GBIF export, 19 February 2025: https://doi.org/10.15468/dl.v64sz8  

## 3. Recommended citation for this archive

High-throughput iNaturalist image analysis reveals flower color divergence in Monarda fistulosa. 
Patrick F. McKenzie, Samuel H. Church, Robin Hopkins. 
bioRxiv 2025.05.21.655392; doi: https://doi.org/10.1101/2025.05.21.655392. 

# DATA & CODE FILE OVERVIEW

This data repository consist of 1 composite data file, 1 raw data file (GBIF export), 3 data intermediates, 6 code scripts for creating the composite data file, 14 code scripts for creating figures and running analyses, a directory for a validation analysis (containing 4 data files and 1 code script) and this README document, with the following data and code filenames and variables.

## Data files and variables

1. `datasets/geo_med.csv`  

The composite dataset created by our floral color phenotyping pipeline. It contains 20628 rows corresponding to the 20,628 images for which we phenotyped flower color. **This is the primary dataset for the study and was used to produce all downstream figures and analysis.**  
*Variables:*  
image_idx: (int) An integer mapping to the index of the corresponding downloaded image (i.e., if images are downloaded according to the pipeline code, they will have integer names such as '1.jpg').  
hex: (str) The hex-color value representing flower color in the image.  
rgb: (int, int, int) The rgb-color values representing flower color in the image.  
hsl: (flt, flt, flt) The hsl-color values representing flower color in the image.  
lab: (int, int, int) The CIELAB-color values representing flower color in the image.  
gbifID: (int) The GBIF identifier for the corresponding observation.  
identifier: (str) The URL path to the image.  
latitude: (flt) The latitude of the corresponding observation (decimal degrees).  
longitude: (flt) The longitude of the corresponding observation (decimal degrees).  

2. `raw_data/0002206-250218110819086/`  

The raw data exported from GBIF.  

* `occurrence.txt`  
A table with information pertaining to occurrences of *Monarda fistulosa* from the GBIF export of research-grade iNaturalist observations. Each row corresponds to one iNaturalist observation.  
* `multimedia.txt`  
A table of multimedia associated with the occurrences found in `occurrence.txt` and the information corresponding to each multimedia item. Note that multiple rows of `multimedia.txt` might correspond to the same row in `occurrence.txt`, e.g. if an observation is accompanied by more than one image.  

*A list of variable names and links to variable descriptions for the raw GBIF export is located in the file `raw_data/0002206-250218110819086/meta.xml`*  

### Intermediate data files

* Script to download all color-labeled images (output of **Pipeline Step 2**): `download_image_dataset.ipynb`  
* Image idxs containing flowers (output of **Pipeline Step 4**): `gpt_image_filtering.csv`  
* Segmentation masks (output of **Pipeline Step 8**): `segmentation_results.zip`  

## Code

The code is broken into two main directories: `notebooks_pipeline` and `notebooks_figures`.
* `notebooks_pipeline` contains notebooks documenting the steps we took to create our full dataset, including downloading images, filtering them with GPT, and phenotyping flower color.
* `notebooks_figures` contains code to reproduce figures and analyses for the manuscript. Everything in this directory is simply analysis of the data produced previously by `notebooks_pipeline`.

### `notebooks_pipeline`

We apply the following pipeline to assemble the composite dataset:  
1) [GBIF](https://www.gbif.org/) export of all Monarda fistulosa observations in North America  
* Method: GBIF in-browser export tools  
* Data file: `raw_data/`  
2) Download all images associated with these observations  
* Method: jupyter notebook (python)  
* `notebooks_pipeline/1_download_write_images.ipynb`  
3) Query chatGPT to filter image dataset to identify which images feature flowers  
* Method: jupyter notebook (python)  
* `notebooks_pipeline/2_gpt_filtering.ipynb`  
* Raw batch submission files and GPT output files in `gpt_raw_labeling/`  
4) Merge the GPT outputs to make a dataframe mapping images to "YES" or "NO" to whether they contain a flower  
* Method: jupyter notebook (python)  
* `notebooks_pipeline/3_merging_filtering_gpt.ipynb`  
* Data file: `intermediates/gpt_image_filtering.csv`  
5) Using csv with GPT labels, filter out images to only include those containing flowers  
* Method: jupyter notebook (python)  
* `notebooks_pipeline/3_merging_filtering_gpt.ipynb`  
6) Randomly sample some images to train a segmentation model on [Roboflow](https://roboflow.com/)  
* Method: jupyter notebook (python)  
* `notebooks_pipeline/3_merging_filtering_gpt.ipynb`  
7) Manually annotate photos on the Roboflow platform and train model to recognize "flower" pixels  
* Method: Roboflow in-browser tools for upload, image annotation, and model training
* [Trained model](https://universe.roboflow.com/patricks-dashboard/monarda_fistulosa_segmentation/model/1)  
* API: `"https://segment.roboflow.com/monarda_fistulosa_segmentation/1?api_key={your_api_key}"`  
8) Query the trained segmentation model for every image containing flowers  
* Method: jupyter notebook (python)  
* `notebooks_pipeline/4_query_segmentation_model.ipynb`  
* All segmented masks in: `intermediates/segmentation_results.zip`  
9) Use the segmentation mask to extract "flower" pixels and apply geometric median to
identify the dominant color among the extracted pixels.  
* Method: jupyter notebook (python)  
* `notebooks_pipeline/5_create_full_dataframe.ipynb`  
10) Save a composite dataframe containing all images, their corresponding occurrence ids, the dominant color
identified (in multiple color codes), and latitude and longitude.  
* Method: jupyter notebook (python)  
* `notebooks_pipeline/5_create_full_dataframe.ipynb`  
* Composite dataframe: `datasets/geo_med.csv`  
11) Inspect the extracted colors to confirm that they are in the range of what we expected.  
* Method: jupyter notebook (python)  
* `notebooks_pipeline/6_inspect_color_distribution.ipynb`

### `notebooks_figures`

* `figure_1.ipynb`  
Figure 1 is conceptual; in this notebook we simply query the locations for each photo used in the demonstration so we can place the points accurately on the map in Figure 1D.  

* figure_2a.ipynb`  
Contains code for reproducing the map in Figure 2A.

* figure_2b.ipynb`  
Contains code for reproducing the box-and-whisker plots comparing east vs. west in Figure 2B, as well as applying Hotelling's statistics to east vs. west.

* `figure_3a.ipynb`  
Contains code for reproducing the 3D histogram in LCh space shown in Figure 3A.

* `figure_3b.ipynb`  
Contains code for reproducing the 2D-project of hue vs. chroma in Figure 3B, as well as sampling and examining specific outlier points.

* `figure_3c.ipynb`  
Contains code for reproducing the sliding Hotelling's statistic test and producing its plot in Figure 3C.

* `figure_3cd.ipynb`  
Contains code for reproducing the basemap used in Figure 3C and Figure 3D.

* `figure_3d.ipynb`  
Contains code for reproducing the k-means clustering sliding analysis shown in Figure 3D.

* `figure_3ef.ipynb`  
Contains code for reproducing the validation analysis and figures shown in Figure 3E and Figure 3F, as well as for reproducing the statistical testing (MANOVA and ANOVAs).

* `figure_s1.ipynb`  
Contains code for reproducing the map of individual observations shown in Figure S1.

* `figure_s2.ipynb`  
Contains code for reproducing the CIELAB color comparisons shown in Figure S2.

* `figure_s3.ipynb`  
Contains code for reproducing the fully rotated 3D color histogram shown in Figure S3.

* `figure_s4.ipynb`  
Contains code for reproducing the 2D CIELAB and LCh projections shown in Figure S4.

* `figure_s5.ipynb`  
Contains code for reproducing the map k-means clustering assignment for individual observations shown in Figure S5.


# SOFTWARE VERSIONS

All software versions are reported in the notebook `software_versions.ipynb`.
