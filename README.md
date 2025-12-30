Nashville Housing Data Cleaning Project
Project Overview
This project involves a series of SQL queries designed to clean and prepare a dataset containing housing information for Nashville, TN. The goal was to take raw data and transform it into a format suitable for visualization or further statistical analysis by improving data integrity and usability.

Data Cleaning Steps Applied
1. Standardizing Date Formats
The original SaleDate included timestamp information that wasn't necessary.

Action: Converted the datetime format to a standard Date format.

Method: Added a new column SaleDateConverted and used CONVERT(date, SaleDate) to populate it.

2. Populating Property Address Data
Some records had missing (NULL) PropertyAddress values, but shared a ParcelID with other records that did have an address.

Action: Used a Self-Join to match records with the same ParcelID but different UniqueIDs.

Method: Applied the ISNULL function to fill the empty address fields with the values from their matching ParcelID counterparts.

3. Breaking Out Addresses (Street, City, State)
The address fields were stored as long strings (e.g., "123 Main St, Nashville, TN"). To make this data useful for geographic analysis, it needed to be separated.

Property Address: Split using SUBSTRING and CHARINDEX.

Owner Address: Split using PARSENAME (after replacing commas with periods), which is a more efficient way to handle delimited strings.

4. Standardizing "Sold as Vacant" Field
The SoldAsVacant column contained inconsistent entries: "Y", "N", "Yes", and "No".

Action: Standardized all entries to "Yes" and "No".

Method: Utilized a CASE statement to replace "Y" and "N" globally across the dataset.

5. Removing Duplicates
Duplicate entries can skew analysis. I identified duplicates based on rows having identical ParcelID, PropertyAddress, SalePrice, SaleDate, and LegalReference.

Action: Used a CTE (Common Table Expression) and the ROW_NUMBER() window function.

Method: Partitioned the data by key columns and filtered for any row_num > 1 to identify and remove redundant records.

6. Deleting Unused Columns
To streamline the table and save space, I removed the original uncleaned columns that were no longer needed after the transformations.

Action: Dropped OwnerAddress, TaxDistrict, PropertyAddress, and the original SaleDate.

Tools Used
SQL Server (T-SQL): Main engine for data manipulation.

Window Functions: ROW_NUMBER() for duplicate identification.

String Manipulation: SUBSTRING, CHARINDEX, PARSENAME, REPLACE.

DML/DDL: ALTER TABLE, UPDATE, JOIN.
