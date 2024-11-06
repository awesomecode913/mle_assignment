# Data Transformation and GMM Encoding Analysis

This project focuses on transforming large datasets using a Gaussian Mixture Model (GMM) encoder and evaluating its accuracy, particularly for handling extreme values. It is divided into two main tasks:

1. **Task 1**: Process a large dataset of up to 1 billion rows by loading and transforming it in chunks, with an emphasis on efficient processing time.
2. **Task 2**: Assess the accuracy of the GMM encoder, particularly with extreme values, and analyze the time taken for transformations.

---

## Table of Contents

- [Requirements](#requirements)
- [Installation](#installation)
- [Tasks Overview](#tasks-overview)
  - [Task 1: Processing Large Dataset in Chunks](#task-1-processing-large-dataset-in-chunks)
  - [Task 2: Testing GMM Encoder Accuracy](#task-2-testing-gmm-encoder-accuracy)

---

## Requirements

- Python 3.6+
- Libraries: `pandas`, `numpy`, `scikit-learn`, `joblib`

## Installation

1. Install the required packages:
   ```bash
   pip install pandas numpy scikit-learn joblib

## task-1-processing-large-dataset-in-chunks
In this task, we handle a very large dataset by loading it in chunks to save memory. We transform each chunk separately and record the time taken for each step. This approach allows us to work with large datasets without loading everything into memory at once.

Steps
1. Import Libraries: First, we import necessary libraries like pandas, time, and joblib for handling data and measuring processing time.

2. Initialize Transformer with Sample Data: We load a small part of the dataset to initialize the DataTransformer with metadata. This allows the transformer to understand the data structure.

3. Define a Chunk Processing Function: The function fit_transform_chunk processes each chunk:

    - Fit: It fits the model to the chunk and measures the fitting time.
    - Transform: It transforms the chunk’s data and measures the transformation time.

4. Load and Process All Chunks: We reload the dataset in chunks and use Parallel processing to handle multiple chunks at once. This speeds up the processing time.

5. Verify Results: After processing, we check the number of processed chunks and display a sample transformed chunk to confirm the operation worked.

## task-2-testing-gmm-encoder-accuracy
In this task, we check how accurately the GMM encoder transforms and restores data, focusing on extreme values (e.g., very large or very small numbers) to ensure the model handles them well. We also measure the time for these transformations.

Steps
1. Define Extreme Values: We create a small set of extreme values to test the encoder’s behavior with unusual data points.

2. Transform and Inverse Transform Extreme Values:

    - We transform the extreme values and record the time.
    - We then inverse transform (decode) these values back to their original form and record the time.
3. Calculate Accuracy: We calculate the Mean Absolute Error (MAE) and Root Mean Squared Error (RMSE) to check if the encoded and decoded values match the original values.

4. Transform and Inverse Transform Full Dataset: Finally, we repeat the transformation and inverse transformation on the entire dataset and record the time for each step. We calculate MAE and RMSE to ensure accuracy across the full dataset.
