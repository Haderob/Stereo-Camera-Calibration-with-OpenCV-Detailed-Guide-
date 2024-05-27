# Stereo-Camera-Calibration-with-OpenCV-Detailed-Guide on colab-
## Prepare left right image of chase
The initial steps involve preparing the left and right camera images with the chessboard pattern. the pattern should be clear and on a flat plane. For my case I used 9x6 chessboard
![9x6 chessboard]([image_url](Image/6_x_9_pattern_chessboard.png))
## Import Necessary Libraries:
```python
import cv2
import glob
import numpy as np
```
**OpenCV (cv2)**: Provides computer vision functionalities, including chessboard corner detection and stereo calibration.
**Glob (glob)**: Facilitates searching for files matching a specific pattern (used here to find left and right camera image paths).
**NumPy (numpy)**: Offers efficient numerical operations and array manipulation (used for creating pattern point coordinates).
## Mount Google Drive (if using Colab):
```python
from google.colab import drive
drive.mount('/content/drive')
```
If you have the images on google drive
## Define Chessboard Pattern Size:
``` pyhton
PATTERN_SIZE = (6, 9)  # Adjust to your chessboard dimensions (rows, columns)
```
Describe the dimensions of your chessboard (number of corners in each row and column).

## Load Left and Right Camera Images:
``` python
left_imgs = list(sorted(glob.glob('/content/drive/MyDrive/Colab-Notebooks/clibration/L*.png')))
right_imgs = list(sorted(glob.glob('/content/drive/MyDrive/Colab-Notebooks/clibration/R*.png')))
print(len(left_imgs))
print(len(right_imgs))
assert len(left_imgs) == len(right_imgs)
```
- ```glob``` is used to find image paths with a specific prefix (L* for left, R* for right) and sort them to ensure proper image pair processing.
- Make sure you have the same number of left and right images. If you got an assertion error check your images if there is blured image or partially covered chessboard. The assertion verifies that you have the same number of left and right images, which is crucial for stereo calibration.
## Set Termination Criteria for Corner Refinement:
```python
criteria = (cv2.TERM_CRITERIA_EPS + cv2.TERM_CRITERIA_MAX_ITER, 30, 1e-3)
```
The line ```criteria = (cv2.TERM_CRITERIA_EPS + cv2.TERM_CRITERIA_MAX_ITER, 30, 1e-3)``` defines a set of termination criteria used by the cv2.cornerSubPix function in OpenCV. This function refines the initially detected chessboard corners in your images to provide more accurate sub-pixel locations.


```python
left_pts, right_pts = [], []
img_size = None

for left_img_path, right_img_path in zip(left_imgs, right_imgs):
    left_img = cv2.imread(left_img_path, cv2.IMREAD_GRAYSCALE)
    right_img = cv2.imread(right_img_path, cv2.IMREAD_GRAYSCALE)
    if img_size is None:
        img_size = (left_img.shape[1], left_img.shape[0])

    # Detect chessboard corners
    res_left, corners_left = cv2.findChessboardCorners(left_img, PATTERN_SIZE)
    res_right, corners_right = cv2.findChessboardCorners(right_img, PATTERN_SIZE)

    # Handle successful corner detection
    if res_left:
        corners_left = cv2.cornerSubPix(left_img, corners_left, (10, 10), (-1, -1), criteria)
    else:
        print("Failed to find chessboard corners in left image!")
        print(left_img_path)

    # Handle successful corner detection
    if res_right:
        corners_right = cv2.cornerSubPix(right_img, corners_right, (10, 10), (-1, -1), criteria)
    else:
        print("Failed to find chessboard corners in right image!")
        print(right_img_path)

    left_pts.append(corners_left)
    right_pts.append(corners_right)

```

-This loop iterates through each image pair in the sorted lists ```left_imgs``` and ```right_imgs```.
-It loads the left and right images as grayscale using ```cv2.imread``` (grayscale is often preferred for corner detection).
-The loop keeps track of the image size (```img_size```) in case it's needed later.
-The core part here is ```cv2.findChessboardCorners```. This function attempts to detect the chessboard pattern in each image using the specified ```PATTERN_SIZE```.
-The if statements handle successful and unsuccessful corner detections. The successful case calls cv2.cornerSubPix to refine the detected corner positions for better accuracy.
-Finally, the detected corners for each image pair (left and right) are appended to the ```left_pts``` and ```right_pts lists```, respectively.

