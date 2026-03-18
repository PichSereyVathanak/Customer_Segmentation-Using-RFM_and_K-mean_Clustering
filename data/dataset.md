# Dataset Information

## Overview

This dataset is sourced from the **Online Retail** dataset available on Kaggle (https://www.kaggle.com/datasets/yasserh/customer-segmentation-dataset). It contains transaction data from a UK-based online retail company, spanning from December 2010 to December 2011. The dataset is used for customer segmentation analysis using RFM (Recency, Frequency, Monetary) analysis and K-Means clustering.

The data folder is organized into three main subdirectories: `raw/`, `processed/`, and `cluster/`, representing the different stages of data processing.

## Data Sources

### Raw Data (`data/raw/`)

- **File**: `Online_Retail.xlsx`
- **Format**: Excel spreadsheet (.xlsx)
- **Description**: The original, unprocessed dataset downloaded from Kaggle. This contains the raw transaction records with potential issues like missing values, duplicates, and inconsistent data types.
- **Size**: Approximately 541,909 rows (transactions)
- **Key Columns**:
  - `InvoiceNo`: Invoice number (nominal, 6-digit integral number)
  - `StockCode`: Product code (nominal, 5-digit integral number)
  - `Description`: Product description (nominal)
  - `Quantity`: Quantity of each product per transaction (numeric)
  - `InvoiceDate`: Invoice date and time (numeric, YYYY-MM-DD HH:MM:SS format)
  - `UnitPrice`: Unit price (numeric, in sterling)
  - `CustomerID`: Customer number (nominal, 5-digit integral number)
  - `Country`: Country name (nominal)

### Processed Data (`data/processed/`)

This subdirectory contains cleaned and processed versions of the dataset, generated through the preprocessing steps in `notebooks/1_data_preprocessing.ipynb`.

- **File 1**: `online_retail_clean.csv`
  - **Description**: Initial cleaned version after removing missing values, duplicates, negative quantities, and zero unit prices. Text normalization applied to descriptions, and data types converted appropriately. A new `TotalSpend` column is added (Quantity * UnitPrice).
  - **Columns**: InvoiceNo, StockCode, Description, Quantity, InvoiceDate, UnitPrice, CustomerID, Country, TotalSpend

- **File 2**: `online_retail_final.csv`
  - **Description**: Final processed dataset with additional derived temporal features for analysis.
  - **Additional Columns**:
    - `InvoiceYearMonth`: Year and month of invoice (YYYY-MM format)
    - `InvoiceDateOnly`: Date part of invoice (YYYY-MM-DD format)
    - `InvoiceDayOfWeek`: Day of the week (e.g., Monday, Tuesday)
    - `InvoiceHour`: Hour of the day (0-23)
  - **Full Columns**: InvoiceNo, StockCode, Description, Quantity, InvoiceDate, UnitPrice, CustomerID, Country, TotalSpend, InvoiceYearMonth, InvoiceDateOnly, InvoiceDayOfWeek, InvoiceHour

### Clustered Data (`data/cluster/`)

- **File**: `online_retail_cluster.csv`
- **Description**: Customer-level aggregated data with RFM scores and cluster assignments, generated through RFM analysis and K-Means clustering in `notebooks/3_RFM_and_Kmean_clustering.ipynb`.
- **Columns**:
  - `CustomerID`: Unique customer identifier
  - `Recency`: Days since last purchase (lower is better)
  - `Frequency`: Total number of purchases
  - `Monetary`: Total monetary value of purchases
  - `RScore`: Recency score (1-5, 5 being most recent)
  - `FScore`: Frequency score (1-5, 5 being most frequent)
  - `MScore`: Monetary score (1-5, 5 being highest spending)
  - `Cluster`: Assigned cluster number (0-4)
  - `Description`: Text description of the cluster characteristics
  - `Type`: Simplified cluster type (e.g., VIP, Active, Inactive, High Spenders)

## Data Processing Pipeline

1. **Raw Data Loading**: Load from Excel file
2. **Cleaning**: Handle missing values, duplicates, invalid entries
3. **Feature Engineering**: Add TotalSpend, temporal features
4. **RFM Calculation**: Aggregate to customer level, compute RFM metrics
5. **Clustering**: Apply K-Means on RFM scores to segment customers

## Usage Notes

- The dataset contains only transactions with valid CustomerID (not null)
- Negative quantities indicate returns/cancellations
- All monetary values are in USD ($)
- The analysis focuses on customers from the United Kingdom primarily
