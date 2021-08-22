# Colon Cancer Image Classification

### Overview ###

Colon cancer has been deemed the number 3 most common cancer in the world, according to the World Cancer Research Fund. Based on this statistic, it is not a surprise to know that more approximately 19 million colonoscopies are perfeormed each year in the United States.

Some experts believe that some of the main causes of this cancer is the Western food diet along with living a sedentary lifestyle as well as being obese. Unfortunately, according to the CDC, the US appears to be on an upward trend in obesity which in turn increases the likelihood of men and women to develop colorectal cancers.

Although the mortality rate for the most part appears to be relatively low (80% survival rate), it is important to note that like everything, there is always something to improve with either accurate test results, the time it takes to report those results and the resources available to compile said results.

Currently, as per the American Cancer Society, it takes 2-3 days to report the findings of a colonoscopy biopsy.

### Objective ###
This notebook has the objective of building a Convolutional Neural Network that can get close to the 1-2% accuracy that current medical tests have. We will also strive to have an efficient model that can give accurate results faster than 2-3 days and ideally within the time frame of "same-day" results.

### The Data ###

The dataset in this notebook comes from a [Zenodo dataset](https://zenodo.org/record/1214456/files/NCT-CRC-HE-100K.zip?download=1) (cite below).

**Note**: Clicking the link will 

This dataset contains 100,000 images of 9 different types of colorectal tissue. Each class is divided by a subdirectory. The two classes that we will use in our model is the NORM, for normal colon mucosa and TUM for colorectal adenocarcinoma. This will be the basis of the defined classes for our model. In total there are 14,317 images classified TUM and 8,763 images classified NORM.

**Normal Cell**           |  **Cancerous Cell**
:-------------------------:|:-------------------------:
![image](https://user-images.githubusercontent.com/70984749/130335104-b5165de1-962b-475f-a137-c1d15c192ba8.png) |  ![image](https://user-images.githubusercontent.com/70984749/130335475-9683486a-66c7-45f6-b9c4-1561e272e81c.png)

The images from the downloaded dataset were previously normalized at a standard set image size (224,224) and pre-stained to bring out different features in the images. [See this link to learn more about staining.](https://serc.carleton.edu/microbelife/research_methods/microscopy/index.html#:~:text=Cell%20staining%20is%20a%20technique,wall%2C%20or%20the%20entire%20cell.) 

**Before running this notebook**

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

Since our self-created models were not able to learn enough from our images, transfer learning was used as the final approach to see if we can learn from our images. The model chosen was the ResNet50 model. Overall this model was able to learn plenty from the images. We got an overall Recall score of 100% and Precision score of 99%. 

When using the model to predict the cell's classification, we found via a confusion matrix that out of the 17,520 images predicted only 1 was predicted incorrectly. This image, as seen below, was incorrectly classified as a CANCER cell when it was actually a NORMAL cell. This shows the sensitivity of the model to accurately predict cancer cells.

**Training Confusion Matrix**             |  **Validation Confusion Matrix**
:-------------------------:|:-------------------------:
![image](https://user-images.githubusercontent.com/70984749/130340701-c5054fa9-6502-4abb-be6b-8926a37d7ebc.png) |  ![image](https://user-images.githubusercontent.com/70984749/130340704-b7962f82-4db1-48cf-9ced-0b1c1711c367.png)

In order to validate the results, we used some holdout data with an Image Generator with no data augmentation and with value normalization. 

**Holdout Confusion Matrix** |
:-----------------:|
![image](https://user-images.githubusercontent.com/70984749/130364024-9864e94a-e5b8-4c19-b7a9-70258eb490b8.png)|

The results we got came in par with our training and validation prediction with a bit higher margin of error. Out of the 720 images predicted 5 were predicted incorrectly. This would show the model correctly predicts 99.3% of the images in the correct classification. Additionally, it is important to point out that most of the incorrect predictions were for NORMAL cells that were classified as CANCER cells showing the sensitivity of the Recall metric which is of upmost importance in this classification problem. This would make our model be on par with doctor's determinations when giving a colon cancer diagnosis (1-2% margin of error).

# Conclusion #

The results form the model show the power that Machine Learning can have in a very convoluted field such as the medical field. Although, it is not expected that Machine Learning models such as this one will take over Doctors' jobs, this shows the incredible potential of becoming an essential tool in the medical field. 

This model can become even more useful in places where the number of patients outnumber the number doctors available. Those places would probably be low income communities and, likely, communities with widespread obesity. However, this can only be determined with more reaserach

### Next Steps ###

As mentioned above, the key to determine where this model would be the most useful will be with a little more research of where in a Region is colon cancer affecting the population. Additionally, knowing the distributions and ratios of doctors to patients in a particular region, would further show where this model could be the most effective.

Another step that we would like to explore would be to use different datasets to use in our model. In this particular dataset, we used a model that had a specific "preprocess" of staining the cells. It would be worth exploring how a model would perform without cells having gone through the staining process as the images in this dataset went through. This would most likely save even more resources for doctors and hospitals in low income regions and would make this model an even more valuable tool.

# Citation #

Kather, Jakob Nikolas, Halama, Niels, & Marx, Alexander. (2018). 100,000 histological images of human colorectal cancer and healthy tissue (v0.1) [Data set]. Zenodo. https://doi.org/10.5281/zenodo.1214456
