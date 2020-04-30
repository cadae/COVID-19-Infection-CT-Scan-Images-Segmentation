# COVID-19-Infection-CT-Scan-Images-Segmentation

A machine learning approach to highlight the COVID-19 infected areas inside the patient’s lungs with the CT-scans images. It was trained on a publically available dataset on Kaggle (https://www.kaggle.com/andrewmvd/covid19-ct-scans). The base model is MobileNetV2 which is a lightweight convolutional neural network that particularly good at object segmentation and classification tasks. The reason for using CT instead of X-ray images is because CT is more reliable in terms of infection detection (source: https://www.itnonline.com/content/ct-provides-best-diagnosis-novel-coronavirus-covid-19) and thus the accuracy of the model would be higher.

## Take one example from the training dataset:

The first image was extracted from one of the slices of the NifTi file which can be exported from a CT machine. Each NifTi file can contain multiple slices. The lung mask highlights the patient’s left and right lung. The infection mask highlights the possible areas of COVID-19 infection. The infection masks are annotated by doctors and experts. The last image combines all three images previously.

![Alt text](README_images/img1.png?raw=true "img")

The goal of the model is to use the first image (raw ct-scans file) as the only input and able to produce the last image which highlights the areas of patient’s lungs and the infected areas. This technique is also known as image segmentation.

## Pre-processing

A few key takeaways here:

1. Slicing the numpy arrays to keep just the middle 60% of all CT slices as I found the start and the end of the NifTi files often have empty masks which provide little to no benefits (false positive rate is not important in this case).
2. Rotating the numpy arrays from [height, width, index] to [index, height, width].
3. Adding channels (extra dimension) to the arrays [index, height, width, channel].
4. Converting the images from grayscale to RGB since the MobileNetV2 requires the input to have 3 channels of information.
5. Downscaling the images' resolution to 224x224 since the MobileNetV2 only provides pre-trained weights that up to 224x224. So I figured there weren't many benefits to train the model at a higher resolution and it also reduced the computational cost while training.
6. Replacing the left and right lungs mask value with the same value as I don't see the reasons of separating the lungs in this task. Feel free to correct me if I'm wrong. 
7. Mask representation: 
   - 0: None
   - 1: Lungs
   - 2: Infection
8. Downscaling the masks to same resolution as images using nearest neighbor method to retain the representaion value.
9. Scaling each image in the dataset to have mean 0 and variance 1

## Prediction result before the training:
The true mask is the “correct answer” of this particular image. As you can see, the predicted mask is basically random guessing at this stage.

![Alt text](README_images/img2.png?raw=true "img")

## After 20 epochs training on 60% of the dataset:
I wanted to use Keras's ImageDataGenerator to dynamically feed the images to the model in order to overcome the OOM (out of memory) issue. However, the ImageDataGenerator doesn't support loading NifTi format directly from the directory. I might circle back on this if I have time.

Here are a few examples of the true masks and predicted masks:

Predictions were made on the testing dataset that the model has never encountered before to make sure it has a good generalisation on unseen data. The white spots are the infected areas. The gray areas are patients' lungs. And with the highly quantifiable prediction result we got, it’s very easy to calculate how much areas of the patient’s lungs have been infected and thus know how seriously ill the patient is. This model aims to relieve the pressure of radiologist and medical professionals at the hospital. It's not very practical at diagnosing patients as it will require patients already symptomatic (pneumonia). The final validation accuracy is 99.24%. In other words, the similarity of predicted result and the actual result annotated by doctors is 99.24% (give or take).

![Alt text](README_images/img3.png?raw=true "img")
![Alt text](README_images/img4.png?raw=true "img")
![Alt text](README_images/img5.png?raw=true "img")
![Alt text](README_images/img6.png?raw=true "img")
![Alt text](README_images/img7.png?raw=true "img")

I'm a developer at RxMx. Happy to exchange ideas and improvements on this project. My LinkedIn: https://www.linkedin.com/in/guanhuacao/

Data sources

[1] - Paiva, O., 2020. CORONACASES.ORG - Helping Radiologists To Help People In More Than 100 Countries! | Coronavirus Cases - 冠状病毒病例. [online] Coronacases.org. Available at: <link> [Accessed 20 March 2020].

[2] - Glick, Y., 2020. Viewing Playlist: COVID-19 Pneumonia | Radiopaedia.Org. [online] Radiopaedia.org. Available at: <link> [Accessed 20 April 2020].

Expert Annotations

[3] - Ma Jun, Ge Cheng, Wang Yixin, An Xingle, Gao Jiantao, Yu Ziqi, … He Jian. (2020). COVID-19 CT Lung and Infection Segmentation Dataset (Version Verson 1.0) [Data set]. Zenodo. DOI
