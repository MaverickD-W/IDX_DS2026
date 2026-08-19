# Dataset Source
CRMLS data was downloaded through company-provided FileZilla access.
The data used in this internship was initially CRMLS data from 05/2025 to 05/2026, but was converted to 06/2025 - 06/2026 as more data became available for 2026.


# Preprocessing

## Preprocessing Analysis:
Defined CRITICAL COLUMNS for cleaning (must be non-null) and further analysis.
  - "ClosePrice", "BedroomsTotal", "BathroomsTotalInteger", "LivingArea", "LotSizeSquareFeet", "DaysOnMarket"

PRE-SET VALUES:
  - "PropertyType" = "Residential"
  - "PropertySubType" == "SingleFamilyResidence"
  - "StateOrProvince" == "CA"
    - *Note: Null or zero-valued 'Longitude' and 'Latitude' values will not need to be dropped as long as 'StateorProvince' == "CA"*
  
  *Note: 'MlsStatus' == "Closed" is implied for non-null 'CloseDate' values*

COMPARISONS:
  - "PostalCode", "City", "CountyOrParish"
  - "ParkingTotal", "GarageSpaces"
  - "Stories", "Levels"

ADDITIONAL FEATURES:
  - "YearBuilt", "ViewYN", "FireplaceYN", "NewConstructionYN"
  - *Note: 'YearBuilt' only fills in the gaps for 'NewConstructionYN' if a house has been built from scratch. Thus, it does not account for any type of additions or improvements constructed.*

## Steps:
[ANALYSIS]

[CLEANING]
- DROPPED all DUPLICATE rows
- DROPPED all rows with NON-NULL VALUES in [critical columns]
- VALIDATED data TYPES for [critical columns]
- KEEPS rows in data frame where [critical columns] have VALUES GREATER THAN 0

[REANALYSIS]
  - "PostalCode" had 1 remaining null-value and the most unique values when compared to "City" and "CountyOrParish"
  - "ParkingTotal" had 1 remaining null-value, significantly less than "GarageSpaces"
  - "Stories" and "Levels" both retained a significant number of null-values
  - "YearBuilt" and "FireplaceYN" respectively contain less than 100 non-null values
- DEFINED additional critical columns, from reanalysis, as [new columns]

[RECLEANING]
  - DISCREPENCIES in "ClosePrice" data entry were handled by comparison to "ListPrice"
    - DROPPED rows where ["ClosePrice"/"ListPrice"] or ["ListPrice"/"ClosePrice"] was GREATER THAN 2
  - VALIDATED data TYPES for [new columns]
  - DROPPED all rows with NON-NULL VALUES in [new columns]
  - DROPPED rows where "ParkingTotal" was 0, but non-null "GarageSpaces" values were not 0

[REANALYSIS]

[CLEANING OULIERS]
  - DROPPED rows where ["YearBuilt"] was GREATER THAN YEAR OF ["CloseDate"]

[ENCODING & RECLEANING]
  - DROPPED columns with almost entirely null-values, as well as ones with listing information unnecessary for analysis
  - CONVERTS "CloseDate" to TYPE datetime and "PostalCode" to TYPE int
  - ENCODES remaining True/False columns (columns with "YN" in the title) as 1/0 values
  - ADDS COLUMN "SaleMonth" from month values in "CloseDate"

## Test-Train Split
Defines Test set data frame as the most recent month in the data

Defines Training set data frame as all remaining data (months prior to the most recent)


# Testing

## Models
Regression
   - Linear Regression
   - Decision Tree Regression
   - Random Forest Regression

Boosting
  - XGBoost
  - Gradient Boosting Regression
  - LightGBM
  - Histogram-based Gradient Boosting Regression

## Scores

- R2 Score (R2)
- Mean Absolute Percentage Error (MAPE)
- Median Absolute Percentage Error (MdAPE)


# Best Results

*Note: These are the results after the data was enriched with feature engineering and school district information.*

## Linear Regression
Non-Transform
 - R2: **0.7303**
 - MAPE: **0.2703**
 - MdAPE: **0.18**

Log Transform
 - R2: **1.0**
 - MAPE: **0.0**
 - MdAPE: **0.0**

## Decision Tree Regression
Non-Transform
 - R2: **0.9828**
 - MAPE: **0.0017**
 - MdAPE: **0.0**

Log Transform
 - R2: **0.9996**
 - MAPE: **0.000089**
 - MdAPE: **0.0**

## Random Forest Regression
Non-Transform
 - R2: **0.9878**
 - MAPE: **0.0013**
 - MdAPE: **0.000011**

Log Transform
 - R2: **0.9997**
 - MAPE: **0.000077**
 - MdAPE: **0.00000058**

## XGBoost
Non-Transform (max_depth=7, learning_rate=0.2, n_estimators=200)
 - R2: **0.964**
 - MAPE: **0.0099**
 - MdAPE: **0.0053**

Log Transform (max_depth=6, learning_rate=0.3, n_estimators=200)
 - R2: **0.999**
 - MAPE: **0.0005**
 - MdAPE: **0.0003**

## Gradient Boosting Regression
Non-Transform (max_depth=3, learning_rate=0.25, n_estimators=200)
 - R2: **0.9928**
 - MAPE: **0.0336**
 - MdAPE: **0.0250**

Log Transform (max_depth=6, learning_rate=0.25, n_estimators=150)
 - R2: **0.9998**
 - MAPE: **0.0004**
 - MdAPE: **0.0003**

## LightGBM
Non-Transform (max_depth=5, learning_rate=0.5, n_estimators=200)
 - R2: **0.9604**
 - MAPE: **0.0305**
 - MdAPE: **0.0207**

Log Transform (max_depth=5, learning_rate=0.5, n_estimators=200)
 - R2: **0.999**
 - MAPE: **0.0008**
 - MdAPE: **0.0004**

## Histogram-based Gradient Boosting Regression
Non-Transform (max_depth=5, learning_rate=0.75)
 - R2: **0.9641**
 - MAPE: **0.0268**
 - MdAPE: **0.0163**

Log Transform (max_depth=6, learning_rate=0.25)
 - R2: **0.9986**
 - MAPE: **0.0009**
 - MdAPE: **0.0005**


