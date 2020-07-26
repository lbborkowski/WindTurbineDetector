# Wind Turbine Object Detection from Aerial Imagery Using TensorFlow Object Detection API and Google Colab
*Google Colab notebook to detect wind turbines from aerial or satellite images using TensorFlow*

## Overview
This repo contains a Jupyter notebook and supporting files to train a wind turbine object detector using [TensorFlow Object Detection API](https://github.com/tensorflow/models/tree/master/research/object_detection). The notebook is run in [Google Colaboratory](https://colab.research.google.com/notebooks/welcome.ipynb) which provides a free virtual machine with TensorFlow preinstalled and access to a GPU. This simplifies the setup process required to start using TensorFlow for interesting things like object detection. Coupling Google Colab with the open source TensorFlow Object Detection API provides all the tools necessary to train a custom object detection model. 

In this repo, wind turbines are detected from aerial images taken over west-central Iowa. The full pipeline from training to inference is contained in the notebook with detailed explanations for each step in the process. This can serve as a tutorial for those interested in training their own custom object detection model. The process is broken down into three steps: 1. **Training**, 2. **Validation**, and 3. **Wind Turbine Detection and Localization**. A brief overview of each of these steps is provided in the README. Further details of each step are included in the Jupyter notebook. 

The figure below illustrates a typical input image and a few resulting output images with each wind turbine properly detected and classified.

![](/READMEimages/WindTurbineDetectorFigure.png)
> *Original aerial images obtained from [National Agriculture Imagery Program (NAIP) database](https://www.fsa.usda.gov/programs-and-services/aerial-photography/imagery-programs/naip-imagery/)*

## Training
Training was performed on labeled 300 x 300 pixel images that were chipped from the original 5978 x 7648 pixel aerial images from the [National Agriculture Imagery Program (NAIP) database](https://www.fsa.usda.gov/programs-and-services/aerial-photography/imagery-programs/naip-imagery/). Each image contained at least one wind turbine which was labeled using [LabelImg](https://github.com/tzutalin/labelImg). A few of the labeled images are shown below. In total, 392 images were used for training and 80 for testing. This training set included wind turbines of different capacity, manufacturer, and design.

![](/READMEimages/train_01.png) ![](/READMEimages/train_02.png) ![](/READMEimages/train_03.png) ![](/READMEimages/train_04.png) ![](/READMEimages/train_05.png) ![](/READMEimages/train_06.png)

## Validation
A set of unlabeled validation images was kept separate from the train and test sets in order to validate the model. A total of 16 images was used for validation. Due to random data/image augmentation performed during training, the validation results can vary between training runs. However, I've found that at least 15 of the 17 wind turbines in the validation image set are detected with high probability with the default training parameters. I have experienced 100% accuracy (all wind turbines detected correctly) however due to the randomness in training, each trained model will likely provide slightly different results. A few results from the validation step are shown below.  

![](/READMEimages/valid_01.png) ![](/READMEimages/valid_02.png) ![](/READMEimages/valid_03.png) ![](/READMEimages/valid_04.png) ![](/READMEimages/valid_05.png) ![](/READMEimages/valid_08.png) 

## Wind Turbine Detection and Localization
Finally, the trained model is applied to full NAIP images covering a 4 mile by 4 mile area, approximately. To perform detection over this large area, a sliding window approach is used to analyze 300 x 300 pixel images over the 5978 x 7648 pixel original image. Once this analysis is performed, a marker is plotted on the original NAIP image for each detected wind turbine. In addition, the latitude and longitude of each wind turbine is output for verification. Two NAIP images with all the detected wind turbines denoted with red markers are presented below. In addition, a table containing a subset of the wind turbine latitude and longitude coordinates is shown below. The accuracy of the model is high however there are instances where non-wind turbine objects such as houses, barns, or roads are detected and classified as wind turbines.   

![](/READMEimages/NAIP_01.png) ![](/READMEimages/NAIP_02.png)

*Locations of a sample set of detected wind turbines*

| (latitude, longitude)  | (latitude, longitude) | (latitude, longitude) |
| ------------- | ------------- | ------------- |
|(42.62258, -94.81749)|(42.60538, -94.79803)|(42.60152, -94.79103)|
|(41.28510, -94.78513)|(41.27862, -94.76478)|(41.28231, -94.74973)|
|(42.60798, -94.79403)|(42.57709, -94.80176)|(42.60988, -94.81421)|

Launch the notebook in Google Colab by clicking on this badge: [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/lbborkowski/wind-turbine-detector/blob/master/WindTurbineDetector_200529.ipynb)
> *After launching the notebook, verify that "Python2" and "GPU" are selected for the "Runtime type" and "Hardware accelerator" in Runtime -> Change runtime type.*
