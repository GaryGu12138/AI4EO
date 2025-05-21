# AI4EO Project

The introduction and guide for this project were uploaded to YouTube: https://youtu.be/SIzm7RYnig0



## 1 Project Description

In January 2025, a catastrophic wildfire struck Los Angeles, USA, causing severe damage to local ecosystems and communities. This project focuses on the most severely affected area (Pacific Palisades) and aims to extract vegetation coverage before and after the wildfire using remote sensing imagery. Change detection is applied to analyze the fire’s impact on vegetation.

The project’s practical significance lies in leveraging remote sensing and AI to rapidly quantify pre-/post-fire vegetation changes, providing scientific support for ecological recovery while establishing an automated monitoring framework to improve emergency resource allocation. Furthermore, visualizations of vegetation damage patterns aim to raise public awareness of wildfire prevention. We expect to validate the effectiveness of low-cost satellite data through multi-method analysis (CNN, K-means, NDVI).

## 2 Dataset Description

This project uses two datasets:

#### 2.1 Training Data for CNN Vegetation Classification

- **Source**: [Land Cover Classification Dataset](https://competitions.codalab.org/competitions/18468) from the DeepGlobe Challenge.
- **Specifications**:
  - Format: RGB satellite imagery with **0.5m/pixel resolution**.
  - Labels: 7 classes including `forest`, `farmland`, `water`, `urban`, `bare soil`, `grassland`, and `others`.
  - Size: **1,146 training images** with corresponding masks. (Due to limited computing resources, only 100 sets of images were used)

![image](https://github.com/user-attachments/assets/e6c68191-2774-421f-b6f9-0085c946be1f)

#### 2.2 Wildfire Area Imagery

- **Source**: Sentinel-2 satellite imagery from the [Copernicus Open Access Hub](https://dataspace.copernicus.eu/).
- **Selection Criteria**:
  - Timeframe: Pre-fire (**January 2, 2025**) and post-fire (**February 1, 2025**).
  - Cloud Coverage: Selected images with **<10% cloud cover**.
  - Area of Interest: Focused on the wildfire core zone in Pacific Palisades (coordinates: `34.0°N–34.1°N, 118.5°W–118.6°W`).
- **Data Specifications**:
  - Bands: 13 multispectral bands (10m/20m resolution), with emphasis on **Near-Infrared (B8)** and **Red (B4)** bands.
  - Preprocessing:
    - Radiometric Correction: Converted to surface reflectance.
    - Atmospheric Correction: Processed to L2A level using Sen2Cor.
    - Image Registration: Aligned pre- and post-fire images (error <1 pixel).

![image](https://github.com/user-attachments/assets/08d88e33-4e86-489e-a78e-9c6798eb9c88)

## 3 Methodology

![image](https://github.com/user-attachments/assets/31bfb7f9-1fa4-40e4-8c13-d0e538d7f00e)

Three methods are implemented for vegetation extraction and change detection:

#### 3.1 CNN (Convolutional Neural Network)

Convolutional Neural Networks (CNNs) are a type of deep learning model widely used for image and video recognition, as well as other tasks like natural language processing.

CNNs use convolutional layers to automatically learn spatial hierarchies of features from input data. These layers apply filters to detect features like edges and textures, producing feature maps. Pooling layers then reduce the dimensionality of these feature maps, improving computational efficiency and robustness. Fully connected layers at the end interpret the extracted features to make predictions.

CNNs are particularly effective for image data because they capture local dependencies and patterns with fewer parameters, reducing overfitting. Their ability to automatically learn features from raw data has made them a cornerstone of modern deep learning applications.

![Introduction to Convolutional Neural Networks CNNs](https://cdn-images-1.medium.com/max/1600/1*g6qPMZTpO2Nl9Y2dxwgvCA.png)

The project implements a custom encoder-decoder CNN for vegetation segmentation. The model consists of a 3-layer encoder (channels 8→16→32 with MaxPooling downsampling) and a 2-layer decoder (bilinear upsampling to restore resolution), outputting a same-size binary mask (vegetation/non-vegetation). 

#### 3.2 K-means Clustering

K-Means is a popular unsupervised learning algorithm for clustering data into K distinct groups based on feature similarity. It starts with K random centroids, assigns each data point to the nearest one, and recalculates centroids as the mean of assigned points. This repeats until centroids stabilize. K-Means is effective for exploratory data analysis and works well when the number of clusters is known. Its simplicity and efficiency make it ideal for tasks like image segmentation, customer segmentation, and anomaly detection.

![K-Means Clustering: Use Cases, Advantages and Working Principle](https://bs-cms-media-prod.s3.ap-south-1.amazonaws.com/K_means_clustering_cd34c9feb8.png)

In order to better identify the vegetation area, this project first set the number of clusters to 4, and then after completing the classification, the vegetation category was identified based on the characteristics of the vegetation with a large difference between the red and near-infrared bands.

#### 3.3 NDVI (Normalized Difference Vegetation Index)

The Normalized Difference Vegetation Index (NDVI) is a widely used index for monitoring vegetation health and detecting changes in plant cover. It leverages the reflectance properties of plants in the red and near-infrared (NIR) regions of the electromagnetic spectrum. NDVI is calculated as (NIR - Red) / (NIR + Red). Healthy vegetation reflects more NIR light and absorbs more red light, resulting in higher NDVI values. This index helps in distinguishing between vegetated and non-vegetated areas, making it valuable for environmental monitoring, agriculture, and ecological studies. Generally, higher NDVI values indicate healthier vegetation.

![Systems and Sensors for NDVI Mapping | Agricultural Drone Specialists](https://www.integraldrones.com.au/wp-content/uploads/2017/09/NDVI-Blog.jpg)

When extracting vegetation areas using NDVI, a threshold value needs to be selected, which usually varies in the range of 0-0.5. In this project, the value of this threshold is 0.5, which means that it is more strict in extracting the vegetation range.

## 4 Results

The figure below summarizes the vegetation change detection results from three methods:

![image](https://github.com/user-attachments/assets/1715cc4f-bb0c-4381-8f84-26802cb7a07c)

![image](https://github.com/user-attachments/assets/1605acca-6a72-4f88-8b9e-846144e83f88)

![image](https://github.com/user-attachments/assets/9c22335b-7b2f-434a-8086-203ae8f50fe3)

## 5 Data Source

DeepGlobe Land Cover Dataset: https://competitions.codalab.org/competitions/18468

Copernicus Open Access Hub: https://dataspace.copernicus.eu/
