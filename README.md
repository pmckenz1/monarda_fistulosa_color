# Range-wide analysis of *Monarda fistulosa* flower color with [iNaturalist](https://www.inaturalist.org/) data

We apply the following pipeline:  
1) [GBIF](https://www.gbif.org/) export of all Monarda fistulosa observations in North America  
* `raw_data/`  
2) Download all images associated with these observations  
* `notebooks_pipeline/1_download_write_images.ipynb`  
3) Query chatGPT to filter image dataset to only those images featuring flowers  
* `notebooks_pipeline/2_gpt_filtering.ipynb`  
* Raw batch submission files and output files in `gpt_raw_labeling/`  
4) Merge the GPT outputs to make a dataframe mapping images to "YES" or "NO" to whether they contain a flower  
* `notebooks_pipeline/3_merging_filtering_gpt.ipynb`  
* `gpt_image_filtering.csv`  
5) Filter out images to only include those containing flowers  
* `notebooks_pipeline/3_merging_filtering_gpt.ipynb`  
6) Randomly sample some images to train a segmentation model on [Roboflow](https://roboflow.com/)  
* `notebooks_pipeline/3_merging_filtering_gpt.ipynb`  
7) Manually annotate photos on the Roboflow platform and train model to recognize "flower" pixels  
* [Trained model](https://universe.roboflow.com/patricks-dashboard/monarda_fistulosa_segmentation/model/1)  
* API: `"https://segment.roboflow.com/monarda_fistulosa_segmentation/1?api_key={your_api_key}"`  
8) Query the trained segmentation model for every image containing flowers  
* `notebooks_pipeline/4_query_segmentation_model.ipynb`  
* All segmented masks in: `segmentation_results.zip`  
9) Use the segmentation mask to extract "flower" pixels and apply k-means clustering to
identify the dominant color among the extracted pixels.  
* `notebooks_pipeline/5_create_full_dataframe.ipynb`  
10) Save a composite dataframe containing all images, their corresponding occurrence ids, the dominant color
identified (in multiple color codes), and latitude and longitude.  
* `notebooks_pipeline/5_create_full_dataframe.ipynb`  
* Composite dataframe: `filtered_labeled_data.csv`  

# Primary data files  

* Composite dataframe: `filtered_labeled_data.csv`  

Intermediates:  
* Raw GBIF export: `raw_data/`  
* Image idxs containing flowers: `gpt_image_filtering.csv`  
* Segmentation masks: `segmentation_results.zip`  
* All color-labeled images (to download): `download_image_dataset.ipynb`  

# Figures -- code in notebooks_figures

## Figure 1: Visualizing the segmentation masks.  
![Figure 1](figures/figure1.png)  

## Figure 2: Demonstrating extracted flower colors.  
![Figure 2](figures/figure2.png)  

## Figure 3: Spatial distribution of extracted colors. *Monarda fistulosa* is darker purple in the western U.S.  
![Figure 3](figures/figure3.png)  