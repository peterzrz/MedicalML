# Introduction
**Motivation:**

Cancer treatment is an exhaustive and complicated process for both patients and doctors.  Many treatment times take upwards of hours to complete, with doctors having to trace MRI images by hand to plan a patient's treatment.  Any reduction in these manual processes could have a profound effect on the overall care a patient receives.

**Problem statement:**

The manual process of mapping out the ever-changing positions of tumors from MRI scans in order to administer radiation treatment is painstaking and time consuming. This is burdensome on both the patients, who face uncomfortable and inconvenient wait times in an already undesirable position, as well as the medical professionals. This process can be automated using existing machine learning techniques.


# Ideas
**2.5D Input**
- Our dataset would only contain ~400 fully 3D examples to train from.  This is too few examples to train a 3D model.
2.5D architecture will generally train faster than a fully 3D architecture.
Improvement over 2D approach as we are able to preserve some of the spatial relationship between slices.

**ResNet Pretrained Model**
- ResNet was chosen at is the most advance fully convolutional Neural Network available.
The ResNet architecture trains quickly due the the batch normalization layers.
ResNet50 was chosen as it allowed us to move to a larger or smaller architecture if we experienced any under or over fitting.

**U-Net architecture**
- Allows skip connections
- Able to provide more information to each step by include the global and local feature extraction into each layer input
  
**Note:** Fast training time is a concern due to the timeframe in which this project needed to be completed

# Usage

# Methods

**Stage 1: classification (pre-processing)**

- Classifying the given slice into 1 (organs of interest is presented) and 0 (none of the organs of interest is presented)
- Maximize recall to 100% so we do not filter out any useable slices  
- More than 50% of the slices are the cross section of the skin which does not contain organs → filtered with a smaller classifying neural network to save resources and time

<img src="https://github.com/peterzrz/MedicalML/blob/master/images/img3.png" height=50% width=80%>

**Stage 2: Segmentation**

- Feed the went through inputs to segmentation model to yield the mask for the 3 organs of interest
- U-Net architecture with Resnet-50 CNN for downsampling and upsampling
- ViT paired with global feature extraction to provide additional information for upsampling
- Skip connection used to provide local feature extraction to each upsampling layer

<img src="https://github.com/peterzrz/MedicalML/blob/master/images/img2.png" height=70% width=80%>

# Results

**Stage 1 - Classification**

Filter out the images with a threshold 0.00001 instead of usual 0.5
Effectively prevent ⅔ of the images without organs of interest continue to next step

| Segmentation Networks    | Combined Loss   | BCE Loss      | Dice Loss |
| ------------- | ------------- | ------------- | ------------|
| ResNet | 0.1258 |  0.0349 |0.2845|
| ResNet/ ViT | 0.1117 |  0.0366 |0.2476|

**Stage 2 - Segmentation**

Loss function - 0.33 * Focal Loss + 0.67 * Tversky Loss 
Customized Loss to address the class imbalance issue in the data
Focal loss - generalization of BCE loss that measures/minimizes the pixel wise difference
Tversky loss - generalization of Dice loss that measures/maximize the overlapping shape & region

<img src="https://github.com/peterzrz/MedicalML/blob/master/Result_comparison/resnet_vit.png">

