
# Large-Scale Data Transformation and Validation Project

## Overview
This project is designed to handle the transformation and validation of large datasets, scaling up to 1 billion rows. It includes processes for fitting, transforming, and validating data using Python with libraries like `Pandas`, `Dask`, and `NumPy`. The project also evaluates transformations using extreme values and calculates error metrics such as MAE and RMSE for performance assessment.

## Project Structure
- **Data Loading and Scaling**: Loads a chunk of data from `Credit.csv` and scales it up to 1 billion rows.
- **Transformation**: Transforms data in chunks to manage memory efficiently.
- **Validation**: Checks the accuracy and reversibility of transformations using extreme values and the full dataset.

## Requirements
- Python 3.x
- `Pandas`
- `NumPy`
- `Dask`
- `scikit-learn`

### Installation
To install the required packages, run:
```bash
pip install pandas numpy dask scikit-learn
```

## Workflow
1. **Data Scaling**: The initial data from `Credit.csv` is scaled to 1 billion rows by repeating the data and converting it to `float32` for memory efficiency.
2. **Transformer Initialization**: A custom `DataTransformer` class is initialized and fitted using a sample of the scaled data.
3. **Transformation and Validation**:
   - Transform and inverse-transform extreme values (e.g., 0 and \(10^{10}\)) to check robustness.
   - Transform the full dataset in smaller, manageable chunks to avoid memory overload.
   - Print intermediate outputs to track progress.
   - Calculate MAE and RMSE for error analysis.

## Error Metrics
- **MAE (Mean Absolute Error)**: Measures the average magnitude of the errors between the original and inverse-transformed data.
- **RMSE (Root Mean Squared Error)**: Evaluates the square root of the average of squared errors to indicate overall accuracy.

## Performance
- **Timing**: Measures and displays the time taken for each transformation step.
- **Progress Output**: Prints intermediate data samples and progress updates for better monitoring.
