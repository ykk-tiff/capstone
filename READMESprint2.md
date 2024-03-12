### Overview - 2
From sprint 1, preliminary EDA was done to look at the 2 datasets from different sources. Although, the sizing between the two are different:
- MHIST dataset is 224 x 224
- Chaoyang dataset is 512 x 512
<br> We can fix this by scaling the images by resizing or cropping the MHIST dataset. 

<br> Another finding is that the labels in:
- Chaoyang dataset has 4 labels "0" means normal, "1" means serrated, "2" means adenocarinoma, and "3" means adenoma 
- MHIST dataset has 2 lables HP (Hyperplastic Polyp) aka normal polyps have no potential to become malignant or SSA (Sessile Serated Adenoma)

So far: 
| <u>DataFrame        | <u>Label | <u>Count |
|------------------|-------|-------|
| <b>mhist_csv_df     | HP    | 617   |
|                  | SSA   | 360   |
| <b>cy_df_train_df   | 2     | 1404  |
|                  | 0     | 1111  |
|                  | 1     | 842   |
|                  | 3     | 664   |
| <b>cy_df_test_df    | 2     | 840   |
|                  | 0     | 705   |
|                  | 1     | 321   |
|                  | 3     | 273   |

Although the MHIST website's Dataset Description shows 3,152 images are in the zip file only 977 were labeled. [MHIST dataset]()

We're merging the MHIST and Chaoyang datasets to improve the detection of sessile serrated adenomas, which are often missed in diagnoses. By prioritizing sensitivity, our model leans towards over-detection, prompting further tests when SSA is suspected. 

In the following pre-processing/EDA steps I will:
1. [combine images with labels](#1-combine-images-with-labels-with-the-image-datasets)
    - resize the choayang dataset to match mhist's
    - combine chaoyang test and train images with corresponding labels
    - set hp and ssa to the chaoyang labels hp = 0 and ssa = 1 via mapping
    - drop unecessary columns and rename columns to match chaoyang dataset
2. [split the mhist dataset into test and split](#3-split-the-mhist-dataset-into-test-and-split)
3. [merge that to the pre-split test and train chaoyang dataset ](#4-merge-mhist-testsplit-to-pre-split-test-and-train-chaoyang-dataset-repsectively)
4. [run the new dataset through:](#5-run-the-combined-test-and-trained-datasets-through)
- pixel value scaling
- noise reduction
- data augmentation 
- edge detection
5. [prelim model](#baseline-model---simple-neural-network)