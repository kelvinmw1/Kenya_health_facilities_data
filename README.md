
#Kenya Health Facilities Data Analysis

## Project Overview
This project analyzes health facility data in Kenya to support policy planning and service gap identification. The dataset contains facility details, including ownership, type, and location. The goal is to clean messy data, standardize facility classifications, and analyze distribution patterns by county.


## Tools Used
- **Python** – data cleaning and analysis  
- **SQL** – optional for data validation and aggregation  
- **Excel** – quick sanity checks and table previews  
- **Power BI** – dashboards and visualizations  
- **Git & GitHub** – version control and portfolio sharing


## Dataset
- Raw dataset: Kenyan health facilities (public, private, faith-based, NGO, etc.)  
- Key fields:
  - `facility_type` → standardized as `Hospital`, `Health Centre`, `Dispensary`, `Clinic`, `Other`  
  - `ownership` → standardized as `Public`, `Private`, `Faith-Based`, `NGO`, `Other`  
  - `county` → cleaned and standardized for analysis

## Steps Completed

### 1. Data Cleaning
- Removed duplicates and unnecessary columns  
- Fixed typos and inconsistent formatting  
- Standardized **ownership** and **facility type** using domain-informed mappings

### 2. Data Validation
- Checked unique values after standardization  
- Ensured no unexpected or null values in key columns  

Example check in Python:
```python
df['ownership_standardized'].value_counts(dropna=False)
df['facility_type_standardized'].value_counts(dropna=False)
````

### 3. County-Level Preparation

* Cleaned county names (strip spaces, title case)
* Verified all 47 counties are present

```python
df['county'] = df['county'].str.strip().str.title()
df['county'].nunique()
```

### 4. Facility Distribution Analysis

Counted facilities by type per county:

```python
facility_by_county = (
    df.groupby(['county', 'facility_type_standardized'])
      .size()
      .reset_index(name='facility_count')
)
```

### 5. Ownership Analysis

Counted facilities by ownership per county:

```python
ownership_by_county = (
    df.groupby(['county', 'ownership_standardized'])
      .size()
      .reset_index(name='facility_count')
)
```

### 6. Service Gap Analysis

Filtered hospitals and counted per county:

```python
hospital_counts = (
    df[df['facility_type_standardized'] == 'Hospital']
      .groupby('county')
      .size()
      .reset_index(name='hospital_count')
)
```

* This identifies counties with few or no hospitals.
