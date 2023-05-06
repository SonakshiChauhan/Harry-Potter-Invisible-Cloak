# Harry-Potter-Invisible-Cloak

https://user-images.githubusercontent.com/91408631/236614385-ae89ba0d-4cf1-4d97-b1c7-c018fde3c349.mp4

**Overview:**
We have tried to create Harry Potter's invisible cloak usinh Color Detection and Masking.

->Step #1: Detect the presence of a colored object using computer vision techniques.
->Step #2: Create a mask by capturing frame
->Step #3: Apply the create mask of background on the detected object.
The outcome is a shown in the video above.

**Completed By Sonakshi Chauhan**

**Data:** Either a video having object movements or your webcam for real-time detection

**Deliverables:** Object Detection and movement Tracking

# Topics Covered:
-Open CV
-Color detection and masking

## Installation and Usage

Ensure that the following packages have been installed and imported.

```bash
pip install numpy
pip install pandas
pip install imutils
pip install opencv-python
```
## Notebook Structure
The structure of this notebook is as follows:

 -Import
 -Frame Capture & Background Capture
 -Upper And Lower Boundary defination of color and background
 -Mask Creation
 -Morphological tranformation
 -Final Mask Combination
 
#Imports:
  ```bash
import cv2
import numpy as np
import time
```
Above we have imported the necessary libraries

#Frame Capture & background Capture
```bash
cap = cv2.VideoCapture(0)
time.sleep(2)
background=0
```
->Above capture the frame of the video and store it in a variable named "cap"
->Then we halt the execution of the current thread for 2 sec
->During the initial 2 seconds we let the bacground be captured then we enter the frame

```bash
for i in range(50):
    ret,background=cap.read()
```
->Above we capture the background which we will use for masking

#Mask Creation:
```bash
while(cap.isOpened()):
    ret,img=cap.read()
    if not ret:
        break
    hsv=cv2.cvtColor(img,cv2.COLOR_BGR2HSV)
    
    #Setting values for cloth and making masks
    lower_red = np.array([0,120,70])
    upper_red = np.array([10,255,255]) #value for red
    mask1=cv2.inRange(hsv,lower_red,upper_red)
    lower_red = np.array([170,120,70])
    upper_red =  np.array([180,255,255])
    mask2=cv2.inRange(hsv,lower_red,upper_red)

    #combiing mask and storing it in default mask
    mask1=mask1+mask2
```
->We convert the image which is being detected till the camera is opened and convert it to HSV format
->Then we create the mask for color red (the color of the cloth we are taking as cloak) by taking lower_red and upper_red values in bgr format and converting them in hsv format and store the mask created
->We apply a similar step as above for background mask creation

#Morphological Transformation
```bash
    close_mask = cv2.morphologyEx(mask1, cv2.MORPH_CLOSE, np.ones((7,7),np.uint8))
    mask1=cv2.morphologyEx(close_mask,cv2.MORPH_OPEN,np.ones((3,3),np.uint8),iterations=2)
    mask2=cv2.morphologyEx(mask1,cv2.MORPH_DILATE,np.ones((3,3),np.uint8),iterations=1)
    mask2=cv2.bitwise_not(mask1)
```
->Firstly we apply morphological transformations and negate the mask1 and store it as second

#combining masks
```bash
    res1= cv2.bitwise_and(background,background,mask=mask1)
    res2=cv2.bitwise_and(img,img,mask=mask2)
    final_output = cv2.addWeighted(res1,1,res2,1,0)
    cv2.imshow('Invisible Cloak',final_output)
```
-> This is the final step where we combine both masks and our cloak is ready
 
#Conclusion -> We built Harry Potter's Cloak

Contact: sonakshichauhan1402@gmail.com

Project Continuity
This project is complete

Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

