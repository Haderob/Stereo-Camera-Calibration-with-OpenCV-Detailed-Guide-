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
- Make sure you have the same number of left and right images.
