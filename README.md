# Data Cleaning Application Overview

This Python script provides a robust framework for cleaning datasets, designed to handle common data quality issues such as duplicates and missing values. 

---

# Table of Contents
- [Data Cleaning Application Overview](#data-cleaning-application-overview)
- [Input and File Handling](#1-input-and-file-handling)
- [Data Cleaning Process](#2-data-cleaning-process)  
   a. [Handling Duplicates](#a-handling-duplicates)  
   b. [Handling Missing Values](#b-handling-missing-values)
- [Output and Saving Results](#3-output-and-saving-results)
- [Interactivity and User Experience](#4-interactivity-and-user-experience)
- [Key Features](#5-key-features)


## 1. Input and File Handling
- **User Input**: Prompts the user to input the name of the dataset, which should be located in the `downloads/` directory.
- **Supported File Types**: Handles `.csv` and `.xlsx` file formats.
- **Error Handling**: Validates the existence of the file and its type before proceeding.

---

## 2. Data Cleaning Process
### **a. Handling Duplicates**
- **Detection**:
  - Identifies duplicate rows in the dataset.
  - Differentiates between all duplicate rows and one copy of each duplicate.
- **Removal**:
  - Removes duplicate rows while retaining the first occurrence.
  - If no duplicates are found, the dataset remains unchanged.
- **Reporting**:
  - Prints the total number of duplicates.
  - Outputs details of duplicate rows for user reference.

### **b. Handling Missing Values**
- **Counting NaNs**:
  - Counts and reports the total number of missing (NaN) values in the dataset.
- **Treatment by Data Type**:
  - For numeric columns (`float` and `int`): Fills missing values with the column mean.
  - For object (non-numeric) columns: Removes rows containing missing values.
- **Record Retention**:
  - Stores rows with missing non-numeric values in a separate file for reference.

---

## 3. Output and Saving Results
- **Cleaned Dataset**:
  - Saves the cleaned dataset in the same format as the input file (CSV or Excel).
  - The filename is appended with `_(cleaned)` for clarity.
- **Missing Value Records**:
  - If missing values are found in non-numeric columns, saves a separate file containing these rows. The filename is appended with `_(null_rows)`.
- **Feedback**:
  - Provides a summary of the cleaning process, including:
    - Total rows and columns.
    - Number of duplicates.
    - Number of missing values.
  - Notifies the user of the saved cleaned dataset and reference file.

---

## 4. Interactivity and User Experience
- Displays progress messages to keep the user informed.
- Ensures ease of use by automating repetitive cleaning tasks and providing clear outputs.

---

## Key Features
- **Duplicate Handling**: Identifies and removes duplicates while preserving one occurrence.
- **NaN Management**: Differentiates between numeric and non-numeric columns for targeted missing value treatment.
- **Separate Record Retention**: Saves problematic rows for auditing purposes.
- **Dynamic Support for File Formats**: Compatible with both CSV and Excel files.
- **User-Friendly Interaction**: Offers clear feedback and summaries throughout the process.

---

## Example Usage
1. Place the dataset in the `downloads/` directory.
2. Run the script in a Python environment.
3. Input the filename when prompted (e.g. `sales.csv`).
4. View the summary of the cleaning process and retrieve cleaned files from the directory.

---

This script automates essential data cleaning tasks, ensuring datasets are ready for analysis while retaining transparency by preserving problematic rows for reference.
