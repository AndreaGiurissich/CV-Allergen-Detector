<img width="2816" height="1536" alt="Gemini_Generated_Image_zgzd9tzgzd9tzgzd" src="https://github.com/user-attachments/assets/9ced55e0-bd48-4f88-8c75-4319bbd90e31" />  

# FoodWatch, a CV Food Allergen detector
Computer Vision project. We trained different YOLO26 models on the Allergen30 Dataset 

# Goal
Develop a fast CPU inference model for mobile use. We evaluate with precision, recall, F1-Score, mAP50 and mAP50-95.

# Dataset
Allergen30 Dataset:It contains images of 30 commonly used food items that can cause an adverse reaction within a human body. These food items pertain to specific food intolerances which can trigger an allergic reaction. Such food intolerance primarily includes Lactose, Histamine, Gluten, Salicylate, Caffeine, and Ovomucoid intolerance.  

The foundation of the dataset is made of 6000 416*416 hand-labeled images with bounding boxes, divided in train, validation and test (70% 20% 10%).
To create the train set the following augmentation was applied to create 3 versions of each source image:
* Random shear of between -15° to +15° horizontally and -15° to +15° vertically

The following transformations were applied to the bounding boxes of each image:
* Equal probability of one of the following 90-degree rotations: none, clockwise, counter-clockwise, upside-down

This results in **14.5k** images.

# Model training
We trained **YOLO26 nano, small and medium** for 100 epochs with batch size 32.
We used the **AdamW** optimizer with a learning rate of: ```0.001``` and a **Cosine Scheduler**. To prevent overfitting, we used label smoothing. We also combined **Mixup and Mosaic** augmentation to make the model more effective at detecting smaller objects.

# Results (summary)
<img width="453" height="113" alt="table" src="https://github.com/user-attachments/assets/c98b8d38-0052-4eea-a658-62b26795f34d" />  

We compared three versions: Nano, Small, and Medium. As the table shows, the Small version provided the best balance, achieving a **mAP50** of 0.769 and the highest **F1-score** of 0.739.
While slightly lower in raw metrics than the original paper's heavy models, our version is significantly faster for mobile deployment.  

# Limitations & scope
- Small dataset with unbalanced food distribution.

- Stock photos do not represent real case scenarios.

# References & credits
```[1] Mishra, M., Sarkar, T., Choudhury, T. et al. Allergen30: Detecting Food Items with Possible Allergens Using Deep Learning-Based Computer Vision. Food Anal. Methods 15, 3045–3078 (2022); doi: 10.1007/s12161-022-02353-9```  

 Authors : Andrea Giurissich, Matteo Pintore (Sapienza University of Rome).

# Possible extensions
- Dataset improvement with real case scenario photos.
  
- Class alignment with EU regulations on Food Allergens.

___

# Application Overview
We provide a [rar file](https://drive.google.com/file/d/1blV78wv5YvyApWvAFLylTvwJxKW_k1Mi/view?usp=sharing) containing all the source code for the application along with a pre-compiled .apk file for direct installation on Android devices.

# Key File Structure
Once extracted, the core application logic and resources can be found in the following directories:
# Main Logic & Inference 
- ```CVapp/app/src/main/java/com/example/cvapp/MainActivity.kt```
  This file contains the main code and logic of our application managing:
  * UI and Interaction
  * Image acquisition from gallery or camera
  * Model Inference
  * Post Processing with specific approaches to improve detection of problematic foods

Model Loaction
- ```CVapp/app/src/main/assets/model.onnx```  

The pre-trained ONNX model is stored here and loaded by the application at runtime.



# User Interface Layout
- ```CVapp/app/src/main/res/layout/activity_main.xml```
  
  Regulates the layout of the application, with a minimal, modern design optimized for smartphones.

# Configuration Files
- ```CVapp/app/src/main/AndroidManifest.xml ```
  
Declares essential permissions for the Camera and Storage usage. 

- ```CVapp/app/src/main/res/values/strings.xml```
  
Contains the application naming.

# Features and Usage

In the settings the user can choose to toggle Filter Mode.
In Filter Mode the user can select specific allergens to avoid, the application will warn the user if any of those allergens are detected.
Standard Mode is the default, it lists all the allergens detected in the image.

To improve detection of smaller items (such as almonds, blueberries or dates) the application implements a Hybrid Filtering with specific lower confidence thresholds and geometry logic to analyze the shape and aspect ratio of those detected items, in order to filter out noise or unrealistic detections that don’t match up the usual properties of the food item.    


