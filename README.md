# Histogram Equalization Using OpenCV (Grayscale & Color Images)

---
## NAME: CJ ROHIT
## REG.NO: 212224243005

## Aim

To write a Python program using OpenCV to perform histogram equalization on both grayscale and color images to enhance image contrast and brightness.

The program performs the following operations:

- Read and display a grayscale image  
- Plot histogram of the grayscale image  
- Apply histogram equalization on grayscale image  
- Read and display a color image  
- Plot histogram of B, G, R channels  
- Convert image to HSV color space  
- Apply histogram equalization on the Value (V) channel  
- Convert the enhanced image back to BGR format  
- Display original and enhanced images with histograms  

---

## Software Used

- Anaconda – Python 3.7  
- Jupyter Notebook / VS Code  
- OpenCV (`cv2`)  
- NumPy  
- Matplotlib  

---

## Algorithm

### Step 1:
Import the required libraries: OpenCV, NumPy, and Matplotlib.

### Step 2:
Read the image `parrot.jpg` in grayscale format.

### Step 3:
Display the grayscale image and plot its histogram.

### Step 4:
Apply histogram equalization using `cv2.equalizeHist()` to enhance contrast.

### Step 5:
Display original grayscale image, its histogram, enhanced image, and its histogram using a 2 × 2 grid.

### Step 6:
Read the same image in color format.

### Step 7:
Split the image into B, G, R channels and plot their histograms.

### Step 8:
Convert the image from BGR to HSV color space.

### Step 9:
Apply histogram equalization on the V (Value) channel.

### Step 10:
Merge the channels and convert the image back to BGR format.

### Step 11:
Display original color image, histogram, enhanced image, and enhanced histogram using a 2 × 2 grid.

---

## Program
import cv2
import numpy as np
import matplotlib.pyplot as plt

# ---------------------------------------------------------
# Read Images
# ---------------------------------------------------------

img = cv2.imread("CJPHO.png")
glass = cv2.imread("sunglass.jpg")

if img is None:
    print("Error: CJPHO.png not found")
    exit()

if glass is None:
    print("Error: sunglass.jpg not found")
    exit()

original = img.copy()

# ---------------------------------------------------------
# Resize Sunglasses
# ---------------------------------------------------------

glass_h, glass_w = glass.shape[:2]

new_width = 215
new_height = int(glass_h * new_width / glass_w)

glass = cv2.resize(
    glass,
    (new_width, new_height)
)

# ---------------------------------------------------------
# Rotate Sunglasses according to face angle
# ---------------------------------------------------------

angle = -8

center = (
    new_width // 2,
    new_height // 2
)

rotation_matrix = cv2.getRotationMatrix2D(
    center,
    angle,
    1.0
)

rotated_glass = cv2.warpAffine(
    glass,
    rotation_matrix,
    (new_width, new_height),
    borderValue=(255, 255, 255)
)

# ---------------------------------------------------------
# Create Mask
# ---------------------------------------------------------

gray_glass = cv2.cvtColor(
    rotated_glass,
    cv2.COLOR_BGR2GRAY
)

_, mask = cv2.threshold(
    gray_glass,
    190,
    255,
    cv2.THRESH_BINARY_INV
)

kernel = np.ones(
    (3, 3),
    np.uint8
)

mask = cv2.morphologyEx(
    mask,
    cv2.MORPH_OPEN,
    kernel
)

mask = cv2.morphologyEx(
    mask,
    cv2.MORPH_CLOSE,
    kernel
)

mask_inv = cv2.bitwise_not(mask)

# ---------------------------------------------------------
# PRECISE POSITION FOR YOUR PHOTO
# ---------------------------------------------------------

x = 330
y = 165

# ---------------------------------------------------------
# Region of Interest
# ---------------------------------------------------------

roi = img[
    y:y + new_height,
    x:x + new_width
]

# ---------------------------------------------------------
# Remove sunglasses background
# ---------------------------------------------------------

background = cv2.bitwise_and(
    roi,
    roi,
    mask=mask_inv
)

# ---------------------------------------------------------
# Extract sunglasses
# ---------------------------------------------------------

foreground = cv2.bitwise_and(
    rotated_glass,
    rotated_glass,
    mask=mask
)

# ---------------------------------------------------------
# Combine
# ---------------------------------------------------------

result = cv2.add(
    background,
    foreground
)

# ---------------------------------------------------------
# Place sunglasses
# ---------------------------------------------------------

img[
    y:y + new_height,
    x:x + new_width
] = result

# ---------------------------------------------------------
# Display Output
# ---------------------------------------------------------

plt.figure(figsize=(14, 7))

plt.subplot(1, 2, 1)

plt.imshow(
    cv2.cvtColor(
        original,
        cv2.COLOR_BGR2RGB
    )
)

plt.title("Original Image")
plt.axis("off")

plt.subplot(1, 2, 2)

plt.imshow(
    cv2.cvtColor(
        img,
        cv2.COLOR_BGR2RGB
    )
)

plt.title("Sunglasses Fitted on Eyes")
plt.axis("off")

plt.tight_layout()
plt.show()

# ---------------------------------------------------------
# Save Output
# ---------------------------------------------------------

cv2.imwrite(
    "CJPHO_with_sunglasses.jpg",
    img
)

print("Sunglasses fitted successfully!")
print("Output saved as CJPHO_with_sunglasses.jpg")

---

##  Output



<img width="827" height="446" alt="image" src="https://github.com/user-attachments/assets/3af2e93b-6351-47c2-874b-4af276320a95" />








## Result

Thus, histogram equalization is successfully performed on both grayscale and color images using OpenCV. The contrast and brightness of the images are significantly improved, enhancing visual quality and feature visibility.
# Histogram-Equalization-Using-OpenCV1
