# CVL_Assignment02
Christiano Jose Intoro - 24/546891/PA/23217

***

# Laundry Pile Segmentation Project
## Project Overview
This project implements a manual Computer Vision pipeline to detect and segment a laundry pile from a floor background. The solution avoids high-level CV libraries, relying instead on fundamental mathematical operations to achieve precise segmentation. 

## Technical Pipeline
The processing flow follows a standard Computer Vision pipeline as outlined in study materials:

### 1. Pre-processing (Image Enhancement)
The raw image is prepared to reduce complexity and noise.
* **Grayscale Conversion:** Reduces RGB data to a single intensity channel using the Luminosity formula:
    * **Formula:** $Gray = 0.299R + 0.587G + 0.114B$
* **Box Blur (Smoothing):** A 3x3 spatial filter is applied to remove "salt-and-pepper" noise and floor textures.
    * **Concept:** Each pixel is recalculated as the average of its 8 neighbors to smooth high-frequency intensity changes.

### 2. Segmentation (Binarization)
This stage isolates the laundry pile from the background.
* **Otsu’s Thresholding:** An automatic data-dependent method that finds the optimal threshold $T$.
    * **Concept:** It maximizes the **Between-class Variance** ($\sigma^2_B$) to find the point that best separates the foreground and background peaks in the image histogram.
* **Auto-Polarity Logic:** The system samples corner pixels (representative of the floor) to determine if the laundry is darker or lighter than the background, ensuring the mask is correctly oriented.

### 3. Post-processing (Morphological Refining)
The raw binary mask is refined to create a coherent object shape.
* **Morphological Closing:** A sequence of **Dilation** followed by **Erosion** using a 3x3 kernel.
    * **Dilation:** Expands the white areas to fill internal holes.
    * **Erosion:** Shrinks the boundaries back to remove small, isolated noise specs.
    * **Result:** A smooth, solid "blob" representing the laundry pile.

## Evaluation Metrics
The algorithm's performance is measured using standard detection evaluation formulas:

1.  **Pixel Accuracy:** * **Formula:** $\frac{Correct\ Pixels}{Total\ Pixels}$
    * **Result:** 99.91%
2.  **Intersection over Union (IoU):** * **Formula:** $IoU = \frac{Area\ of\ Overlap}{Area\ of\ Union}$
    * **Result:** 0.9908
    * **Significance:** An IoU > 0.5 is typically considered a successful detection; a score of 0.99 indicates near-perfect alignment with the ground truth.



## Usage
1.  **Demo Mode:** Set `MODE = "DEMO"` to run the pipeline on synthetic data and verify mathematical accuracy.
2.  **Real Mode:** Set `MODE = "REAL"` and provide the `MY_PHOTO_PATH` to process your uploaded laundry image.

## Summary of Findings
The high IoU score of 0.9908 demonstrates that deterministic pixel-based methods (Otsu's thresholding and Morphological cleaning) are highly effective for segmentation tasks where there is a distinct intensity difference between the object of interest and the background environment.

***
