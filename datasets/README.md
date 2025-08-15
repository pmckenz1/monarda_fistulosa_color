### Final dataset: `datasets/geo_med.csv`  

The composite dataset created by our floral color phenotyping pipeline. It contains 20628 rows corresponding to the 20,628 images for which we phenotyped flower color. **This is the primary dataset for the study and was used to produce all downstream figures and analysis.**  

#### Variables:
`image_idx`: (int) An integer mapping to the index of the corresponding downloaded image (i.e., if images are downloaded according to the pipeline code, they will have integer names such as '1.jpg').  
`hex`: (str) The hex-color value representing flower color in the image.  
`rgb`: (int, int, int) The rgb-color values representing flower color in the image.  
`hsl`: (flt, flt, flt) The hsl-color values representing flower color in the image.  
`lab`: (int, int, int) The CIELAB-color values representing flower color in the image.  
`gbifID`: (int) The GBIF identifier for the corresponding observation.  
`identifier`: (str) The URL path to the image.  
`latitude`: (flt) The latitude of the corresponding observation (decimal degrees).  
`longitude`: (flt) The longitude of the corresponding observation (decimal degrees).  