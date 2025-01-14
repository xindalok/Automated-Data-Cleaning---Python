# Automated Data Cleaning Script

This Python script provides a robust framework for cleaning datasets, designed to handle common data quality issues such as duplicates and missing values. 
Primarily been used in Jupyter Notebooks.

---

### Table of Contents
- [Input and File Handling](#1-input-and-file-handling)
- [Data Cleaning Process](#2-data-cleaning-process)  
   - [Handling Duplicates](#a-handling-duplicates)  
   - [Handling Missing Values](#b-handling-missing-values)
- [Output and Saving Results](#3-output-and-saving-results)
- [Interactivity and User Experience](#4-interactivity-and-user-experience)
- [Key Features](#key-features)
- [Script code](#code)
   - [Output](#output-from-code)

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
2. Run the script in Jupyter Notebooks.
3. Input the filename when prompted (e.g. `sales.csv`).
4. View the summary of the cleaning process and retrieve cleaned files from the directory.

---

This script automates essential data cleaning tasks, ensuring datasets are ready for analysis while retaining transparency by preserving problematic rows for reference.

--------
--------

### Code: 

``` python
!pip install xlrd


import pandas as pd
import numpy as np
import time
import openpyxl
import os
import random
import xlrd
import re

```

``` python
# check if path exists

def data_cleaning_app(data_name):
    
    data_name = 'downloads/' + data_name
    
    if os.path.exists(data_name):
        if data_name.endswith(".csv"):
            data = pd.read_csv(data_name)
        elif data_name.endswith(".xlsx"):
            data = pd.read_excel(data_name)

        else:
            print("Unknown file type")


    else:
        print("Enter correct data path")


    # Data cleaning

    ### Handle duplicates ###

    # check for duplicates 
    duplicates = data.duplicated()
    keep_duplicates = data[data.duplicated(keep=False)]

    # duplicate records (without keeping both duplicate rows)
    duplicates_record = data[duplicates]

    #drop duplicates (only drops if there are duplicates)
    if duplicates.sum() > 0:
        data_cleaned = data.drop_duplicates()
    else:
        data_cleaned = data

    
    ### Handle missing values ###

    # NaN counter
    null_counter = data_cleaned.isnull().sum().sum()

    # Create a dataframe to store rows that have non-numeric NaN values 
    null_rows = pd.DataFrame()


    # Use fillna for integer and float types, dropna for object types
    columns = data_cleaned.columns
    for col in columns:
        if data_cleaned[col].dtype in (float, int):
            data_cleaned[col].fillna(data_cleaned[col].mean())

        # if column is non-numeric, delete rows that have NaN values
        elif data_cleaned[col].dtype == 'object':
            # Select rows where the column has NaN values
            missing_rows = data_cleaned[data_cleaned[col].isnull()]
            # Indicate the column that missing values were found in 
            missing_rows['Dropped_From'] = col

            # Append the missing rows to the `null_rows` DataFrame
            null_rows = pd.concat([null_rows, missing_rows])

            # Drop entire row from data_cleaned dataset
            data_cleaned.dropna(subset = col, inplace = True)


    # Save a copy of dataset of dropped NaN values for reference 
    if null_rows.shape[0] > 0:
        null_dataset = data_name.split('.')[0] + '_(null_rows)' + '.' + data_name.split('.')[-1]
        null_rows.to_csv(null_dataset, index=False)

    # Extract file type from data_name using regex
    file_type = re.sub(r'[^a-zA-Z]', '', data_name[-4:])
    print(f'Dataset is {file_type} file  \nTotal Rows: {data.shape[0]} \nTotal Columns: {data.shape[1]}')
    print(f'Total Duplicates: {duplicates.sum()}')

    if duplicates.sum() > 0:
        print(f'\n\nDuplicate Records: \n{keep_duplicates.sort_values(by = keep_duplicates.columns[0])}')
    else:
        print(f'\n\nThere are 0 duplicates in dataset.')

    if  null_rows.shape[0] > 0:
        print(f'\n\nThere were {null_counter} NaN values found in the dataset.')
    else:
        print(f'\n\nThere were no NaN values in dataset.')

    data_cleaned_pathname =  data_name.split(".")[0] + '_(cleaned).' + data_name.split(".")[-1]
    stripped_pathname = data_cleaned_pathname.split('/')[1]

    if data_name.split(".")[-1] == 'csv':
        data_cleaned.to_csv(data_cleaned_pathname, index = False)
    elif data_name.split(".")[-1] == 'xlsx':
        data_cleaned.to_excel(data_cleaned_pathname, index = False)


    print(f'\n\n.\n.\n.\n.\n.')

    print(f'\n\nDataset has been cleaned')
    print(f'\n.\n.\n.\n.\n.')

    print(f'\nCleaned dataset is saved as: {stripped_pathname}')

    if null_rows.shape[0] > 0:
        print(f'A file containing rows with null values has been saved as: {null_dataset.split("/")[1]}')
```

``` python
# Code to take user input in a Jupyter Notebook
print("Welcome to Data Cleaning Master!")

# Ask for path and file name
data_name = input("Please enter full dataset name (eg sales.csv): ")

# Display the entered values
print(f"\n\n\nDataset name: {data_name}")
data_cleaning_app(data_name)
```

 ### Output from code: 
 
<img src="images/output" width="1000" height="500" />


