# Stereo-Camera-Calibration-with-OpenCV-Detailed-Guide-
##Prepare left right image of chase
The initial steps involve preparing the left and right camera images with the chessboard pattern. the pattern should be clear and on a flat plane.
##Import Necessary Libraries:
```python
import cv2
import glob
import numpy as np
```
OpenCV (cv2): Provides computer vision functionalities, including chessboard corner detection and stereo calibration.
Glob (glob): Facilitates searching for files matching a specific pattern (used here to find left and right camera image paths).
NumPy (numpy): Offers efficient numerical operations and array manipulation (used for creating pattern point coordinates).
