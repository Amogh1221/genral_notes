# 🎓 COMPUTER VISION & PATTERN RECOGNITION - COMPLETE EXAM BOOTCAMP
**Subject Code: AI3202 | University: Manipal University, Jaipur**  
**Target: 70-80 Marks | Duration: 1 Day Intensive | Knowledge Level: ZERO → EXPERT**

---

## 📚 COMPLETE TABLE OF CONTENTS

1. [MODULE 1: INTRODUCTION TO COMPUTER VISION](#module-1-introduction-to-computer-vision)
2. [MODULE 2: IMAGE FORMATION & TRANSFORMATION](#module-2-image-formation--transformation)
3. [MODULE 3: SAMPLING & QUANTIZATION](#module-3-sampling--quantization)
4. [MODULE 4: GEOMETRIC TRANSFORMATIONS](#module-4-geometric-transformations)
5. [MODULE 5: INTENSITY TRANSFORMATIONS](#module-5-intensity-transformations)
6. [MODULE 6: SPATIAL FILTERING](#module-6-spatial-filtering)
7. [MODULE 7: HISTOGRAM PROCESSING](#module-7-histogram-processing)
8. [MODULE 8: FREQUENCY DOMAIN FILTERING](#module-8-frequency-domain-filtering)
9. [MODULE 9: EDGE DETECTION](#module-9-edge-detection)
10. [MODULE 10: FEATURE DETECTION & DESCRIPTORS](#module-10-feature-detection--descriptors)
11. [MODULE 11: IMAGE SEGMENTATION](#module-11-image-segmentation)
12. [MODULE 12: ADVANCED ALGORITHMS](#module-12-advanced-algorithms)
13. [MODULE 13: DIMENSIONALITY REDUCTION](#module-13-dimensionality-reduction)
14. [QUICK REVISION SUMMARIES](#quick-revision-summaries)
15. [EXAM PREPARATION STRATEGY](#exam-preparation-strategy)

---

# MODULE 1: INTRODUCTION TO COMPUTER VISION

## WHAT IS COMPUTER VISION? ⭐

### **Simple Explanation (Beginner Level)**

Imagine you have eyes and a brain. Your eyes see things around you, and your brain understands what those things are—like recognizing your friend's face, reading a book, or knowing if a fruit is ripe.

**Computer Vision is exactly the same!** It's teaching computers to:
- **See** images like human eyes do
- **Understand** what's in those images like the brain does
- **Make decisions** based on what they see

**Real-life example:** When you unlock your phone with your face, that's computer vision working!

### **Technical Explanation (Exam-Ready)**

**Computer Vision** is a field of artificial intelligence that enables computers to interpret and understand visual information from digital images and videos, performing tasks such as:
- Object recognition and detection
- Image classification
- Scene understanding
- 3D reconstruction
- Motion estimation

### **Key Formula/Concept**

```
Computer Vision Pipeline:
INPUT (Image) → PROCESSING → INTERPRETATION → OUTPUT (Decision/Action)
         ↓              ↓              ↓
    Camera/Sensor   Algorithms    Classification/Recognition
```

### **Revision Point**
- CV = Teaching computers to see, understand, and decide like humans
- Three steps: Input image → Process → Output decision

### **Can Be Asked in Exam**

**2-Mark Questions:**
1. Define Computer Vision
2. What is the goal of computer vision?
3. Name 3 applications of computer vision

**5-Mark Questions:**
1. Explain the computer vision pipeline with an example
2. Difference between computer vision and image processing
3. How does computer vision differ from human vision?

**10-Mark Questions:**
1. Explain the complete process of how computer vision works from image acquisition to decision making
2. Discuss different applications of computer vision in real world

---

## DIFFERENCE: CV vs Image Processing vs Computer Graphics ⭐

### **Computer Vision (CV)**
- **Input:** Image
- **Output:** Interpretation/Understanding
- **Goal:** Extract meaningful information
- **Example:** Face recognition system
- **Question:** "What is this object?"

### **Image Processing (IP)**
- **Input:** Image
- **Output:** Processed Image
- **Goal:** Improve or extract features
- **Example:** Making a dark image brighter
- **Question:** "How can I enhance this image?"

### **Computer Graphics (CG)**
- **Input:** Scene description/3D model
- **Output:** Image
- **Goal:** Generate visual content
- **Example:** Creating a 3D movie scene
- **Question:** "How do I create this image?"

### **Comparison Table**

| Aspect | CV | IP | CG |
|--------|----|----|-----|
| Input | Image | Image | Scene/Model |
| Output | Interpretation | Enhanced Image | Image |
| Direction | Analysis | Enhancement | Synthesis |
| Example | Detect cars | Blur reduction | Game graphics |

### **Exam Questions**

**2-Mark:**
1. What is the main difference between CV and Image Processing?
2. Input and output of Computer Graphics?

**5-Mark:**
1. Compare Computer Vision, Image Processing, and Computer Graphics with examples

---

## RELATED DISCIPLINES & INTERDISCIPLINARY CONNECTIONS

### **1. Machine Learning & Deep Learning**
- CV uses ML for training recognition models
- Deep neural networks (CNNs) for image classification
- Example: Training a model to recognize cats and dogs

### **2. Image Processing**
- Preprocessing for CV tasks
- Filtering, edge detection, noise reduction
- Foundation for many CV algorithms

### **3. Computer Graphics**
- Rendering for creating synthetic training data
- 3D visualization of CV results
- Understanding camera models

### **4. Pattern Recognition**
- Feature extraction and matching
- Classification of patterns in images
- Recognition of objects and shapes

### **5. Cognitive Science**
- Understanding how human vision works
- Designing human-like vision systems
- Attention mechanisms in neural networks

### **6. Algorithms & Mathematics**
- Matrix operations for image processing
- Optimization for model training
- Linear algebra for transformations

### **Revision Point**
- CV is interdisciplinary, combining ML, IP, Graphics, and Cognitive Science
- Each field contributes unique tools and techniques

---

## APPLICATIONS OF COMPUTER VISION ⭐⭐⭐

### **Medical Imaging**
- Detecting tumors in X-rays and MRI scans
- Analyzing histology images
- Disease diagnosis assistance
- **Impact:** Life-saving early detection

### **Autonomous Vehicles**
- Road sign recognition
- Pedestrian detection
- Lane detection
- Obstacle avoidance
- **Impact:** Safe self-driving cars

### **Facial Recognition**
- Face detection and verification
- Emotion recognition
- Biometric authentication
- **Impact:** Security and convenience

### **Surveillance & Security**
- Motion detection
- Intrusion alerts
- Crowd monitoring
- **Impact:** Public safety

### **Retail & E-Commerce**
- Product recognition
- Inventory management
- Virtual try-on
- **Impact:** Improved shopping experience

### **Agricultural Monitoring**
- Crop disease detection
- Yield prediction
- Resource optimization
- **Impact:** Better farming

### **Sports Analytics**
- Player tracking
- Performance analysis
- Instant replays
- **Impact:** Enhanced sports experience

### **Robotics**
- Navigation and obstacle avoidance
- Manipulation and grasping
- Scene understanding
- **Impact:** Autonomous robots

### **Exam Questions**

**2-Mark:**
1. Name 5 applications of computer vision
2. How is CV used in medical imaging?

**5-Mark:**
1. Explain how computer vision is used in autonomous vehicles
2. Discuss applications of CV in 3 different domains

---

# MODULE 2: IMAGE FORMATION & TRANSFORMATION ⭐⭐⭐

## WHAT IS A DIGITAL IMAGE?

### **Simple Explanation**

Think of a photograph printed on paper. If you look at it very closely with a magnifying glass, you'll see it's made of tiny colored dots called pixels.

A **digital image** is the same! It's a picture stored in a computer as:
- A grid of tiny picture elements (pixels)
- Each pixel has a brightness/color value
- These values are numbers (0-255 for grayscale)

**Real example:** When you take a photo with your phone, it stores it as millions of pixels arranged in a grid!

### **Technical Definition**

A **digital image** is a 2D array of quantized intensity values arranged in rows and columns, where:
- Each element represents a **pixel** (picture element)
- Each pixel has discrete integer values
- Common formats: 8-bit (0-255), 16-bit, 32-bit

**Mathematical Representation:**
```
f(x, y) = Intensity at position (x, y)
where x ∈ [0, M-1], y ∈ [0, N-1]
M = number of rows, N = number of columns
```

### **Image as a Matrix**

```
        y (columns)
    0   1   2   ... N-1
0  [f₀₀ f₀₁ f₀₂ ... f₀ₙ]
1  [f₁₀ f₁₁ f₁₂ ... f₁ₙ]
x  [f₂₀ f₂₁ f₂₂ ... f₂ₙ]
(rows) ...
M-1[fₘ₀ fₘ₁ fₘ₂ ... fₘₙ]
```

### **Key Concepts**

**Resolution:**
- **PPI (Pixels Per Inch):** How many pixels in one inch
- Higher PPI = sharper, more detailed image
- Typical: 72 PPI (screen), 300 PPI (print)

**Bit Depth:**
- 8-bit: 256 gray levels (0-255)
- 24-bit: 16.7 million colors (8 bits each for R, G, B)

### **Revision Point**
- Digital image = 2D array of quantized pixel intensities
- Each pixel = discrete integer value
- Resolution = pixel density (PPI)

### **Exam Questions**

**2-Mark:**
1. What is a pixel? 
2. What is image resolution?
3. Define digital image
4. What is 8-bit image?

**5-Mark:**
1. Explain how an image is represented as a matrix
2. What determines image quality?

---

## IMAGE ACQUISITION & FORMATION ⭐⭐⭐

### **Simple Explanation**

How does a camera turn a real-world scene into a digital image? Let's trace the journey:

1. **Light from objects** bounces around the environment
2. **Lens focuses** this light onto a sensor
3. **Sensor captures** the light and converts it to electrical signals
4. **Analog-to-Digital Converter** changes signals to numbers
5. **Computer stores** these numbers as an image file

Like a movie camera, but capturing a still frame!

### **Technical Process**

The image acquisition pipeline:

```
Real-World Scene
    ↓
[Light Source] → [Object] → [Light Reflection]
    ↓
[Camera Lens] → [Image Plane] → [Sensor]
    ↓
[Analog Image] (continuous voltage)
    ↓
[Sampling & Quantization]
    ↓
[Digital Image] (discrete values)
    ↓
[Storage/Processing]
```

### **Key Steps**

**1. Image Formation**
- Light reflects from objects
- Lens focuses light onto image plane
- Creates an optical image (analog, continuous)

**2. Image Acquisition**
- Sensor converts light to electrical signal
- Creates analog image (continuous in space and intensity)

**3. Image Digitization**
- **Sampling:** Discretize in spatial domain
- **Quantization:** Discretize in intensity domain

### **Camera Model**

The **Pinhole Camera Model:**

```
         Object Point P(X,Y,Z)
                ↓
         [Lens/Pinhole]
                ↓
         Image Point p(x,y)
         
Mathematical relationship:
x/X = y/Y = f/Z
where f = focal length
```

### **Revision Point**
- Image acquisition: Light → Sensor → Analog → Digital
- Three stages: Formation → Acquisition → Digitization
- Sampling and quantization essential for digital images

### **Exam Questions**

**2-Mark:**
1. What is analog image?
2. What is image formation?
3. Define image acquisition

**5-Mark:**
1. Explain the image acquisition process with diagram
2. Difference between analog and digital image
3. Explain pinhole camera model

**10-Mark:**
1. Describe complete image formation process from light source to digital storage
2. Discuss sampling and quantization in image acquisition

---

# MODULE 3: SAMPLING & QUANTIZATION ⭐⭐⭐

## SAMPLING: SPATIAL DISCRETIZATION

### **Simple Explanation (Beginner)**

Imagine an analog image is like a continuous sheet of paper with smooth color gradations. If you want to store it in a computer, you can't store an infinite amount of information.

**Sampling** is like placing a grid over the image and only recording the color at the center of each grid cell:

```
Analog Image → Grid placed → Sample at grid points → Sampled Image
```

If the grid is fine (small cells), you get more detail. If it's coarse (large cells), you lose detail but save space.

### **Technical Definition**

**Sampling** is the process of converting a continuous spatial domain signal into a discrete spatial domain signal by recording values at regular intervals.

**Sampling Theorem (Nyquist-Shannon):**
- To avoid aliasing, the sampling rate must be **at least 2× the highest frequency** in the image
- Minimum sampling rate = 2 × bandwidth
- Violating this causes aliasing (false patterns)

### **Aliasing**

**What is it?** When you don't sample frequently enough, high-frequency details appear as false low-frequency patterns.

**Example:** A checkerboard pattern sampled too coarsely appears as stripes or moiré patterns.

### **Sampling Interval**

```
If domain is [0, L]:
  Number of samples = M
  Sampling interval Δx = L/M
  Sampling frequency = M/L = 1/Δx
```

### **Revision Point**
- Sampling = Converting continuous space to discrete grid
- Nyquist criterion: Sample rate ≥ 2 × max frequency
- Aliasing occurs from insufficient sampling
- Resolution determined by sampling rate

### **Exam Questions**

**2-Mark:**
1. What is sampling?
2. State Nyquist theorem
3. What is aliasing?
4. How to prevent aliasing?

**5-Mark:**
1. Explain sampling with an example
2. Discuss Nyquist sampling theorem
3. What happens if sampling rate is too low?

**10-Mark:**
1. Explain aliasing in detail with examples
2. Discuss sampling theorem and its importance

---

## QUANTIZATION: INTENSITY DISCRETIZATION

### **Simple Explanation**

Now imagine the sampled image still has continuous intensity values (like 45.735 brightness level).

**Quantization** is rounding these continuous values to discrete levels:

```
Continuous intensity values
    ↓
Round to nearest discrete level
    ↓
Store as integer (0-255 for 8-bit)
```

**Trade-off:** More quantization levels = more colors but more storage. Fewer levels = less storage but visible banding.

### **Technical Definition**

**Quantization** is the process of converting continuous or high-precision intensity values into discrete or low-precision values.

**8-bit Quantization Example:**
- Continuous range: [0, 1] or [0, 255]
- Discrete levels: 0, 1, 2, ..., 255 (256 levels)
- Each continuous value maps to nearest level

### **Quantization Levels**

```
Bit Depth | Levels | Range
----------|--------|-------
1-bit     | 2      | {0, 1}
4-bit     | 16     | {0...15}
8-bit     | 256    | {0...255}
16-bit    | 65536  | {0...65535}
```

### **Quantization Error**

When rounding continuous value to discrete level, we introduce error:

```
Error = |Original Value - Quantized Value|
Maximum Error = 0.5 × (Step Size)
```

### **Revision Point**
- Quantization = Rounding intensity to discrete levels
- 8-bit image: 256 levels (0-255)
- More bits = better quality but more storage
- Quantization error is unavoidable

### **Exam Questions**

**2-Mark:**
1. What is quantization?
2. What is 8-bit image?
3. How many gray levels in 8-bit image?

**5-Mark:**
1. Explain quantization with example
2. Discuss bit depth and its effect on image quality
3. What is quantization error?

---

## COMPLETE SAMPLING & QUANTIZATION PROCESS

### **Step-by-Step Process**

```
Analog Image (continuous in space and intensity)
    ↓
SAMPLING (Spatial discretization)
Grid placed at regular intervals → Select discrete points
    ↓
Sampled Image (discrete space, continuous intensity)
    ↓
QUANTIZATION (Intensity discretization)
Round continuous values to discrete levels
    ↓
Digital Image (discrete space, discrete intensity)
    ↓
Stored as 2D array of integers
```

### **Example: 2×2 Sampling & Quantization**

```
Analog: [45.6]  [78.3]
        [123.7] [156.2]

↓ (Quantize to nearest integer)

Digital: [46]  [78]
         [124] [156]

Store as:
46   78
124  156
```

### **Exam Questions**

**5-Mark:**
1. Explain sampling and quantization with example
2. Why are both sampling and quantization necessary?

**10-Mark:**
1. Describe complete sampling and quantization process with diagram
2. Discuss effects of under-sampling and under-quantization

---

## COORDINATE SYSTEMS & NEIGHBORHOODS

### **Image Coordinate System**

**Standard convention:**
- Origin (0,0) at top-left
- x-axis points right (columns)
- y-axis points down (rows)
- Pixel at position (x, y) has value f(x, y)

### **Pixel Neighborhoods**

**4-Connected Neighborhood (N₄):**
- Only horizontal and vertical neighbors
- Formula: N₄(p) = {(x, y-1), (x, y+1), (x-1, y), (x+1, y)}
- **Used for:** Diagonal-resistant connectivity

**8-Connected Neighborhood (N₈):**
- All adjacent pixels (including diagonal)
- Formula: N₈(p) = all 8 surrounding pixels
- **Used for:** Complete connectivity

```
4-Connected:       8-Connected:
    [ ] [2] [ ]        [1] [2] [3]
    [1] [P] [3]    =   [4] [P] [5]
    [ ] [4] [ ]        [6] [7] [8]
```

### **Distance Metrics**

**1. Euclidean Distance** (actual straight-line distance)
```
dE[(i,j), (k,l)] = √[(i-k)² + (j-l)²]
Examples: (0,0) to (1,1) = √2 ≈ 1.41
         (0,0) to (1,0) = 1
```

**2. City Block/Manhattan Distance (L₁)**
```
dL1[(i,j), (k,l)] = |i-k| + |j-l|
Examples: (0,0) to (1,1) = 2
         (0,0) to (1,0) = 1
Uses: Movement on grid (like city blocks)
```

**3. Chessboard/Chebyshev Distance (L∞)**
```
d8[(i,j), (k,l)] = max{|i-k|, |j-l|}
Examples: (0,0) to (1,1) = 1
         (0,0) to (1,2) = 2
Uses: Chess king movement
```

### **Distance Comparison Diagram**

```
All points at distance 1 from center P:

Euclidean (circle):    City Block (diamond):  Chessboard (square):
      [·]                   [·]                  [·][·][·]
    [·] P [·]            [·] P [·]          =   [·] P [·]
      [·]                   [·]                  [·][·][·]
```

### **Exam Questions**

**2-Mark:**
1. What is 4-connected neighborhood?
2. What is 8-connected neighborhood?
3. Define Euclidean distance
4. Define City Block distance

**5-Mark:**
1. Explain different distance metrics with examples
2. Difference between 4 and 8 connectivity
3. When to use which distance metric?

---

# MODULE 4: GEOMETRIC TRANSFORMATIONS ⭐⭐

## IMAGE TRANSFORMATIONS OVERVIEW

### **Simple Explanation**

Imagine you have a photograph and want to:
- Move it to a different position
- Rotate it
- Make it bigger or smaller
- Flip it

These are **geometric transformations**. They change WHERE pixels are, not WHAT color they are.

### **Two Steps in Geometric Transformation**

```
1. SPATIAL TRANSFORMATION
   Define where each pixel goes
   (x, y) → (x', y')

2. INTENSITY INTERPOLATION
   Assign intensity values to new positions
   (find which old pixels contribute)
```

### **Key Concepts**

**Forward vs Backward Mapping:**
- **Forward:** Old position → New position (gaps may occur)
- **Backward:** For each new pixel, find which old pixels to use (no gaps)
- **Better:** Backward mapping is preferred

---

## HOMOGENEOUS COORDINATES ⭐

### **Why Homogeneous Coordinates?**

**Problem:** 2D translations can't be expressed as matrix multiplication!
```
Translation: x' = x + tx, y' = y + ty
Not linear! Can't use matrix form [A][P] = [P']
```

**Solution:** Add a third coordinate w = 1!

### **Homogeneous Coordinate Conversion**

**Cartesian → Homogeneous:**
```
(x, y) → (x, y, 1)
```

**Homogeneous → Cartesian:**
```
(x, y, w) → (x/w, y/w, 1)
```

**Examples:**
```
(2, 3) → (2, 3, 1)
(4, 6, 2) → (2, 3, 1)  [Same point!]
(6, 9, 3) → (2, 3, 1)  [Same point!]
```

**Key Property:** Multiple homogeneous coordinates represent the same point!

### **Advantages**

- All transformations (translation, rotation, scaling, shearing) as single matrix multiply
- Can combine multiple transformations easily
- Projective geometry becomes linear algebra

---

## AFFINE TRANSFORMATIONS ⭐⭐

### **Definition**

An **affine transformation** is a linear transformation plus translation:

```
[x']   [a₁₁ a₁₂ a₁₃] [x]
[y'] = [a₂₁ a₂₂ a₂₃] [y]
[1 ]   [0   0   1   ] [1]
```

This can handle: **Translation, Rotation, Scaling, Shearing**

### **General Form**

```
x' = a₁₁x + a₁₂y + a₁₃
y' = a₂₁x + a₂₂y + a₂₃
```

---

## SPECIFIC TRANSFORMATIONS

### **1. TRANSLATION**

**Purpose:** Move image by (tx, ty)

**Equations:**
```
x' = x + tx
y' = y + ty
```

**Matrix Form:**
```
[x']   [1  0  tx] [x]
[y'] = [0  1  ty] [y]
[1 ]   [0  0  1 ] [1]
```

**Example:** Translate by (5, 3)
```
(0,0) → (5,3)
(10,10) → (15,13)
```

### **2. ROTATION** (about origin)

**Purpose:** Rotate by angle θ (counterclockwise)

**Equations:**
```
x' = x cos(θ) - y sin(θ)
y' = x sin(θ) + y cos(θ)
```

**Matrix Form:**
```
[x']   [cos(θ)  -sin(θ)  0] [x]
[y'] = [sin(θ)   cos(θ)  0] [y]
[1 ]   [0        0       1] [1]
```

**Example:** Rotate 90° counterclockwise
```
θ = 90° → cos(90°)=0, sin(90°)=1
(1,0) → (0,1)
(0,1) → (-1,0)
```

### **3. SCALING**

**Purpose:** Scale by factors (sx, sy)

**Equations:**
```
x' = sx × x
y' = sy × y
```

**Matrix Form:**
```
[x']   [sx  0   0] [x]
[y'] = [0   sy  0] [y]
[1 ]   [0   0   1] [1]
```

**Example:** Scale by 2 in both directions
```
(1,1) → (2,2)
(10,10) → (20,20)
```

**Special Cases:**
- sx = sy: Uniform scaling (maintains aspect ratio)
- sx ≠ sy: Non-uniform scaling (changes shape)
- sx = -1: Horizontal flip
- sy = -1: Vertical flip

### **4. SHEARING**

**Purpose:** Slant image (like parallelogram)

**Vertical Shear:**
```
x' = x + sh×y
y' = y

Matrix: [1   sh  0]
        [0   1   0]
        [0   0   1]
```

**Horizontal Shear:**
```
x' = x
y' = sh×x + y

Matrix: [1   0   0]
        [sh  1   0]
        [0   0   1]
```

### **5. REFLECTION**

**About x-axis:**
```
Matrix: [1   0   0]
        [0  -1   0]
        [0   0   1]

Effect: (x,y) → (x,-y)
```

**About y-axis:**
```
Matrix: [-1  0   0]
        [0   1   0]
        [0   0   1]

Effect: (x,y) → (-x,y)
```

---

## COMBINING TRANSFORMATIONS

### **Composite Transformation**

Apply multiple transformations in sequence:

```
P' = T1 × T2 × T3 × P

Example: Rotate then Translate
[x']   [1  0  tx] [cos(θ)  -sin(θ)  0] [x]
[y'] = [0  1  ty] [sin(θ)   cos(θ)  0] [y]
[1 ]   [0  0  1 ] [0        0       1] [1]
```

**Important:** Order matters! Rotation then translation ≠ Translation then rotation

### **Inverse Transformation**

To undo a transformation, use its inverse matrix:

```
Original: P' = T × P
Inverse:  P = T⁻¹ × P'
```

---

## ROTATION ABOUT ARBITRARY CENTER

### **Problem:** Rotation about origin moves the image. How to rotate about center?

### **Solution:**
1. Translate center to origin
2. Rotate
3. Translate back

**Matrix:**
```
T_final = T(cx,cy) × R(θ) × T(-cx,-cy)

where (cx,cy) = center of rotation
```

---

## PYTHON IMPLEMENTATION EXAMPLE

```python
import cv2
import numpy as np

# Load image
img = cv2.imread('image.jpg')
height, width = img.shape[:2]

# Translation Matrix (shift by 50, 30)
T_translate = np.array([[1, 0, 50],
                        [0, 1, 30]], dtype=np.float32)
translated = cv2.warpAffine(img, T_translate, (width, height))

# Rotation Matrix (45 degrees about center)
center = (width/2, height/2)
angle = 45
scale = 1.0
M_rotate = cv2.getRotationMatrix2D(center, angle, scale)
rotated = cv2.warpAffine(img, M_rotate, (width, height))

# Scaling Matrix (2x larger)
M_scale = np.array([[2, 0, 0],
                    [0, 2, 0]], dtype=np.float32)
scaled = cv2.warpAffine(img, M_scale, (width*2, height*2))
```

---

## EXAM QUESTIONS

### **2-Mark Questions**
1. What is geometric transformation?
2. Define affine transformation
3. What is translation matrix?
4. How to rotate image 90°?
5. What is homogeneous coordinate?

### **5-Mark Questions**
1. Explain translation with matrix form
2. Derive rotation matrix for angle θ
3. Difference between scaling and rotation
4. How to combine multiple transformations?
5. Explain scaling with uniform and non-uniform examples

### **10-Mark Questions**
1. Describe all types of affine transformations with matrices
2. How to rotate image about arbitrary center? Derive complete solution
3. Explain homogeneous coordinates and their advantage in geometric transformations
4. Discuss forward and backward mapping in geometric transformations

---

# MODULE 5: INTENSITY TRANSFORMATIONS ⭐⭐⭐

## POINT OPERATIONS (INTENSITY TRANSFORMATIONS)

### **Simple Explanation**

Imagine each pixel in your image has a brightness value (0=black, 255=white).

An **intensity transformation** changes these brightness values:
```
Old brightness → Processing → New brightness
    (input)                      (output)
```

It's called a "point operation" because each pixel is processed individually, without looking at neighbors.

**Examples:**
- Make image brighter
- Make image darker  
- Increase contrast
- Create negative

### **Technical Definition**

**Intensity transformation** (or gray-level transformation):
```
s = T(r)

where:
r = input intensity level [0, L-1]
s = output intensity level [0, L-1]
T = transformation function
L = total intensity levels (256 for 8-bit)
```

**Key Property:** Each input intensity maps to exactly one output intensity (many-to-one or one-to-one).

---

## TYPES OF INTENSITY TRANSFORMATIONS

### **1. LINEAR TRANSFORMATIONS**

#### **Identity Transformation**
```
s = r
(No change to image)
```

#### **Negative/Inverse**
```
s = L - 1 - r
where L = 256 for 8-bit

Example: r=0 → s=255 (black→white)
         r=255 → s=0 (white→black)
```

**Use:** Enhancing white/gray details embedded in dark regions

**Graph:**
```
s (output)
255 |     *
    |    *
    |   *
    |  *
    | *
  0 |*_____
    0     255 r (input)
```

---

### **2. LOGARITHMIC TRANSFORMATIONS**

**Formula:**
```
s = c × log(1 + r)

where c = scaling constant
```

**Properties:**
- Maps narrow range of dark values → wide range of outputs
- Maps wide range of bright values → narrow range of outputs
- **Compresses** high-valued pixels
- **Expands** low-valued pixels

**Use Cases:**
- Displaying Fourier spectrum (values 0 to 10⁶)
- Enhancing dark images
- Reducing dynamic range

**Example:**
```
r = [1,     10,    100,   1000]
s = [0,   2.3×c,  4.6×c,  6.9×c]

Notice: Big jump in input (1→1000) becomes smaller jump in output
```

**Graph:**
```
s  |     ___
   |   _/
255|  /
   | /
   |/___
 0 |     
   0___________255 r
     (Curved line showing compression of bright values)
```

---

### **3. POWER-LAW (GAMMA) TRANSFORMATION** ⭐⭐

**Formula:**
```
s = c × r^γ

where:
γ (gamma) = power coefficient
c = scaling constant
```

**Properties Depend on γ:**
- **γ < 1:** Brightens image (lifts dark values more)
- **γ = 1:** Linear (no change)
- **γ > 1:** Darkens image (compresses bright values)

**Use Cases:**
- Correcting camera non-linearities
- Adapting images for different displays
- General-purpose contrast manipulation

**Examples:**

```
γ = 0.4 (Brighten):
r = [0,    64,   128,   192,   255]
s = [0,   161,   220,   240,   255]
```

```
γ = 2.0 (Darken):
r = [0,    64,   128,   192,   255]
s = [0,    16,    64,   144,   255]
```

**Graphs:**

```
γ < 1 (Brightening):
s |     *
  |   *
  | *_____
0 |*
  0___________r

γ > 1 (Darkening):
s |        *
  |       *
  |_____*
0 |*
  0___________r
```

**Typical Values:**
- Monitors: γ = 2.2
- Correction: γ = 1/2.2 ≈ 0.45
- Dark images: γ = 0.3-0.5
- Bright images: γ = 1.5-2.0

---

## PIECEWISE LINEAR TRANSFORMATIONS ⭐

### **Concept**

Instead of single formula, divide intensity range into segments. Each segment has its own linear transformation.

**Advantages:**
- More flexible than single function
- Can implement complex transformations
- Practical control points (r₁,s₁), (r₂,s₂)

---

### **TYPE 1: CONTRAST STRETCHING**

**Problem:** Low-contrast images have most pixels in narrow range (e.g., 80-180)

**Solution:** Stretch this range to full scale (0-255)

**Using Control Points:**
- Define two control points: (r₁,s₁), (r₂,s₂)
- Below r₁: Linear mapping to 0
- Between r₁ and r₂: Stretch to (0, L-1)
- Above r₂: Linear mapping to L-1

**Equation (for r₁ ≤ r ≤ r₂):**
```
s = s₁ + (r - r₁) × [(s₂ - s₁)/(r₂ - r₁)]
```

**Example:**
```
Original image: most pixels in [80, 180]
Control points: (80, 0), (180, 255)

For r = 80: s = 0
For r = 130: s = 0 + (130-80) × (255/100) = 127.5
For r = 180: s = 255
```

**Effect:** Stretches narrow range to full range, increasing contrast

---

### **TYPE 2: THRESHOLDING**

**Definition:** Convert grayscale to binary (only black and white)

**Formula:**
```
s = 0,     if r < k
s = L-1,   if r ≥ k

where k = threshold value
```

**Control Points:** (k, 0), (k, L-1) — vertical jump!

**Use Cases:**
- Document scanning
- Object detection
- Binary segmentation

**Example:**
```
Threshold at k = 128
r = [50, 120, 130, 200] → s = [0, 0, 255, 255]
```

---

### **TYPE 3: GRAY-LEVEL SLICING**

**Purpose:** Highlight specific intensity range, suppress others

**Two Approaches:**

**A) Bright appearance:**
```
s = L-1,   if A ≤ r ≤ B  (highlight range [A,B])
s = r,     otherwise      (keep others)
```

**B) Dark appearance:**
```
s = L-1,   if A ≤ r ≤ B  (highlight range [A,B])
s = 0,     otherwise      (suppress others)
```

**Use:** Isolating specific tissue in medical images, highlighting objects

**Example:**
Highlight pixels in range [100, 150]:
```
r = [50, 110, 125, 140, 200]
s = [50, 255, 255, 255, 200]  (bright approach)
s = [0,  255, 255, 255, 0]    (dark approach)
```

---

### **TYPE 4: BIT-PLANE SLICING** ⭐⭐

**Concept:** Every 8-bit intensity value can be written in binary:
```
255 = 11111111
127 = 01111111
64  = 01000000
1   = 00000001
```

**Bit-plane slicing:** Extract individual bit planes and display as separate binary images

**Bit Positions:**
```
Bit 8 (MSB): Most Significant Bit - contributes 128
Bit 7:                        contributes 64
Bit 6:                        contributes 32
...
Bit 1 (LSB): Least Significant Bit - contributes 1
```

**Process:**
1. Convert each pixel to 8-bit binary
2. Extract bit at position k for all pixels
3. Create binary image from these bits

**Example:**
```
Pixel value: 203 = 11001011 (binary)
Bit 8: 1
Bit 7: 1
Bit 6: 0
Bit 5: 0
Bit 4: 1
Bit 3: 0
Bit 2: 1
Bit 1: 1
```

**Significance:**
- Higher bit planes (8,7,6) contain major features
- Lower bit planes (3,2,1) contain minor details/noise
- **Use:** Image compression, watermarking, analysis

**Python Implementation:**
```python
# Extract bit plane k (0=LSB, 7=MSB)
bit_plane = (image >> k) & 1
```

---

## HISTOGRAM PROCESSING (PREVIEW) ⭐⭐⭐

### **What is a Histogram?**

**Histogram** = Graph showing how many pixels have each intensity level

```
Frequency
   |      *
   |    * * 
   |  * * *     
   | * * * *
   |_*_*_*_*_*_
   0  64  128 192 255 Intensity
```

**Read:** "There are many pixels at intensity 200, few pixels at intensity 50"

### **Histogram Equalization**

**Problem:** Image has low contrast because histogram is bunched up

**Solution:** Spread histogram to use all intensity levels — increases contrast!

**Result:** More uniform distribution of pixels across all intensities

**Will cover in detail in Module 7!**

---

## NUMERICAL EXAMPLES & PROBLEMS

### **Example 1: Image Negative**

**Given:** 4×4 grayscale image (8-bit)
```
Original Image f(r):
[100  110   90   95]
[98   140  145  135]
[89   90   88   87]
[102  105  99   101]
```

**Find:** Negative image using s = 255 - r

**Solution:**
```
Step 1: Apply formula s = 255 - r to each pixel
For f(0,0) = 100: s = 255 - 100 = 155
For f(0,1) = 110: s = 255 - 110 = 145
... (continue for all pixels)

Negative Image g(s):
[155  145  165  160]
[157  115  110  120]
[166  165  167  168]
[153  150  156  154]
```

**Observation:** Dark pixels become bright, bright become dark

---

### **Example 2: Gamma Transformation**

**Given:** Image values [0, 64, 128, 192, 255], γ = 0.4, c = 1

**Find:** Transformed values using s = c × r^γ

**Solution:**
```
s = 1 × r^0.4

r = 0:   s = 0^0.4 = 0
r = 64:  s = 64^0.4 = 64^(2/5) ≈ 7.76 × 2 ≈ 15.5... 
Wait, let me recalculate:

64^0.4 = (2^6)^0.4 = 2^2.4 ≈ 5.28  (No, this is wrong)

Actually:
64^0.4 = exp(0.4 × ln(64)) = exp(0.4 × 4.16) = exp(1.66) ≈ 5.27

Hmm, let me use standard approach:
If range is normalized to [0,1]:

Normalized r = [0, 0.25, 0.5, 0.75, 1.0]
s = r^0.4 = [0, 0.25^0.4, 0.5^0.4, 0.75^0.4, 1.0]
         = [0, 0.644, 0.758, 0.889, 1.0]

De-normalized to [0, 255]:
s = [0, 164, 193, 227, 255]

Result:
r = [0,   64,  128,  192,  255]
s = [0,  164,  193,  227,  255]
```

**Observation:** Low values get boosted more (0.4 < 1), brightening image

---

### **Example 3: Contrast Stretching**

**Given:** Image with pixels mostly in range [80, 180]
Control points: (80, 0), (180, 255)
Find transformed values for r = [80, 100, 140, 180, 200]

**Solution:**
```
Transformation equation (for 80 ≤ r ≤ 180):
s = 0 + (r - 80) × [(255 - 0)/(180 - 80)]
s = (r - 80) × 2.55

For r < 80: s = 0
For r > 180: s = 255

Calculating:
r = 80:   s = (80-80) × 2.55 = 0
r = 100:  s = (100-80) × 2.55 = 20 × 2.55 = 51
r = 140:  s = (140-80) × 2.55 = 60 × 2.55 = 153
r = 180:  s = (180-80) × 2.55 = 100 × 2.55 = 255
r = 200:  s = 255 (above 180)

Results:
r = [80,  100,  140,  180,  200]
s = [0,   51,   153,  255,  255]
```

**Observation:** Narrow range [80, 180] stretched to [0, 255] — increases contrast!

---

### **Example 4: Thresholding**

**Given:** Grayscale image, threshold k = 128
```
Original:
[100  120  130]
[90   128  150]
[110  115  140]
```

**Find:** Binary image using threshold

**Solution:**
```
Rule: if r < 128 → s = 0 (black)
      if r ≥ 128 → s = 255 (white)

Applying to each pixel:
100 < 128   → 0
120 < 128   → 0
130 ≥ 128   → 255
90 < 128    → 0
128 ≥ 128   → 255
150 ≥ 128   → 255
110 < 128   → 0
115 < 128   → 0
140 ≥ 128   → 255

Binary Image:
[0    0    255]
[0    255  255]
[0    0    255]
```

---

### **Example 5: Bit-Plane Slicing**

**Given:** 3×3 image with values (4-bit for simplicity)
```
Image:
[5   10   3]
[7   15   9]
[2   4    14]
```

**Find:** Each bit plane

**Solution:**
```
Step 1: Convert to binary (4-bit)
5 = 0101
10 = 1010
3 = 0011
7 = 0111
15 = 1111
9 = 1001
2 = 0010
4 = 0100
14 = 1110

Step 2: Extract each bit plane
Bit 4 (MSB):
[0  1  0]
[0  1  1]
[0  0  1]

Bit 3:
[1  0  0]
[1  1  0]
[0  0  1]

Bit 2:
[0  1  0]
[1  1  0]
[1  1  1]

Bit 1 (LSB):
[1  0  1]
[1  1  1]
[0  0  0]

Observations:
- Bit 4: Few 1s (high-value pixels)
- Bit 1: Many 1s scattered (noise/details)
```

---

## EXAM QUESTIONS

### **2-Mark Questions**
1. Define intensity transformation
2. What is image negative?
3. What is gamma transformation?
4. What is thresholding?
5. What is contrast stretching?
6. What is histogram?
7. What is bit-plane slicing?

### **5-Mark Questions**
1. Explain logarithmic transformation with applications
2. Derive formula for contrast stretching
3. Explain gray-level slicing with example
4. Compare power-law and logarithmic transformations
5. What is bit-plane slicing? Why is it useful?
6. Solve: Apply image negative to given 2×2 image

### **10-Mark Questions**
1. Explain all types of piecewise linear transformations with diagrams
2. Derive gamma transformation and discuss effects of γ > 1, γ = 1, γ < 1
3. Given an image histogram, design contrast stretching transformation
4. Discuss histogram equalization concept and its advantages
5. Explain bit-plane slicing and its applications in image processing

---

# MODULE 6: SPATIAL FILTERING ⭐⭐⭐

## SPATIAL FILTERING CONCEPT

### **Simple Explanation**

Imagine you're looking at a noisy image. You want to smooth it out.

**Spatial filtering** is like:
1. Looking at a small window (3×3, 5×5, etc.) around each pixel
2. Doing some calculation using that window
3. Replacing the center pixel with the result
4. Moving to the next pixel
5. Repeating

It's called **filtering** because you're filtering out unwanted features (like noise).

### **Key Concept: The Kernel (Filter Mask)**

A **kernel** is a small matrix of weights:

```
Kernel (3×3):
[w₁₁  w₁₂  w₁₃]
[w₂₁  w₂₂  w₂₃]
[w₃₁  w₃₂  w₃₃]
```

The kernel slides over the image, and at each position:
1. Multiply kernel values by image pixel values
2. Sum all products
3. Assign this sum to output pixel

---

## LINEAR vs NON-LINEAR FILTERING

### **Linear Filters**

**Definition:** Output is a weighted sum of input pixel values

**Formula:**
```
g(x,y) = Σ Σ w(s,t) × f(x+s, y+t)
         s t

where:
w(s,t) = kernel weights
f(x+s, y+t) = input image pixels
g(x,y) = output pixel
```

**Examples:**
- Mean/Box filter (average neighbors)
- Gaussian filter (weighted average)
- Sobel filter (edge detection)

**Properties:**
- Superposition: filter(a×i1 + b×i2) = a×filter(i1) + b×filter(i2)
- Easy to implement with convolution

### **Non-Linear Filters**

**Definition:** Output is NOT a weighted sum

**Examples:**
- Median filter (middle value of neighbors)
- Morphological filters (erosion, dilation)
- Bilateral filter (edge-preserving)

**Properties:**
- No superposition
- Can preserve edges better
- More computationally expensive

---

## CONVOLUTION OPERATION ⭐⭐

### **Definition**

**Convolution** is the fundamental operation in spatial filtering:

```
g(x,y) = f(x,y) ⊗ h(x,y)

where:
⊗ = convolution operator
f = input image
h = kernel (filter)
g = output image
```

### **Mathematical Formula**

```
g(x,y) = Σ Σ f(m,n) × h(x-m, y-n)
         m n
```

Or equivalently (used in practice):

```
g(x,y) = Σ Σ w(s,t) × f(x+s, y+t)
         s  t
```

**Note:** Second form is actually correlation, but commonly called convolution in image processing

### **Step-by-Step Process**

```
1. Place kernel at position (x,y)
2. Multiply each kernel element by corresponding image element
3. Sum all products
4. Place result at (x,y) in output
5. Move kernel to next position
6. Repeat for all positions
```

### **Visual Example**

```
Input Image:        Kernel:         Computation at (1,1):
[10 20 30]         [1 0 -1]        
[40 50 60]   ⊗     [2 0 -2]    = 10×1 + 20×0 + 30×(-1)
[70 80 90]         [1 0 -1]        + 40×2 + 50×0 + 60×(-2)
                                    + 70×1 + 80×0 + 90×(-1)
                                   = 10 + 0 - 30 + 80 + 0 - 120 + 70 + 0 - 90
                                   = -80
```

---

## LINEAR FILTERS FOR SMOOTHING ⭐⭐

### **PURPOSE OF SMOOTHING**
- Remove noise
- Blur image
- Pre-processing for other operations

### **1. MEAN (BOX) FILTER**

**Concept:** Replace each pixel with average of neighborhood

**Kernel (3×3):**
```
(1/9) × [1 1 1]
        [1 1 1]
        [1 1 1]
```

**Formula:**
```
g(x,y) = (1/mn) × Σ Σ f(x+s, y+t)
                   s t

where m×n = kernel size
```

**Effect:**
- Smooths image
- Removes fine details
- Blurs edges
- Reduces noise

**Advantages:**
- Simple and fast
- Reduces noise

**Disadvantages:**
- Blurs edges significantly
- Not realistic blur model
- Creates ringing artifacts

**Python Code:**
```python
import cv2
import numpy as np

img = cv2.imread('image.jpg')
kernel = np.ones((5,5), np.float32)/25
blurred = cv2.filter2D(img, -1, kernel)
```

---

### **2. WEIGHTED AVERAGE (GAUSSIAN) FILTER** ⭐⭐

**Concept:** Weights decrease with distance from center

**Kernel (3×3):**
```
(1/16) × [1 2 1]
         [2 4 2]
         [1 2 1]

Center has weight 4, edges have weight 1
```

**Properties:**
- Center pixel weighted most
- Smoother, more natural blur
- Matches Gaussian blur of real lenses
- Better edge preservation than mean filter

**Gaussian Distribution:**

```
g(x,y) = (1/(2πσ²)) × e^(-(x²+y²)/(2σ²))

where σ = standard deviation
Larger σ = wider, softer blur
```

**Effect:**
- More realistic smoothing
- Better edge preservation
- Smoother gradations

**Python Code:**
```python
blurred = cv2.GaussianBlur(img, (5, 5), 1.0)
# (5,5) = kernel size
# 1.0 = standard deviation
```

---

### **3. MEDIAN FILTER**

**Concept:** Replace pixel with median value of neighborhood (non-linear!)

**Process:**
1. Collect all pixel values in neighborhood
2. Sort them
3. Take middle value
4. Use as output

**Example (3×3):**
```
Neighborhood: [10, 45, 23, 67, 89, 12, 34, 56, 78]
Sorted:       [10, 12, 23, 34, 45, 56, 67, 78, 89]
Median:       45 (middle value)
```

**Advantages:**
- Preserves edges well
- Removes salt-and-pepper noise effectively
- Non-linear (more powerful)

**Disadvantages:**
- Slower than linear filters
- Can remove fine details
- Computational expensive

**Python Code:**
```python
filtered = cv2.medianBlur(img, 5)
# 5 = neighborhood size
```

---

## EDGE DETECTION FILTERS ⭐⭐⭐

### **WHAT ARE EDGES?**

An **edge** is a boundary where pixel intensity changes sharply.

**Types:**
- Step edge: Abrupt change
- Ramp edge: Gradual change
- Roof edge: Peak/valley

### **GRADIENT CONCEPT**

**Gradient:** Vector showing direction and magnitude of maximum intensity change

```
∇f = [∂f/∂x, ∂f/∂y]

Magnitude: |∇f| = √((∂f/∂x)² + (∂f/∂y)²)
Direction: θ = atan2(∂f/∂y, ∂f/∂x)
```

### **SOBEL FILTER** ⭐⭐

**Purpose:** Detect edges using gradient approximation

**Kernel Gx (horizontal edges):**
```
[-1 0 1]
[-2 0 2]
[-1 0 1]

Detects vertical edges
```

**Kernel Gy (vertical edges):**
```
[-1 -2 -1]
[0  0  0]
[1  2  1]

Detects horizontal edges
```

**Process:**
1. Compute gx = image ⊗ Gx
2. Compute gy = image ⊗ Gy
3. Edge magnitude = √(gx² + gy²)
4. Edge direction = atan2(gy, gx)

**Example:**
```
Original 3×3:
[100  150  100]
[100  180  100]
[100  150  100]

Gx convolution:
(-100) + 0 + 100 + (-200) + 0 + 200 + (-100) + 0 + 100 = 0

Gy convolution:
(-100) + (-200) + (-100) + 0 + 0 + 0 + 100 + 200 + 100 = 0

Edge magnitude = √(0² + 0²) = 0 (uniform region, no edge)
```

**Python Code:**
```python
sobelx = cv2.Sobel(img, cv2.CV_64F, 1, 0, ksize=3)
sobely = cv2.Sobel(img, cv2.CV_64F, 0, 1, ksize=3)
magnitude = np.sqrt(sobelx**2 + sobely**2)
```

---

### **PREWITT FILTER**

Similar to Sobel but with different weights:

```
Gx:              Gy:
[-1 0 1]         [-1 -1 -1]
[-1 0 1]    or   [0  0  0]
[-1 0 1]         [1  1  1]
```

---

### **LAPLACIAN FILTER** ⭐

**Purpose:** Detect edges using second derivative

**Kernel:**
```
[0  1  0]
[1 -4  1]
[0  1  0]

or for diagonal neighbors:

[-1 -1 -1]
[-1  8 -1]
[-1 -1 -1]
```

**Formula:**
```
∇²f = ∂²f/∂x² + ∂²f/∂y²
```

**Properties:**
- Detects all edges
- Sensitive to noise
- Fast computation
- Often used with Gaussian (LoG)

**Python Code:**
```python
laplacian = cv2.Laplacian(img, cv2.CV_64F)
```

---

## NUMERICAL EXAMPLE

### **Example: Applying Mean Filter**

**Given:**
```
Image f (4×4):
[10  20  30  40]
[15  25  35  45]
[12  22  32  42]
[18  28  38  48]

Kernel h (3×3):
(1/9) × [1 1 1]
        [1 1 1]
        [1 1 1]
```

**Find:** Output image g (apply mean filter)

**Solution:**

Position (1,1) - center on value 25:
```
Neighborhood:     Multiplication:
[10 20 30]    ×   [(1/9)×10  (1/9)×20  (1/9)×30]
[15 25 35]        [(1/9)×15  (1/9)×25  (1/9)×35]
[12 22 32]        [(1/9)×12  (1/9)×22  (1/9)×32]

Sum = (1/9) × (10+20+30+15+25+35+12+22+32)
    = (1/9) × 201
    = 22.33

g(1,1) = 22.33 ≈ 22
```

Position (1,2) - center on value 35:
```
Neighborhood:
[20 30 40]
[25 35 45]
[22 32 42]

Sum = (1/9) × (20+30+40+25+35+45+22+32+42)
    = (1/9) × 291
    = 32.33

g(1,2) = 32
```

**Complete Output Image:**
```
(Computing all positions similarly)

g ≈ [14  22  28  32]
    [18  24  31  37]
    [16  22  28  33]
    [18  24  30  35]
```

**Observation:** Values are smoother, details blurred

---

## EXAM QUESTIONS

### **2-Mark Questions**
1. What is spatial filtering?
2. What is a kernel/mask?
3. Difference between linear and non-linear filtering?
4. What is edge?
5. What is gradient?

### **5-Mark Questions**
1. Explain convolution operation with example
2. Discuss mean filter and its properties
3. Explain Sobel edge detection
4. Difference between Sobel and Laplacian
5. Apply 3×3 mean filter to given 4×4 image

### **10-Mark Questions**
1. Derive convolution formula and explain step-by-step
2. Compare mean, Gaussian, and median filters
3. Explain edge detection concept and methods
4. Discuss properties of different edge detection filters
5. Given an image and kernel, compute convolution result

---

# MODULE 7: HISTOGRAM PROCESSING ⭐⭐⭐

## HISTOGRAM CONCEPT ⭐⭐

### **Simple Explanation**

A **histogram** shows how many pixels have each brightness level.

**Imagine:**
- X-axis: Intensity levels (0=black, 255=white)
- Y-axis: Number of pixels at that level
- Each bar's height = count of pixels at that intensity

**Example:**
```
Histogram of dark image:
Frequency
   |*
   |* *
   |* * *
   |* * * *
   |_*_*_*_*_*_________________
   0    50   100  150  200  250  Intensity
   
Most pixels are dark (left side has tall bars)
Few bright pixels (right side is empty)
```

### **Technical Definition**

**Histogram h(rₖ):**
```
h(rₖ) = nₖ

where:
rₖ = k-th intensity level
nₖ = number of pixels with intensity rₖ
k = 0, 1, 2, ..., L-1
L = total levels (256 for 8-bit)
```

**Normalized Histogram (PDF):**
```
p(rₖ) = nₖ / N

where N = total number of pixels
```

### **Reading a Histogram**

```
Histogram Analysis:

[Tall bars on left] → Dark image
[Tall bars on right] → Bright image
[Bars spread evenly] → High contrast
[Bars bunched together] → Low contrast
[Gaps in bars] → Missing intensity levels
```

---

## HISTOGRAM EQUALIZATION ⭐⭐⭐

### **THE PROBLEM**

Low-contrast images have histograms where pixels are bunched in a narrow range.

**Example:**
```
Low-contrast image histogram:
Frequency
   |    
   |      ***
   |      ***
   |      ***
   |___***_____________________
   0   100-150  255
   
All pixels are middle-gray, no pure blacks or whites
Result: Image looks washed out, hard to see details
```

### **THE SOLUTION: HISTOGRAM EQUALIZATION**

**Goal:** Spread histogram to use all intensity levels

**Effect:** Increase contrast, make image more visible

### **Histogram Equalization Formula** ⭐⭐

The key formula:

```
sk = round((L-1) × Σ(nj/N)) for j=0 to k
    = round((L-1) × CDF(rk))

where:
sk = output intensity for input rk
L = total levels (256)
nj/N = normalized histogram
CDF = cumulative distribution function
```

**Interpretation:**
```
For each intensity level k:
1. Sum up all pixels at that level and below (CDF)
2. Scale to range [0, L-1]
3. Round to nearest integer
4. Use as new intensity
```

### **Cumulative Distribution Function (CDF)**

**CDF shows:** "What fraction of pixels have intensity ≤ k?"

```
CDF(k) = Σ p(j) for j=0 to k

where p(j) = probability of intensity j
```

**Example:**
```
Original histogram:
Intensity: 0   100  100  100  200  200
Count:     1   2    2    2    1    1
Total pixels = 9

Probabilities:
p(0)=1/9, p(100)=4/9, p(200)=2/9

CDF:
CDF(0) = 1/9 ≈ 0.111
CDF(100) = 1/9 + 4/9 = 5/9 ≈ 0.556
CDF(200) = 1/9 + 4/9 + 2/9 = 7/9 ≈ 0.778
```

---

## STEP-BY-STEP HISTOGRAM EQUALIZATION

### **Algorithm**

```
Input: Original image with L intensity levels

Step 1: Compute histogram h(rk)
        Count pixels at each level

Step 2: Compute normalized histogram p(rk)
        p(rk) = h(rk) / N

Step 3: Compute CDF (cumulative sum)
        C(rk) = Σ p(rj) for j=0 to k

Step 4: Compute transformation
        sk = round((L-1) × C(rk))

Step 5: Apply transformation
        For each pixel rk in input:
        Replace with sk

Output: Enhanced image with stretched histogram
```

### **Numerical Example**

**Given:**
```
4×4 image (16 pixels):
[0   50   50   50]
[100 100  100  100]
[100 100  200  200]
[200 200  200  200]

L = 256, N = 16
```

**Step 1: Histogram**
```
Intensity 0:   1 pixel
Intensity 50:  3 pixels
Intensity 100: 5 pixels
Intensity 200: 7 pixels
```

**Step 2: Normalized histogram p(r)**
```
p(0) = 1/16 = 0.0625
p(50) = 3/16 = 0.1875
p(100) = 5/16 = 0.3125
p(200) = 7/16 = 0.4375
```

**Step 3: CDF**
```
C(0) = 0.0625
C(50) = 0.0625 + 0.1875 = 0.25
C(100) = 0.25 + 0.3125 = 0.5625
C(200) = 0.5625 + 0.4375 = 1.0
```

**Step 4: Transformation (with L=256)**
```
s0 = round(255 × 0.0625) = round(15.94) = 16
s50 = round(255 × 0.25) = round(63.75) = 64
s100 = round(255 × 0.5625) = round(143.44) = 143
s200 = round(255 × 1.0) = 255
```

**Mapping:**
```
0 → 16
50 → 64
100 → 143
200 → 255
```

**Step 5: Apply transformation**
```
Original:                  Equalized:
[0   50   50   50]        [16  64   64   64]
[100 100  100  100]   →   [143 143  143  143]
[100 100  200  200]       [143 143  255  255]
[200 200  200  200]       [255 255  255  255]

New histogram: More spread out across [0-255]
```

---

## EFFECTS OF HISTOGRAM EQUALIZATION

### **Before Equalization:**
- Histogram bunched in narrow range
- Low contrast
- Washed out appearance
- Hard to see details

### **After Equalization:**
- Histogram spread across full range
- High contrast
- Vivid appearance
- Details become visible

### **Important Properties:**

1. **Expands** narrow ranges of intensity
2. **Compresses** wide ranges of intensity
3. **Always** increases contrast (or keeps same)
4. **Can create** false edges/artifacts in flat regions
5. **Works best** for images with poor contrast

### **Histogram Equalization Formula (Simplified)**

Sometimes written as:

```
sk = round((L-1) / N × Σ h(rj)) for j=0 to k
```

Which is equivalent to the CDF formula.

---

## ADAPTIVE HISTOGRAM EQUALIZATION

### **Problem with Standard Equalization**
- Enhances entire image uniformly
- Can over-enhance some regions
- May not work well for all regions

### **Solution: Adaptive Equalization**
- Divide image into blocks
- Apply histogram equalization to each block
- Smoother results, prevents over-enhancement
- Better local contrast enhancement

### **Python Code**

```python
import cv2
import numpy as np

img = cv2.imread('image.jpg', 0)  # Grayscale

# Standard equalization
equalized = cv2.equalizeHist(img)

# Adaptive equalization
clahe = cv2.createCLAHE(clipLimit=2.0, tileGridSize=(8,8))
adapted = clahe.apply(img)

cv2.imshow('Original', img)
cv2.imshow('Equalized', equalized)
cv2.imshow('Adaptive', adapted)
cv2.waitKey(0)
```

---

## HISTOGRAM MATCHING (Specification)

### **Concept**

Adjust histogram of one image to match histogram of reference image.

**Use Case:** Image enhancement, color correction

**Process:**
1. Compute histogram of source image
2. Compute histogram of reference image
3. Find transformation that matches source to reference
4. Apply transformation

---

## EXAM QUESTIONS

### **2-Mark Questions**
1. What is histogram?
2. What is histogram equalization?
3. Why use histogram equalization?
4. What is CDF?
5. What is adaptive equalization?

### **5-Mark Questions**
1. Explain histogram equalization with formula
2. Given image with histogram, find equalized histogram
3. Discuss effects of histogram equalization
4. Solve: Apply histogram equalization to 2×2 image
5. Compare standard and adaptive equalization

### **10-Mark Questions**
1. Derive histogram equalization formula and explain
2. Given image, compute complete histogram equalization step-by-step
3. Discuss advantages and disadvantages of histogram equalization
4. Explain CDF-based histogram equalization method
5. Design histogram equalization algorithm in pseudocode

---

# MODULE 8: FREQUENCY DOMAIN FILTERING ⭐⭐⭐

## FOURIER TRANSFORM BASICS ⭐⭐⭐

### **Simple Explanation**

Imagine a signal (like audio) that repeats a pattern. You can describe it two ways:

1. **Time domain:** How the signal changes over time
2. **Frequency domain:** What frequencies (pitches) make up the signal

**Fourier Transform** converts between these two representations!

**Visual Analogy:**
```
Musical sound can be described as:
- Time domain: Vibrating speaker membrane
- Frequency domain: Bass (low freq), Treble (high freq)

Image can be described as:
- Spatial domain: Pixel brightness values
- Frequency domain: How fast brightness changes
```

### **Why Frequency Domain?**

```
Spatial Domain Problems:
- Hard to filter globally
- Must process each pixel with neighbors

Frequency Domain Advantages:
- Multiplication instead of convolution
- Easy to identify noise frequencies
- Intuitive understanding of filters
- FFT makes it fast
```

### **Fourier Transform Formula**

**Continuous Fourier Transform:**
```
F(u,v) = ∫∫ f(x,y) × e^(-j2π(ux+vy)) dx dy

where:
f(x,y) = spatial domain image
F(u,v) = frequency domain representation
j = √-1 (imaginary unit)
u,v = frequency variables
```

**Discrete Fourier Transform (DFT) - for digital images:**
```
F(u,v) = Σ Σ f(x,y) × e^(-j2π(ux/M + vy/N))
         x=0 y=0

where:
M, N = image dimensions
u = 0,1,...,M-1; v = 0,1,...,N-1
```

### **Key Components**

**F(u,v)** is complex:
```
F(u,v) = |F(u,v)| × e^(j×phase)

|F(u,v)| = magnitude/amplitude (how strong that frequency)
phase = phase angle (location information)
```

**More useful forms:**
```
Magnitude spectrum: |F(u,v)| = √(Real² + Imaginary²)
Power spectrum: |F(u,v)|²
Phase spectrum: φ(u,v) = atan2(Imaginary, Real)
```

### **Inverse Fourier Transform**

Convert back to spatial domain:

```
f(x,y) = (1/MN) × Σ Σ F(u,v) × e^(j2π(ux/M + vy/N))
```

**Key Property:** Convolution in spatial domain = Multiplication in frequency domain

```
g(x,y) = f(x,y) ⊗ h(x,y)
 ↓
G(u,v) = F(u,v) × H(u,v)

This is much faster! [Convolution is slow]
```

---

## FREQUENCY DOMAIN INTERPRETATION ⭐⭐

### **What Do Frequencies Represent?**

In images:

```
Low frequencies: Gradual intensity changes (smooth regions)
High frequencies: Rapid intensity changes (edges, details, noise)
```

**Frequency domain visualization:**
```
Magnitude spectrum:
- Center (0,0) = Average intensity (DC component)
- Nearby = Low frequencies (smooth, general structure)
- Distant = High frequencies (details, edges, noise)
- Corners = Highest frequencies
```

### **2D Frequency Space**

```
       v (vertical frequency)
       ↑
       |      Far = high freq
       |  *      *      *
       |  
       | *  F(0,0)  *
       |  (lowest)
       |  *      *      *
   ----+----→ u (horizontal frequency)
    0
```

---

## FAST FOURIER TRANSFORM (FFT) ⭐

### **What is FFT?**

**FFT** is a fast algorithm to compute DFT.

```
Naive DFT: O(N²) operations
FFT: O(N log N) operations

1000-pixel signal:
DFT: 1,000,000 operations
FFT: 10,000 operations
100× faster!
```

### **FFT Requirements**

For efficiency, image dimensions should be powers of 2:
- Good: 256×256, 512×512, 1024×1024
- OK: Other sizes (zero-padded to power of 2)

### **Python Implementation**

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

# Read image
img = cv2.imread('image.jpg', 0)

# Compute FFT
f_transform = np.fft.fft2(img)

# Shift zero-frequency component to center
f_shift = np.fft.fftshift(f_transform)

# Compute magnitude spectrum
magnitude_spectrum = 20 * np.log1p(np.abs(f_shift))

# Compute phase spectrum
phase_spectrum = np.angle(f_shift)

# Inverse FFT to get back image
img_back = np.fft.ifft2(np.fft.ifftshift(f_shift))

# Display
plt.subplot(1,2,1)
plt.imshow(img, cmap='gray')
plt.title('Original Image')

plt.subplot(1,2,2)
plt.imshow(magnitude_spectrum, cmap='hot')
plt.title('Magnitude Spectrum')
plt.show()
```

---

## FREQUENCY DOMAIN FILTERING ⭐⭐

### **Key Insight**

```
Spatial Domain:                Frequency Domain:
Image ⊗ Kernel           →    Image × Filter

Slow convolution         →    Fast multiplication
```

### **Steps**

```
1. Compute FFT of image: F = FFT(image)
2. Create frequency filter: H
3. Multiply: G = F × H
4. Inverse FFT: output = IFFT(G)
```

### **Types of Filters**

**1. Ideal Low-Pass Filter**

Allows only low frequencies (smooth image):

```
H(u,v) = 1, if D(u,v) ≤ D₀
        = 0, otherwise

where D(u,v) = √(u² + v²) = distance from origin
D₀ = cutoff distance
```

**Effect:**
- Removes noise and details
- Smooth image
- Can create ringing artifacts

```
Magnitude spectrum:       After Low-Pass:
*    *    *              *              
*    *    *       →      * hollow *
*    *    *              *              
Remove outer components
```

**2. Ideal High-Pass Filter**

Allows only high frequencies (edge detection):

```
H(u,v) = 0, if D(u,v) ≤ D₀
        = 1, otherwise
```

**Effect:**
- Enhances edges
- Removes smooth regions
- Highlights details

**3. Butterworth Low-Pass Filter** ⭐

Smooth transition (no ringing):

```
H(u,v) = 1 / (1 + (D(u,v)/D₀)^(2n))

where n = filter order
Larger n = sharper transition
```

**4. Gaussian Low-Pass Filter** ⭐⭐

Smooth Gaussian transition:

```
H(u,v) = e^(-D²(u,v)/(2σ²))

where σ = standard deviation
```

**Advantage:** No ringing, natural appearance

**5. Notch Filter**

Block specific frequency (reject noise at specific frequency):

```
H(u,v) = 0, at specific (u₀,v₀)
       = 1, elsewhere
```

**Use:** Remove periodic noise (power line 50/60 Hz)

---

## CONVOLUTION THEOREM ⭐⭐

### **The Key Relationship**

```
Convolution in spatial domain = Multiplication in frequency domain

f(x,y) ⊗ h(x,y) ⟷ F(u,v) × H(u,v)
```

**Proof idea:** Convolution is integral of products. In frequency domain, this becomes simple multiplication!

### **Computational Benefit**

```
Spatial domain convolution:
- For each output pixel (x,y)
- Multiply all kernel values
- Sum products
- Total: O(M×N×m×n) for M×N image, m×n kernel

Frequency domain:
- FFT: O(MN log MN)
- Multiply: O(MN)
- IFFT: O(MN log MN)
- Total: O(MN log MN)

For large images, frequency domain is MUCH faster!
```

---

## PRACTICAL FILTERING EXAMPLE

### **Low-Pass Filtering to Remove Noise**

```python
import cv2
import numpy as np

# Read image
img = cv2.imread('noisy_image.jpg', 0)

# FFT
f_transform = np.fft.fft2(img)
f_shift = np.fft.fftshift(f_transform)

# Create Gaussian low-pass filter
rows, cols = img.shape
crow, ccol = rows//2, cols//2
sigma = 30

X = np.arange(cols)
Y = np.arange(rows)
X, Y = np.meshgrid(X - ccol, Y - crow)
D = np.sqrt(X**2 + Y**2)

H = np.exp(-D**2 / (2 * sigma**2))

# Apply filter
f_filtered = f_shift * H

# Inverse FFT
f_ishift = np.fft.ifftshift(f_filtered)
img_filtered = np.fft.ifft2(f_ishift)
img_filtered = np.abs(img_filtered)

# Display
cv2.imshow('Original', img)
cv2.imshow('Filtered', img_filtered.astype(np.uint8))
cv2.waitKey(0)
```

---

## EXAM QUESTIONS

### **2-Mark Questions**
1. What is Fourier Transform?
2. What is frequency domain?
3. What is FFT?
4. What do low frequencies represent?
5. What do high frequencies represent?
6. Advantage of frequency domain filtering?

### **5-Mark Questions**
1. Explain Fourier Transform with example
2. Explain convolution theorem
3. How to apply low-pass filter?
4. Compare spatial and frequency domain filtering
5. What is magnitude spectrum?

### **10-Mark Questions**
1. Derive DFT formula and explain components
2. Discuss different types of frequency domain filters
3. Explain why FFT is faster than spatial convolution
4. Design high-pass filter for edge detection
5. Given noise at specific frequency, design notch filter

---

# MODULE 9: EDGE DETECTION ⭐⭐⭐

## EDGE DETECTION OVERVIEW

### **What is an Edge?**

An **edge** is a boundary between regions of different intensity.

**Characteristics:**
- Sharp intensity change
- Marks object boundaries
- Contains important information
- Smaller representation than full image

**Types of Edges:**
```
Step edge:  Abrupt change
            I|
            I|___
            
Ramp edge:  Gradual change
            I  /
            I /
            I/___
            
Roof edge:  Peak/valley
            I /\
            I/__\
```

---

## GRADIENT-BASED EDGE DETECTION

### **1. FIRST DERIVATIVE (GRADIENT)**

**Concept:** Edges have large gradients (fast intensity change)

**1D example:**
```
Intensity:  0  0  0  100  100  100
Position:   0  1  2   3    4    5

First derivative (slope):
            0  0  100   0    0
Position:   0  1   2    3    4

Edge at position 2-3 (large derivative)!
```

**2D Gradient:**
```
∇f = [∂f/∂x, ∂f/∂y]

Magnitude: |∇f| = √((∂f/∂x)² + (∂f/∂y)²)
Direction: θ = atan2(∂f/∂y, ∂f/∂x)
```

**Approximate with Finite Differences:**
```
∂f/∂x ≈ f(x+1,y) - f(x,y)
∂f/∂y ≈ f(x,y+1) - f(x,y)
```

---

### **2. SOBEL EDGE DETECTOR** ⭐⭐

Already covered in Module 6, but recap:

**Kernels:**
```
Gx:              Gy:
[-1 0 1]         [-1 -2 -1]
[-2 0 2]   or    [0  0  0]
[-1 0 1]         [1  2  1]
```

**Process:**
```
1. gx = image ⊗ Gx
2. gy = image ⊗ Gy
3. magnitude = √(gx² + gy²)
4. direction = atan2(gy, gx)
5. Threshold magnitude to get binary edge map
```

**Advantages:**
- Simple and fast
- Works reasonably well
- Widely used

**Disadvantages:**
- Sensitive to noise
- Thick edges
- Sensitive to threshold selection

---

### **3. ROBERTS EDGE DETECTOR**

**Kernels (diagonal):**
```
Gx:         Gy:
[1  0]      [0  1]
[0 -1]      [-1 0]

Smaller than Sobel (2×2)
```

**Faster but less noise-robust than Sobel**

---

### **4. PREWITT EDGE DETECTOR**

**Kernels:**
```
Gx:              Gy:
[-1 0 1]         [-1 -1 -1]
[-1 0 1]         [0  0  0]
[-1 0 1]         [1  1  1]

Same as Sobel but uniform weights
```

---

## SECOND DERIVATIVE METHODS

### **LAPLACIAN EDGE DETECTOR** ⭐⭐

**Concept:** Use second derivative to find edges

**Second derivative has zero-crossing at edges!**

**Kernel:**
```
[0  1  0]
[1 -4  1]
[0  1  0]

Formula: ∇²f = ∂²f/∂x² + ∂²f/∂y²
```

**Properties:**
- Detects edge type (step, ramp)
- More sensitive to noise
- Fast and simple

---

### **LOG (LAPLACIAN OF GAUSSIAN)** ⭐⭐⭐

**Problem:** Laplacian is noise-sensitive

**Solution:** Smooth first with Gaussian, then apply Laplacian

**Two approaches:**
```
Method 1: LoG = ∇²(G ⊗ f) = (∇²G) ⊗ f
Method 2: LoG = (G ⊗ f) then apply Laplacian
```

**Effect:**
- Noise reduction from Gaussian smoothing
- Edge detection from Laplacian
- LoG filter as single kernel

**Formula:**
```
LoG(x,y) = -(1/(πσ⁴)) × [1 - (x²+y²)/(2σ²)] × e^(-(x²+y²)/(2σ²))

where σ = Gaussian standard deviation
```

**Properties:**
- Smooth multi-scale edge detection
- Better than Laplacian alone
- Good noise handling

---

### **DOG (DIFFERENCE OF GAUSSIAN)** ⭐⭐

**Concept:** Difference of two Gaussians approximates LoG

**Formula:**
```
DoG = G(σ₁) - G(σ₂)

where σ₁ < σ₂ (two different scales)
```

**Advantage:** Faster to compute than LoG

**Relation to LoG:**
```
DoG ≈ (σ₂ - σ₁) × LoG

Better approximation when σ₂/σ₁ ≈ 1.6
```

**Used in:** SIFT feature detection (Module 10)

---

## CANNY EDGE DETECTION ⭐⭐⭐

### **Overview**

**Canny** is a sophisticated multi-stage edge detector:

```
Steps:
1. Gaussian smoothing (noise reduction)
2. Gradient computation (find edge strength)
3. Non-maximum suppression (thin edges)
4. Double thresholding (weak/strong edges)
5. Edge tracking by hysteresis (connect weak edges)
```

---

### **Step 1: Gaussian Smoothing**

Reduce noise:
```
Smoothed = image ⊗ Gaussian(σ)

σ ≈ 1.4 typically
```

---

### **Step 2: Gradient Computation**

Compute magnitude and direction:
```
gx = smoothed ⊗ Gx
gy = smoothed ⊗ Gy
magnitude = √(gx² + gy²)
direction = atan2(gy, gx)
```

---

### **Step 3: Non-Maximum Suppression** ⭐

**Problem:** Edges are thick (multiple pixels wide)

**Solution:** Keep only local maxima in gradient direction

**Process:**
```
For each edge pixel:
  Look at two neighbors along gradient direction
  If not local maximum: suppress (set to 0)
  
Result: Thin edges (1 pixel wide)
```

**Why?** Thinning improves localization

---

### **Step 4: Double Thresholding**

Use two thresholds to classify edges:

```
If magnitude > T_high: strong edge
If magnitude < T_low: not edge
If T_low < magnitude < T_high: weak edge (undecided)

Typical ratio: T_high = 2 × T_low
Example: T_low = 50, T_high = 150
```

---

### **Step 5: Edge Tracking by Hysteresis** ⭐

**Idea:** Weak edges are edges if connected to strong edges

**Process:**
```
1. Start from strong edge pixels
2. Follow connected weak edges
3. Keep weak edges connected to strong edges
4. Discard isolated weak edges
```

**Result:** Connected edge map with no isolated noise

---

### **Canny Algorithm Summary**

```
Input: Image, σ, T_low, T_high

1. Smooth: g = image ⊗ Gaussian(σ)
2. Gradients: gx, gy from Sobel
3. Magnitude: M = √(gx² + gy²)
4. Direction: θ = atan2(gy, gx)
5. Non-max suppression: thin edges
6. Double thresholding: classify as strong/weak
7. Hysteresis: track weak edges from strong
8. Output: Binary edge map

Output: Binary image with edges (white) and non-edges (black)
```

### **Python Implementation**

```python
import cv2
import numpy as np

img = cv2.imread('image.jpg', 0)

# Canny edge detection
edges = cv2.Canny(img, 100, 200)
#                  image, low_threshold, high_threshold

cv2.imshow('Edges', edges)
cv2.waitKey(0)
```

---

### **Parameters**

**σ (Gaussian sigma):**
- Small σ: Sensitive to noise
- Large σ: Miss fine details
- Typical: 1-2

**T_low, T_high:**
- Too low: Noise detected as edges
- Too high: Faint edges missed
- Typical ratio: 1:2 or 1:3

---

## EDGE DETECTION COMPARISON

| Detector | Simple | Fast | Noise Robust | Localization | Used For |
|----------|--------|------|--------------|--------------|----------|
| Sobel | Yes | Yes | Poor | Good | Quick detection |
| Roberts | Yes | Yes | Poor | Good | Fast processing |
| Laplacian | Yes | Yes | Poor | Excellent | Floating point |
| LoG | No | Medium | Good | Excellent | Robust detection |
| Canny | No | Medium | Excellent | Excellent | Industry standard |

---

## EDGE DETECTION EXAMPLE

### **Visual Example: Simple Step Edge**

```
Original image:
100 100 100 100 100
100 100 100 100 100
100 200 200 200 100  ← Transition here
100 200 200 200 100
100 100 100 100 100

Sobel horizontal gradient:
0   0    0    0   0
0   0    0    0   0
0 100  100  100   0  ← High gradient at transition
0 100  100  100   0
0   0    0    0   0

After non-max suppression:
0   0    0    0   0
0   0    0    0   0
0   0 100   0    0  ← Thin edge
0   0 100   0    0
0   0    0    0   0

After thresholding (T=50):
0   0    0    0   0
0   0    0    0   0
0   1 100   0    0  ← Binary edges
0   1 100   0    0
0   0    0    0   0
```

---

## EXAM QUESTIONS

### **2-Mark Questions**
1. What is edge detection?
2. Difference between first and second derivative
3. What is gradient?
4. Name 5 edge detection methods
5. What is non-maximum suppression?

### **5-Mark Questions**
1. Explain Sobel edge detection
2. Explain Laplacian edge detection
3. Difference between LoG and DoG
4. Explain Canny algorithm steps
5. Why use Gaussian smoothing before edge detection?

### **10-Mark Questions**
1. Describe all edge detection methods with comparison
2. Derive Canny algorithm completely with explanations
3. Explain non-maximum suppression and hysteresis
4. Given an image, apply Canny and explain each step
5. Design edge detection system for medical imaging

---

# MODULE 10: FEATURE DETECTION & DESCRIPTORS ⭐⭐⭐

[Due to length constraints, I'll provide summary format for modules 10-13]

## BRIEF OVERVIEW

### **SIFT (Scale-Invariant Feature Transform)** ⭐⭐⭐
- Keypoint detection at multiple scales using DoG
- 128-dimensional descriptor
- Invariant to scale, rotation, translation
- Industry standard for feature matching
- Slow but accurate

### **SURF (Speeded-Up Robust Features)** ⭐⭐
- Faster alternative to SIFT
- Uses Hessian matrix
- 64-dimensional descriptor
- Similar invariances to SIFT
- Much faster

### **HOG (Histogram of Oriented Gradients)** ⭐⭐⭐
- Divides image into cells
- Computes gradient orientation histogram in each cell
- Used for object detection (pedestrian detection)
- 1764-D feature vector for 64×128 image
- Robust to edge appearance variations

### **Harris Corner Detector** ⭐⭐
- Detects corner keypoints
- Uses autocorrelation matrix
- Fast and efficient
- Less distinctive than SIFT

---

# MODULE 11: IMAGE SEGMENTATION ⭐⭐⭐

## TYPES

1. **Region-Based:** Region growing, splitting-merging
2. **Edge-Based:** Edge detection followed by linking
3. **Clustering:** K-means, watershed
4. **Graph-Based:** Normalized cuts, min-cut

## ALGORITHMS

**Thresholding:** Binary classification by intensity

**K-Means Clustering:** Partition into K regions

**Watershed:** Water-flooding analogy

**Morphological:** Erosion, dilation, opening, closing

---

# MODULE 12: ADVANCED ALGORITHMS ⭐⭐⭐

## RANSAC (Random Sample Consensus)
- Robust model fitting
- Outlier rejection
- Used in feature matching

## EPIPOLAR GEOMETRY ⭐⭐⭐
- Stereo vision geometry
- Fundamental matrix
- Correspondence constraints

## STEREO VISION ⭐⭐⭐
- Disparity estimation
- Depth reconstruction
- 3D point cloud generation

## OPTICAL FLOW
- Motion estimation
- Lucas-Kanade algorithm
- Horn-Schunck method

---

# MODULE 13: DIMENSIONALITY REDUCTION ⭐⭐⭐

## PCA (Principal Component Analysis)
- Linear dimensionality reduction
- Finds principal directions
- Energy maximization
- Eigenvalue decomposition

## LDA (Linear Discriminant Analysis)
- Supervised dimensionality reduction
- Maximizes class separability
- Better for classification

## ICA (Independent Component Analysis)
- Non-Gaussian component recovery
- Used in blind source separation
- Different objective than PCA

---

# QUICK REVISION SUMMARIES

## FORMULA SHEET

### **SAMPLING & QUANTIZATION**
```
Nyquist: fs ≥ 2 × fmax
Digital image: f(x,y), x∈[0,M-1], y∈[0,N-1]
```

### **TRANSFORMATIONS**
```
Translation: [1 0 tx; 0 1 ty; 0 0 1]
Rotation: [cos θ -sin θ 0; sin θ cos θ 0; 0 0 1]
Scaling: [sx 0 0; 0 sy 0; 0 0 1]
```

### **INTENSITY TRANSFORM**
```
Negative: s = L - 1 - r
Logarithmic: s = c × log(1 + r)
Power-law: s = c × r^γ
```

### **HISTOGRAM EQUALIZATION**
```
sk = round((L-1) × Σ(nj/N)) for j=0 to k
Or: sk = round((L-1) × CDF(rk))
```

### **CONVOLUTION**
```
g(x,y) = Σ Σ w(s,t) × f(x+s, y+t)
```

### **FOURIER TRANSFORM**
```
F(u,v) = Σ Σ f(x,y) × e^(-j2π(ux/M + vy/N))
Convolution theorem: f ⊗ h ⟷ F × H
```

### **GRADIENT**
```
∇f = [∂f/∂x, ∂f/∂y]
|∇f| = √((∂f/∂x)² + (∂f/∂y)²)
```

---

# EXAM PREPARATION STRATEGY

## TIME ALLOCATION

**70-80 mark distribution:**
- Image Formation & Basics: 10 marks
- Sampling & Quantization: 8 marks
- Transformations: 10 marks
- Intensity Transforms: 8 marks
- Spatial Filtering: 10 marks
- Histogram Processing: 8 marks
- Frequency Domain: 8 marks
- Edge Detection: 10 marks
- Features/Segmentation: 8 marks

## STUDY SEQUENCE

**Day 1 Morning:**
1. Modules 1-3 (Basics, Formation, Sampling)
2. Module 4 (Transformations)
3. Module 5 (Intensity)

**Day 1 Afternoon:**
4. Module 6 (Spatial Filtering)
5. Module 7 (Histogram)
6. Module 8 (Frequency)

**Day 1 Evening:**
7. Module 9 (Edge Detection)
8. Modules 10-13 (Advanced)
9. Quick revision

## LAST-DAY FOCUS

**Most Important (3-4 hours):**
- Convolution & Filtering
- Edge Detection (Canny)
- Histogram Equalization
- Fourier Transform
- Image Transformations

**Important (2-3 hours):**
- Sampling & Quantization
- Intensity Transforms
- Image Segmentation

**Reference (1-2 hours):**
- Advanced algorithms
- Dimensionality reduction
- Features

## EXAM TIPS

1. **Read carefully** - Understand what's asked
2. **Draw diagrams** - Visual explanations help
3. **Show formulas** - Demonstrate understanding
4. **Step-by-step solutions** - For numericals
5. **Label results** - Clear presentation
6. **Time management** - Allocate time by marks

---

# FINAL CHECKLIST

Before exam, ensure you can:

✅ Define all basic concepts  
✅ Explain sampling & quantization  
✅ Apply geometric transformations  
✅ Apply intensity transforms (positive, log, power-law)  
✅ Perform histogram equalization  
✅ Apply spatial filters (mean, Gaussian, median)  
✅ Explain edge detection methods  
✅ Understand Fourier Transform basics  
✅ Explain frequency domain filtering  
✅ Solve numericals step-by-step  
✅ Answer 10-mark questions comprehensively  

---

# GOOD LUCK! 🎓

**Target: 70-80 marks**
**You've got this! 💪**

---

*End of Complete Computer Vision Bootcamp*  
*Last Updated: April 26, 2026*
