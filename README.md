# Hotel Booking Data Analysis: Unveiling Patterns with PCA

This notebook presents a structured analysis of the *hotel_bookings.csv* dataset with the objective of uncovering underlying patterns and reducing dimensional complexity using Principal Component Analysis (PCA). The workflow follows a rigorous data preparation pipeline, applies PCA using both a standard library implementation and a manual approach, and compares the results to validate correctness and understanding.

## 1. Data Loading and Initial Exploration

The analysis begins by loading the *hotel_bookings.csv* file into a pandas DataFrame. The dataset contains 119,390 observations across 32 variables, offering a comprehensive view of hotel reservation behavior.

## 2. Data Cleaning and Preprocessing

Several data quality issues were addressed to ensure reliable analysis:

* **Handling Missing Values**:

  * *country*: Missing entries were replaced with the label "Unknown".
  * *agent* and *company*: Missing values were set to 0. Notably, the *company* variable contained missing values for over 94% of the records, suggesting that most bookings were individual or that company details were not consistently recorded.
  * *children*: A small number of missing values were also replaced with 0.

* **Data Type Corrections**:

  * The *reservation_status_date* column was converted to a datetime format to support time-based analysis.

## 3. Feature Engineering and Encoding

To enhance the analytical value of the dataset and prepare it for PCA, the following steps were taken:

* **Feature Engineering**:

  * *total_guests*: Computed as the sum of adults, children, and babies.
  * *total_nights*: Derived by summing weekday and weekend stays.

* **Categorical Encoding**:

  * Eleven categorical variables (including hotel type, arrival month, and country) were transformed using one-hot encoding. This expanded the feature space from 32 to 263 variables, making dimensionality reduction a necessary next step.

## 4. Feature Scaling

All features were standardized using *StandardScaler* to ensure a mean of approximately zero and a standard deviation of one. This step is critical for PCA, as it prevents variables with larger scales from disproportionately influencing the results.

## 5. PCA Using Scikit-learn

PCA was first applied using scikit-learn’s implementation:

* **Handling Remaining Missing Values**:

  * A small number of NaN values appeared after encoding. Since PCA cannot operate on missing data, the affected rows were removed prior to fitting the model.

* **Dimensionality Reduction Results**:

  * The analysis showed that 36 principal components were sufficient to explain at least 95% of the total variance in the 263-feature dataset. This represents a reduction of approximately 86% in dimensionality while retaining most of the informational content.

* **Explained Variance Analysis**:

  * A cumulative explained variance plot confirmed a clear point of diminishing returns, supporting the choice of 36 components.

* **Visualization**:

  * The first two principal components were plotted and color-coded by booking cancellation status. This visualization provided an initial indication of how dominant patterns in the data relate to cancellations.

## 6. Manual Implementation of PCA

To deepen understanding of PCA, the method was also implemented from first principles:

* Computation of the covariance matrix from the standardized data.
* Extraction of eigenvalues and eigenvectors.
* Sorting components by descending eigenvalues.
* Selection of the top components explaining 95% of the variance.
* Projection of the original data onto the selected eigenvectors.

This manual process independently yielded the same requirement of 36 components to reach the 95% variance threshold.

## 7. Comparison of Implementations

The results of the manual PCA were compared with those from scikit-learn:

* **Explained Variance**:

  * The explained variance ratios matched closely across corresponding components.

* **Transformed Features**:

  * Principal component scores from both methods exhibited near-perfect correlation (±1.0). Any sign differences are expected and do not affect
