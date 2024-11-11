# DataTransformer and Large-Scale Data Encoding

## Overview
This project implements a `DataTransformer` class designed for processing and transforming large-scale tabular data to train generative models such as CTGAN. The transformer can handle continuous, categorical, and mixed-type columns and ensures efficient data encoding and decoding, including validation for extreme and edge cases.

## Project Structure
- **`DataTransformer` Class**: A core class responsible for fitting data, encoding it using Bayesian Gaussian Mixture Models (BGMM), transforming data for training, and inverse transforming it back to its original form.
- **Encoding and Decoding**: Methods to process and validate data for large-scale datasets, handling up to billions of rows efficiently.
- **Validation**: Code for checking the validity of the encoding/decoding process, especially for extreme values.

## Features
- **BGMM-based Transformation**: Uses Bayesian Gaussian Mixture Models for continuous and mixed data columns.
- **Categorical Handling**: Supports categorical columns using one-hot encoding.
- **Inverse Transformation**: Converts transformed data back to its original format with validation checks.
- **Scalability**: Optimized for large-scale data handling using Dask and vectorized operations.

## Code Overview

### 1. `DataTransformer` Class
A Python class to transform data for training with generative models and perform inverse transformations.

#### Key Components
- **`__init__()`**: Initializes the transformer with metadata about the columns.
- **`fit()`**: Fits the BGMM models for continuous and mixed-type columns.
- **`transform()`**: Transforms data using fitted BGMM models and one-hot encoding for categorical data.
- **`inverse_transform()`**: Converts transformed data back to its original state.
- **Validation**: Includes checks for NaNs, infinities, and ensures consistent shapes between transformations.

### 2. Main Script
Code to use the `DataTransformer` for processing a large dataset.

```python
from distributed import Client
import dask.dataframe as dd
import numpy as np
import logging
from DataTransformer import DataTransformer  # Assuming this is imported from a module

# Configure logging
logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(message)s')

# Set up Dask client for parallel processing
client = Client(n_workers=2, threads_per_worker=1, memory_limit='4GB')

# Load and prepare data using Dask
chunk_size = "500MB"
dask_df = dd.read_csv("Credit.csv", usecols=["Amount"], blocksize=chunk_size)
dask_df["Amount"] = dask_df["Amount"].astype(np.float32)

# Scale data for testing (e.g., simulating 1 billion rows)
scaled_dask_df = dd.concat([dask_df] * (1_000_000_000 // len(dask_df)), interleave_partitions=True)

# Initialize and fit the DataTransformer
sample_df = scaled_dask_df.head(5_000_000, compute=True)
transformer = DataTransformer(train_data=sample_df['Amount'].values)
transformer.fit()

# Transform data
transformed_data = transformer.transform(sample_df['Amount'].values)
logging.info(f"Transformed data shape: {transformed_data.shape}, Expected output_dim: {transformer.output_dim}")

# Validate and inverse transform
try:
    logging.info("Evaluating inverse transformation.")
    inverse_data = transformer.inverse_transform(transformed_data)
    differences = np.abs(sample_df['Amount'].values - inverse_data)
    max_diff = np.max(differences)
    if max_diff > 1e-5:
        logging.warning(f"Inverse transformation inconsistency detected. Max difference: {max_diff:.5f}")
    else:
        logging.info("Inverse transformation is consistent.")
except Exception as e:
    logging.error(f"Error during inverse transformation: {e}")

# Shut down the client
client.close()
