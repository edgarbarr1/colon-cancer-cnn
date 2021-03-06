# Colon Cancer Image Classification

### Overview ###

Colon cancer has been deemed the number 3 most common cancer in the world, according to the World Cancer Research Fund. Based on this statistic, it is not a surprise to know that more approximately 19 million colonoscopies are performed each year in the United States.

Some experts believe that some of the main causes of this cancer is the Western food diet along with living a sedentary lifestyle as well as being obese. Unfortunately, [according to the CDC](https://www.cdc.gov/nchs/products/databriefs/db360.htm), the US appears to be on an upward trend in obesity which in turn increases the likelihood of men and women to develop colorectal cancers.

![image](https://user-images.githubusercontent.com/70984749/131425635-edd8270d-e639-4c13-bbe8-a5e4aa332748.png)


Although the mortality rate for the most part appears to be relatively low (80% survival rate), it is important to note that like everything, there is always something to improve with either accurate test results, the time it takes to report those results and the resources available to compile said results.

Currently, as per the [American Cancer Society](https://www.cancer.org/treatment/understanding-your-diagnosis/tests/testing-biopsy-and-cytology-specimens-for-cancer/how-long-does-testing-take.html), it takes 2-3 days to report the findings of a colonoscopy biopsy.

### Objective ###
The objective of this project is to build a Convolutional Neural Network that can get close to the 1-2% accuracy that current medical tests have. We will also strive to have an efficient model that can give accurate results faster than 2-3 days and ideally within the time frame of "same-day" results.

### The Data ###

The dataset used comes from a [Zenodo dataset](https://zenodo.org/record/1214456/files/NCT-CRC-HE-100K.zip?download=1) (cite below).

**Note**: Clicking the link will 

This dataset contains 100,000 images of 9 different types of colorectal tissue. Each class is divided by a subdirectory. The two classes that we will use in our model is the NORM, for normal colon mucosa and TUM for colorectal adenocarcinoma. This will be the basis of the defined classes for our model. In total there are 14,317 images classified TUM and 8,763 images classified NORM.

**Normal Cell**           |  **Cancerous Cell**
:-------------------------:|:-------------------------:
![image](https://user-images.githubusercontent.com/70984749/130335104-b5165de1-962b-475f-a137-c1d15c192ba8.png) |  ![image](https://user-images.githubusercontent.com/70984749/130335475-9683486a-66c7-45f6-b9c4-1561e272e81c.png)

The images from the downloaded dataset were previously normalized at a standard set image size (224,224) and pre-stained to bring out different features in the images. [See this link to learn more about staining.](https://serc.carleton.edu/microbelife/research_methods/microscopy/index.html#:~:text=Cell%20staining%20is%20a%20technique,wall%2C%20or%20the%20entire%20cell.) 

## Before running the notebook ##

_**Due to the amount of images and the large dataset, it is highly encouraged to run this notebook in Google Colab**_

Please make sure to note the following prior to running the notebook:

1. The dataset was downloaded and uploaded to google drive prior to running preprocessing and models
  - In order to run the model smoothly and as is in this notebook, please create a subdirectory in MyDrive named colon_dataset. The dataset `NCT-CRC-HE-100K` will then be added to the newly created subdirectory which contains the NORM and TUM subdirectories we need.
  - Overall our path should look like the following:
    - `/content/drive/MyDrive/colon_dataset/NCT-CRC-HE-100K/TUM`
2. Our images are in `TIF` format, a format that is very resource intensive for our model's and can hinder performance, therefore `tif_to_jpeg` function was created. Make sure to run this function to convert our images into jpeg format prior to running our models.

### Pre-Modeling ###

Before the modeling process I made sure to randomly undersample to prevent a class imbalance. The 8000 images for each class was selected for our training data, 400 images for each class was selected for our validation data, and 360 images for each class was selected as our holdout data. This brought our total number of images used in this dataset to 17,520 images used in total.

The images had to be converted into jpeg format because the dataset is originally in tif format. This allowed the models to run more efficiently without wasting too much time and resources decoding the images.

To prevent having memory (RAM) issues, we ran our model in Google Colab and we used an ImageDataGenerator alongside the `flow_from_directory` method so that the model itself would grab a batch of images from the specified directory, learn from it, then move on to the next batch. 

### Modeling ###

The modeling process consisted of creating a baseline model to get preliminary results and fine tune the model by changing some parameters and adding layers to the Convolutional Neural Network.

The first three models severely underperformed, though that was to be expected since the models were very "shallow", did not go very deep, and trying to get a model to learn about tiny itricacies of cells, requires a much more deep Neural Network that can learn from tiny details.

### Best Model ###

Since our self-created models were not able to learn enough from our images, transfer learning was used as the final approach to see if we can learn from our images. The model chosen was the ResNet50 model. Overall this model was able to learn plenty from the images. We got an overall Recall score of 99% and Precision score of 99%.

Below are the different cofusion matrices comparing the true label and the predictions given by the model. the confusion matrices are labeled based on the data subset used.

**Training Confusion Matrix**             |  **Validation Confusion Matrix**
:-------------------------:|:-------------------------:
![image](https://github.com/edgarbarr1/colon-cancer-cnn/blob/main/images/training_cf.png) |  ![image](https://github.com/edgarbarr1/colon-cancer-cnn/blob/main/images/validation_cf.png)

In order to validate the results, we used some holdout data with an Image Generator with no data augmentation and with value normalization. The dataset used in this case was the testing subset or subdirectory that we named as holdout_data.

**Holdout Confusion Matrix** |
:-----------------:|
![image](https://github.com/edgarbarr1/colon-cancer-cnn/blob/main/images/holdout_cf.png)|

The results we got came in par with our training and validation prediction with a bit higher margin of error. Out of the 720 images predicted 6 were predicted incorrectly. This would show the model correctly predicts 99.2% of the images in the correct classification. These are great results from images that our model has not seen or been trained on.

## Computer Vision ##

We can see the model in action by seeing the LIME package in action. Essentially, the LIME package visualizes what features is the model "looking" or concentrating at to make its category determination. Below are the results of the different images used to viasualize with this package.

**Training Image NORMAL**             |  **Training Image Pros/Cons** | **Training Image Segmentation**
:-------------------------:|:-------------------------:|:-------------------------:
![image](https://github.com/edgarbarr1/colon-cancer-cnn/blob/main/images/image_1_normal.png) |  ![image](https://github.com/edgarbarr1/colon-cancer-cnn/blob/main/images/image_1_pros.png)|![image](https://github.com/edgarbarr1/colon-cancer-cnn/blob/main/images/image_1_segmented.png)

**Training Image CANCER**             |  **Training Image Pros/Cons** | **Training Image Segmentation**
:-------------------------:|:-------------------------:|:-------------------------:
![image](https://github.com/edgarbarr1/colon-cancer-cnn/blob/main/images/image_2_normal.png) |  ![image](https://github.com/edgarbarr1/colon-cancer-cnn/blob/main/images/image_2_pros.png)|![image](https://github.com/edgarbarr1/colon-cancer-cnn/blob/main/images/image_2_segmented.png)

**Validation Image NORMAL**             |  **Validation Image Pros/Cons** | **Validation Image Segmentation**
:-------------------------:|:-------------------------:|:-------------------------:
![image](https://github.com/edgarbarr1/colon-cancer-cnn/blob/main/images/image_3_normal.png) |  ![image](https://github.com/edgarbarr1/colon-cancer-cnn/blob/main/images/image_3_pros.png)|![image](https://github.com/edgarbarr1/colon-cancer-cnn/blob/main/images/image_3_segmented.png)

**Validation Image CANCER**             |  **Validation Image Pros/Cons** | **Validation Image Segmentation**
:-------------------------:|:-------------------------:|:-------------------------:
![image](https://github.com/edgarbarr1/colon-cancer-cnn/blob/main/images/image_4_normal.png) |  ![image](https://github.com/edgarbarr1/colon-cancer-cnn/blob/main/images/image_4_pros.png)|![image](https://github.com/edgarbarr1/colon-cancer-cnn/blob/main/images/image_4_segmented.png)

**Test Image NORMAL**             |  **Test Image Pros/Cons** | **Test Image Segmentation**
:-------------------------:|:-------------------------:|:-------------------------:
![image](https://github.com/edgarbarr1/colon-cancer-cnn/blob/main/images/image_5_normal.png) |  ![image](https://github.com/edgarbarr1/colon-cancer-cnn/blob/main/images/image_5_pros.png)|![image](https://github.com/edgarbarr1/colon-cancer-cnn/blob/main/images/image_5_segmented.png)

**Test Image CANCER**             |  **Test Image Pros/Cons** | **Test Image Segmentation**
:-------------------------:|:-------------------------:|:-------------------------:
![image](https://github.com/edgarbarr1/colon-cancer-cnn/blob/main/images/image_6_normal.png) |  ![image](https://github.com/edgarbarr1/colon-cancer-cnn/blob/main/images/image_6_pros.png)|![image](https://github.com/edgarbarr1/colon-cancer-cnn/blob/main/images/image_6_segmented.png)


# Conclusion #

Based on the restults we have gotten from our model predictions, it is clear that the best model that performed was the ResNet50.

With its 48 convolutional layers, it was able to learn from the images and correctly classify 99.2% of the images correctly.

The results from the model show the power that Machine Learning can have in a very "convoluted" fiel such as the medical field. Although, it is not expected that Machine Learning Models like this one will take over Doctor's jobs, this shows the incredible potential of becoming an essential tool in the medical field.

One of the potential use cases where this model could be used is in the prioritization of patients based on the model predictions. This would allow the most urgent cases needed for medical evaluation to be quickly given the priority and attention needed.

Another potential use case is the monitored deployment of this model. That is, have a validation team that evaluates the predictions this model outputs and validate or invalidate said prediction. This would not only add a safety net in diagnosis but also allow the continued learning of the model as it keeps being trained with new data.

However, this model is not without its potential risks that we need to take into consideration. For example, it was mentioned in the introduction, that the images in this particular dataset were pre-stained. As someone who has no prior medical experience, I cannot account for the different techniques of staining and different techniques of photographing said stains. The differences in techniques could potentially disrupt the model performance and therefore not give accurate predictions on new images.

### Next Steps ###

As mentioned above, the key to determine where this model would be the most useful will be with a little more research of where in a Region is colon cancer affecting the population. Additionally, knowing the distributions and ratios of doctors to patients in a particular region, would further show where this model could be the most effective.

Another step that we would like to explore would be to use different datasets to use in our model. In this particular dataset, we used a model that had a specific "preprocess" of staining the cells. It would be worth exploring how a model would perform without cells having gone through the staining process as the images in this dataset went through. This would most likely save even more resources for doctors and hospitals in low income regions and would make this model an even more valuable tool.

# Citation #

Kather, Jakob Nikolas, Halama, Niels, & Marx, Alexander. (2018). 100,000 histological images of human colorectal cancer and healthy tissue (v0.1) [Data set]. Zenodo. https://doi.org/10.5281/zenodo.1214456
