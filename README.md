Handwritten Nepali Digit Feature Extraction using Image Processing

A Python-based image-processing project that automatically extracts handwritten Nepali (Devanagari) digits ०–९ from scanned grid sheets, preprocesses and normalizes each digit, extracts numerical shape and HOG features, and exports the generated datasets to CSV files.

📌 Project Overview

This project implements a complete image-processing pipeline for handwritten Nepali digit feature extraction.

The system automatically:

📷 Reads scanned handwritten Nepali digit sheets

🎨 Converts images to grayscale

🧹 Reduces image noise using Gaussian filtering

⚫ Applies adaptive and Otsu thresholding

🧱 Detects horizontal and vertical grid lines using morphological operations

✂️ Extracts each handwritten digit from its grid cell

🏷️ Assigns labels from 0 to 9 using the fixed digit order

🧼 Removes remaining grid lines and small noise components

📐 Normalizes every digit to 28 × 28 pixels

📊 Extracts 19 basic shape features

📈 Extracts 324 HOG features

📄 Exports basic, HOG, combined and raw-pixel datasets to CSV files

The final validated dataset contains 540 digit images, with 54 samples for each digit class.

📂 Project Structure

Handwritten_Nepali_Digit_Feature_Extraction/
│
├── Image_extraction/
│   ├── image1.jpeg
│   ├── image2.jpeg
│   ├── image3.jpeg
│   ├── main.ipynb
│   ├── vertical_lines.png
│   ├── horizontal_lines.png
│   ├── grid.png
│   └── Prepared datasets/
│       └── Extracted digit images
│
├── Final/
│   ├── Image_preprocessing.ipynb
│   ├── Feature_extraction.ipynb
│   │
│   ├── processed_28x28/
│   │   └── 540 normalized digit images
│   │
│   └── feature_csv/
│       ├── nepali_digit_basic_features.csv
│       ├── nepali_digit_hog_features.csv
│       ├── nepali_digit_basic_hog_combined_features.csv
│       └── nepali_digit_pixels_28x28.csv
│
└── README.md

The .git folder is not part of the project documentation and should not be copied when creating a new repository.

🔄 Processing Pipeline

Scanned Grid Sheets
        │
        ▼
Grayscale Conversion
        │
        ▼
Gaussian Noise Reduction
        │
        ▼
Adaptive Gaussian Thresholding
        │
        ▼
Horizontal and Vertical Grid Detection
        │
        ▼
Grid-Based Digit Extraction
        │
        ▼
Position-Based Labeling
        │
        ▼
Otsu Thresholding
        │
        ▼
Grid-Line and Noise Removal
        │
        ▼
28×28 Normalization
        │
        ▼
Basic + HOG + Raw Pixel Features
        │
        ▼
CSV Feature Datasets

⚙️ Technologies Used

Technology

Purpose

Python

Programming language

OpenCV

Image processing, thresholding, morphology and contour analysis

NumPy

Pixel and numerical operations

Pandas

Dataset creation and CSV export

Matplotlib

Image and processing-stage visualisation

Scikit-image

HOG feature extraction

Jupyter Notebook

Development and execution environment

Pathlib

File and folder path management

📥 Input

The input consists of scanned or photographed grid sheets containing handwritten Nepali digits in the repeating order:

०  १  २  ३  ४  ५  ६  ७  ८  ९

The current project processes:

Image_extraction/
├── image1.jpeg
├── image2.jpeg
└── image3.jpeg

Each valid sheet contains 18 rows × 10 digit columns = 180 digits.

3 sheets × 180 digits = 540 final digit images

Supported image formats include:

JPG

JPEG

PNG

BMP

TIFF

🚀 How to Run

Step 1: Clone the repository

git clone <repository-url>

Step 2: Open the project

cd Handwritten_Nepali_Digit_Feature_Extraction

Step 3: Install the required packages

pip install opencv-python numpy pandas matplotlib scikit-image jupyter

Step 4: Add scanned sheets

Place the scanned digit sheets inside:

Image_extraction/

Update the image_files list in main.ipynb when different filenames are used.

Step 5: Run the notebooks in order

Image_extraction/main.ipynb
            ↓
Final/Image_preprocessing.ipynb
            ↓
Final/Feature_extraction.ipynb

⚠️ Important Folder-Path Setup

The notebooks use Path.cwd(), so they should be opened and executed from their own folders.

Before running Final/Image_preprocessing.ipynb, use the extracted-image folder path:

PROJECT_DIR = Path.cwd()
DATASET_DIR = PROJECT_DIR.parent / "Image_extraction" / "Prepared datasets"
PROCESSED_DIR = PROJECT_DIR / "processed_28x28"
CSV_DIR = PROJECT_DIR / "csv_output"

Before running Final/Feature_extraction.ipynb, make sure the processed-image path matches the actual output folder:

PROJECT_DIR = Path.cwd()
PROCESSED_DIR = PROJECT_DIR / "processed_28x28"
CSV_DIR = PROJECT_DIR / "feature_csv"

This avoids a FileNotFoundError caused by the notebook referring to a folder named Preprocessed images while the repository contains processed_28x28.

📖 Notebook Description

📗 1. Image Extraction — main.ipynb

This notebook detects the grid structure and extracts every digit cell from the scanned sheets.

Operations

Read scanned sheets

Resize sheets while preserving aspect ratio

Convert images to grayscale

Apply a 5×5 Gaussian filter

Apply adaptive Gaussian thresholding

Perform morphological opening

Detect vertical lines using a vertical structuring element

Detect horizontal lines using a horizontal structuring element

Combine detected lines into a complete grid

Use projection analysis to locate line coordinates

Crop individual digit cells

Save extracted digit images

Output

Image_extraction/
└── Prepared datasets/

📘 2. Image Preprocessing — Image_preprocessing.ipynb

This notebook cleans and standardises every extracted digit image.

Operations

Read each extracted digit image

Convert it to grayscale

Apply a 3×3 Gaussian filter

Apply inverse Otsu thresholding

Detect and remove remaining grid lines

Apply morphological closing to reconnect small stroke gaps

Remove small components using connected-component analysis

Find the foreground bounding box

Preserve the digit aspect ratio during resizing

Centre the digit on a black canvas

Create the final 28×28 binary image

Assign digit labels and sample-group information

Output

Final/
└── processed_28x28/

Example filename:

group_000_digit_0_index_0000.png

📙 3. Feature Extraction — Feature_extraction.ipynb

This notebook converts every normalised digit image into numerical features.

Feature categories

Basic shape and structural features

HOG gradient features

Combined basic and HOG features

Flattened 28×28 raw-pixel values

Output

Final/
└── feature_csv/

📐 Basic Features Extracted

A total of 19 basic features are extracted from each digit:

Foreground pixel count

Foreground ratio

Bounding-box width

Bounding-box height

Aspect ratio

Bounding-box density

Centroid X

Centroid Y

Contour area

Perimeter

Solidity

Compactness

Orientation

Eccentricity

Number of holes

Horizontal symmetry

Vertical symmetry

Horizontal transition rate

Vertical transition rate

📈 HOG Features

HOG stands for Histogram of Oriented Gradients. It captures local edge direction and handwritten stroke structure.

The following configuration is used:

HOG Parameter

Value

HOG image size

32 × 32

Orientations

9

Pixels per cell

8 × 8

Cells per block

2 × 2

Block normalisation

L2-Hys

HOG features per digit

324

The 28×28 image is resized to 32×32 before HOG extraction so it can be divided exactly into 4×4 cells.

📊 Dataset Overview

Dataset Property

Value

Total valid images

540

Digit classes

10

Labels

0–9 / ०–९

Samples per class

54

Sample groups

54

Normalised image size

28×28

Class distribution

Balanced

📄 Generated CSV Files

1. Basic Feature Dataset

nepali_digit_basic_features.csv

540 rows

5 metadata columns

19 basic features

Shape: 540 × 24

2. HOG Feature Dataset

nepali_digit_hog_features.csv

540 rows

5 metadata columns

324 HOG features

Shape: 540 × 329

3. Combined Feature Dataset

nepali_digit_basic_hog_combined_features.csv

540 rows

5 metadata columns

19 basic features

324 HOG features

Shape: 540 × 348

4. Raw Pixel Dataset

nepali_digit_pixels_28x28.csv

540 rows

5 metadata columns

784 normalised pixel values

Shape: 540 × 789

🏷️ Metadata Columns

Every CSV file contains the following metadata columns:

Column

Description

image_index

Unique image sequence number

sample_group

Group containing one sample of digits 0–9

filename

Source image filename

label

Numerical class label from 0 to 9

nepali_label

Nepali digit label from ० to ९

📁 Final Output Folder

Final/
│
├── processed_28x28/
│   └── 540 normalised digit images
│
└── feature_csv/
    ├── nepali_digit_basic_features.csv
    ├── nepali_digit_hog_features.csv
    ├── nepali_digit_basic_hog_combined_features.csv
    └── nepali_digit_pixels_28x28.csv

💡 Features

Automatic grid-line detection

Automatic digit-cell extraction

Position-based digit labelling

Gaussian noise reduction

Adaptive Gaussian thresholding

Otsu thresholding

Morphological grid-line removal

Connected-component noise removal

Aspect-ratio-preserving resizing

28×28 digit normalisation

Basic shape-feature extraction

HOG feature extraction

Raw-pixel feature generation

Balanced dataset creation

CSV dataset export

Modular notebook-based workflow

📌 Project Workflow

Input Grid Sheet
       │
       ▼
Grid Detection
       │
       ▼
Digit Extraction
       │
       ▼
Labelling
       │
       ▼
Image Preprocessing
       │
       ▼
28×28 Normalisation
       │
       ▼
Feature Extraction
       │
       ▼
CSV Datasets

📦 Requirements

opencv-python
numpy
pandas
matplotlib
scikit-image
jupyter

Install all packages using:

pip install opencv-python numpy pandas matplotlib scikit-image jupyter

⚠️ Limitations

Grid extraction depends on clear horizontal and vertical lines.

Extra or missing detected lines can generate incorrect digit cells.

Position-based labelling requires digits to remain in the correct 0–9 order.

The current dataset contains a limited number of writers and samples.

This project performs feature extraction only; it does not train or evaluate a recognition classifier.

The initial extraction stage may contain additional invalid cells when a handwritten stroke is mistakenly detected as a grid line. Only the validated 540 images are included in the final processed dataset.

🔮 Future Work

Train and compare KNN, SVM and Random Forest classifiers

Evaluate classification accuracy using basic, HOG and combined features

Add more handwriting samples from different writers

Apply data augmentation

Improve grid detection for rotated or low-quality images

Use CNN for end-to-end handwritten Nepali digit recognition

📚 Learning Outcome

This project demonstrates the complete classical image-processing workflow for handwritten Nepali digit feature extraction, including image acquisition, grid detection, segmentation, labelling, preprocessing, normalisation, contour analysis, geometric feature extraction, HOG feature extraction and structured CSV dataset generation for future machine-learning applications.

👨‍💻 Author

Developed as an academic project for Image Processing and Pattern Recognition (IPPR).

About

A Python-based handwritten Nepali digit feature-extraction system using OpenCV and classical image-processing techniques, including grid-based segmentation, preprocessing, 28×28 normalisation, basic shape features, HOG features and CSV dataset generation.
