# University Data Imputation using K-Means Clustering

This repository contains a Google Colab notebook (`cdist2.ipynb`) that demonstrates a method for imputing missing numerical data in university records. It leverages K-Means clustering and distance metrics to find similar records and use their cluster's characteristics for imputation.

## Table of Contents

-   [Overview](#overview)
-   [Key Features](#key-features)
-   [Technologies Used](#technologies-used)
-   [Notebook Structure & Analysis Steps](#notebook-structure--analysis-steps)
-   [Key Insights & Results](#key-insights--results)
-   [How to Use](#how-to-use)

---

## Overview

The `cdist2.ipynb` notebook provides a practical example of imputing missing numerical attributes for a specific university (Tufts University, in this case) using unsupervised learning techniques. The core idea is to identify complete university records, cluster them based on their attributes, find the cluster most similar to the target university (considering its non-missing attributes), and then use the statistical properties (mean) of that cluster to fill in the missing values.

## Key Features

-   **Data Loading & Preprocessing**: Efficiently loads university data and separates numerical attributes, handling records with and without missing values.
-   **Data Scaling**: Utilizes `StandardScaler` to normalize numerical features, crucial for distance-based algorithms like K-Means.
-   **K-Means Clustering**: Groups similar universities into distinct clusters based on their complete numerical profiles.
-   **Similarity Search**: Employs `scipy.spatial.distance.cdist` to find the most similar complete record to a target university (e.g., Tufts University) based on their shared, non-missing attributes.
-   **Cluster-Based Imputation**: Imputes missing values in the target record by using the mean values of the cluster to which its most similar complete record belongs.

## Technologies Used

The notebook is written in Python and leverages the following libraries:

-   **pandas**: For data manipulation and analysis.
-   **scikit-learn**:
    -   `sklearn.preprocessing.StandardScaler`: For feature scaling.
    -   `sklearn.cluster.KMeans`: For performing K-Means clustering.
-   **scipy**:
    -   `scipy.spatial.distance.cdist`: For calculating distances between data points.

## Notebook Structure & Analysis Steps

The notebook is organized into logical steps, following a typical machine learning workflow:

1.  ### **Setup and Data Loading (Cells 1-2)**
    -   Imports necessary libraries (`pandas`, `StandardScaler`, `KMeans`, `cdist`).
    -   Loads the `Universities.csv` dataset into a pandas DataFrame.

2.  ### **Data Preprocessing & Segregation (Cells 3-4)**
    -   Drops non-numerical columns (`College Name`, `State`) to prepare data for numerical analysis.
    -   Separates the dataset into `full_records` (records with no missing values) and `notfull_records` (records with at least one missing value).

3.  ### **K-Means Clustering on Complete Data (Cells 5-6)**
    -   Scales the `full_records` using `StandardScaler` to ensure all features contribute equally to distance calculations.
    -   Applies K-Means clustering with 5 clusters (`n_clusters=5`, `random_state=42`) to the scaled complete data.
    -   Adds the assigned cluster labels back to the `full_records` DataFrame.

4.  ### **Target Record Identification (Cells 7-8)**
    -   Identifies and extracts the record for 'Tufts University' from the original dataset.
    -   Extracts only the numerical columns from the Tufts record and identifies which of these columns have non-null values.

5.  ### **Similarity-Based Imputation Strategy (Cells 9-13)**
    -   **Critical Step**: Scales only the common non-null features of Tufts University and the corresponding features across *all* complete records.
        *(Note: There appears to be a typo in the original notebook's Cell 9, using `complete_records` instead of `full_records`. Assuming `full_records[nonnull_columns]` was intended for scaling the subset of complete records for similarity comparison).*
    -   Calculates the distance between the normalized Tufts record (using its non-null features) and all other normalized complete records (using their corresponding features) using `cdist`.
    -   Identifies the complete record that is closest to Tufts University.
    -   Retrieves the cluster label of this closest record.
    -   Calculates the mean of all numerical attributes for all universities belonging to that specific cluster. This forms the basis for imputation.
    -   Iterates through the original Tufts record; for any missing (NaN) values, it replaces them with the corresponding mean value from the identified cluster's averages.

6.  ### **Results (Cell 14)**
    -   Prints the `tufts_completed` DataFrame, which now contains the original Tufts data with missing values filled by cluster means.

7.  ### **Full Notebook Run (Cell 15)**
    -   A consolidated cell containing all the code from previous steps, useful for a quick re-run or demonstration.

## Key Insights & Results

The notebook successfully demonstrates a robust method for imputing missing data in a structured dataset. By leveraging K-Means clustering, it groups similar entities, enabling context-aware imputation. The final output is a complete record for 'Tufts University', with its previously missing numerical values intelligently estimated based on the characteristics of its most similar peer group. This method is more sophisticated than simple mean or median imputation, as it considers the multivariate relationships within the data.

## How to Use

To run this notebook:

1.  **Environment**: This notebook is designed for Google Colab.
2.  **Data**: Ensure the `Universities.csv` dataset is available in your Colab environment at the path `/content/Universities.csv`. You can upload it directly to your Colab session's files.
3.  **Execution**: Open the `cdist2.ipynb` notebook in Google Colab and run all cells sequentially.
    -   You can run individual cells by clicking the play button next to them.
    -   Alternatively, go to `Runtime -> Run all` to execute the entire notebook.

The final output, `tufts_completed`, will be printed, showing Tufts University's record with imputed values.