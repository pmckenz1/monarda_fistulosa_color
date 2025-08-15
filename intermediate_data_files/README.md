### Intermediate data files

Files produced internally within the pipeline in `../notebooks_pipeline` are stored here.  
* `gpt_raw_labeling`: Folder containing the 41069 submissions to the GPT API, and the responses. Each json file contains a set of queries, and each corresponding .csv file contains a set of answers: `YES` or `NO` in the single column `flower_present` (output of **Pipeline Step 4**).  
* `gpt_image_filtering.csv`: Image idxs containing flowers (output of **Pipeline Step 4**).  
* `segmentation_results.zip`:  Segmentation masks (output of **Pipeline Step 8**).  