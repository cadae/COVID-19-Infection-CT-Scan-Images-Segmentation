# COVID-19-Infection-CT-Scan-images-segmentation

A machine learning based approach to highlight the COVID-19 infected areas inside the patient’s lungs with the CT-scans images. It was trained on a publically available dataset on Kaggle (https://www.kaggle.com/andrewmvd/covid19-ct-scans). The base model is MobileNetV2 which is a lightweight convolutional neural network that particularly good at object segmentation and classification tasks. The reason of using CT instead of X-ray images is because CT is more reliable in terms of infection detection (source: https://www.itnonline.com/content/ct-provides-best-diagnosis-novel-coronavirus-covid-19) and thus the accuracy of the model would be higher. 

Data sources
[1] - Paiva, O., 2020. CORONACASES.ORG - Helping Radiologists To Help People In More Than 100 Countries! | Coronavirus Cases - 冠状病毒病例. [online] Coronacases.org. Available at: <link> [Accessed 20 March 2020].

[2] - Glick, Y., 2020. Viewing Playlist: COVID-19 Pneumonia | Radiopaedia.Org. [online] Radiopaedia.org. Available at: <link> [Accessed 20 April 2020].

Expert Annotations
[3] - Ma Jun, Ge Cheng, Wang Yixin, An Xingle, Gao Jiantao, Yu Ziqi, … He Jian. (2020). COVID-19 CT Lung and Infection Segmentation Dataset (Version Verson 1.0) [Data set]. Zenodo. DOI
