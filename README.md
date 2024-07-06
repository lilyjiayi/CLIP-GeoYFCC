# CLIP-GeoYFCC

## Description
This repository contains a link to the metadata and instructions of CLIP-GeoYFCC, a large-scale multi-domain image dataset built around realistic geographical domains. CLIP-GeoYFCC is introduced in the paper "[Benchmarking Multi-domain Active Learning on Image Classification](https://doi.org/10.48550/arXiv.2312.00364)" and is constructed based on [GeoYFCC](https://github.com/abhimanyudubey/GeoYFCC?tab=readme-ov-file) with relabeling and rebalancing.

The original GeoYFCC dataset contains a subset of the [YFCC100M dataset](https://multimediacommons.wordpress.com/yfcc100m-core-dataset/), that are partitioned based on the images' country of origin. To obtain the original images of YFCC100M, users could download them via [this API](https://pypi.org/project/yfcc100m/). 

In GeoYFCC, images are assigned labels based on filtering keywords of image tags; however, we discover that GeoYFCC contains much label noise as images with the same label show very different subjects. In order to obtain more accurate labels for the images, we use CLIP ViT-G-14 to relabel the GeoYFCC dataset. We calculate image features and match them with labels in ImageNet-21K that have the closest text features. Country tags are used to assign each image to its corresponding continent. We select continents instead of countries to be the domains because countries geographically close to each other demonstrate insignificant domain difference. This results in the **CLIP-GeoYFCC** dataset with 1,146,768 images from 18,814 classes and 6 continents (domains). 

The metadata file is available at [this Google Drive link](https://drive.google.com/file/d/1GT44IiEGIGgnW_WXGnGWO77YQjIpnW5e/view?usp=sharing). The file is tar-zipped and unzipped results in a `pickle` file that stores a `pandas` dataframe:

| Column | Description |
| ----------- | ----------- |
| `yfcc_row_id` | Corresponding row ID within YFCC100M (present in the first column of the `yfcc100m_dataset` file from YFCC100M, begins with `0`) |
| `label_ids` | Label, indexed from 0 to 18813|
| `in_imagenet21k_label_indices` | Corresponding index of label in the ImageNet-21K dataset (see the `imagenet21k_wordnet_ids.txt` file) |
| `continent` | Domain name (continent) |
| `region` | Region such as West Africa, Southeast Asia, etc. |
| `country` | Country name |
| `country_id`| Serialization of country from 0-61 |
| `is_train` | Boolean specifying whether row is in the training image split |
| `file_name` | File name of the image |
| `yfcc_metadata` | Copy of the original YFCC metadata for image |


To match the design of [Domainnet](https://ai.bu.edu/M3SDA/#dataset), a popular multi-domain image dataset, and to further clean up the dataset, we rank samples in CLIP-GeoYFCC by their similarity scores with labels matched by CLIP and select the top 1M samples. The domain of Oceania is removed because it accounts for less than 4% of the total images. To ensure that each class has sufficient samples for training, we select the 350 largest classes where each class has at least 450 samples. For any class with more than 2k samples, we randomly sub-sample to limit it to 2k samples. The resulting 350 class subset with 338,730 images is called **CLIP-GeoYFCC-350**. From here, we further subsample 45 classes to form **CLIP-GeoYFCC-45** containing 86,288 images. These two datasets mirror two publicly released Domainnet versions, Domainnet-345 with 345 classes and 586,575 samples and Domainnet-40 with 40 classes and 72,614 samples.


| Dataset / Subset | # of classes | # of samples | File Link |
| ----------- | ----------- | ----------- | ----------- |
| CLIP-GeoYFCC | 18,814 | 1,146,768 | [metadata file](https://drive.google.com/file/d/1GT44IiEGIGgnW_WXGnGWO77YQjIpnW5e/view?usp=sharing) | 
| CLIP-GeoYFCC-350 | 350 | 338,730 | [350_sample_indices.npy](https://github.com/lilyjiayi/CLIP-GeoYFCC/blob/main/350_sample_indices.npy) |
| CLIP-GeoYFCC-45 | 45 | 86,288 | [45_sample_indices.npy](https://github.com/lilyjiayi/CLIP-GeoYFCC/blob/main/45_sample_indices.npy) |


If you find this dataset useful, please consider citing our work below.
```
@misc{title={Benchmarking Multi-Domain Active Learning on Image Classification}, 
      author={Jiayi Li and Rohan Taori and Tatsunori B. Hashimoto},
      year={2023},
      eprint={2312.00364},
      archivePrefix={arXiv},
      primaryClass={cs.LG}
}
```
