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

**Normal Cell**

![image](https://user-images.githubusercontent.com/70984749/130335104-b5165de1-962b-475f-a137-c1d15c192ba8.png)

**Cancerous Cell**

![image](https://user-images.githubusercontent.com/70984749/130335475-9683486a-66c7-45f6-b9c4-1561e272e81c.png)

The images from the downloaded dataset were previously normalized at a standard set image size (224,224) and pre-stained to bring out different features in the images. [See this link to learn more about staining.](https://serc.carleton.edu/microbelife/research_methods/microscopy/index.html#:~:text=Cell%20staining%20is%20a%20technique,wall%2C%20or%20the%20entire%20cell.) 

