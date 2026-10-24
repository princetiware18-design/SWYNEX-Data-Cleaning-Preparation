# SWYNEX Data Cleaning & Preparation

Task 1 of my SWYNEX Technologies data internship: cleaning a raw, messy dataset and preparing it for analysis.

## Dataset
Dirty Cafe Sales dataset (Kaggle), 10,000 transactions × 8 columns.

## Problems found
- All 8 columns were stored as text (object) because "ERROR" and "UNKNOWN" values were mixed in
- Hidden missing values: "ERROR"/"UNKNOWN" placeholders were not detected by `isnull()`, so the real missing counts were far higher than first reported
- Missing values in every column except Transaction ID
- No duplicate rows and no duplicate Transaction IDs

## Cleaning steps
1. Replaced "ERROR" and "UNKNOWN" with real missing values
2. Converted Quantity, Price Per Unit and Total Spent to numeric, and Transaction Date to datetime
3. Recovered missing numbers using Total Spent = Quantity × Price Per Unit
4. Recovered missing Item names where the price belonged to only one item (Cookie 1.0, Tea 1.5, Coffee 2.0, Salad 5.0); shared prices were not guessed
5. Labelled unrecoverable Item, Payment Method and Location values as "Unknown" instead of inventing data
6. Dropped 23 rows with no Total Spent (unusable for revenue analysis)
7. Kept 460 rows with missing dates and left the date blank, because a date cannot be calculated from other columns

## Validation
- Total mismatches (Quantity × Price ≠ Total): 0
- Fractional quantities: 0
- Duplicate Transaction IDs: 0

## Results

| Column | Missing before | Missing after |
|---|---|---|
| Item | 969 | 477 |
| Quantity | 479 | 3 |
| Price Per Unit | 533 | 3 |
| Total Spent | 502 | 0 |
| Payment Method | 3,178 | 3,169 |
| Location | 3,961 | 3,953 |
| Transaction Date | 460 | 460 |

Rows before: 10,000 | Rows after: 9,977 | Rows dropped: 23

## Key learnings
- Missing values can hide behind placeholder text, so always inspect with `value_counts()`
- Business logic (Total = Quantity × Price) can recover data without guessing
- Unrecoverable data should be flagged honestly, not invented

## Tools
Python, Pandas, Google Colab

## Files
- `raw_data.csv`: original dataset
- `cleaned_data.csv`: cleaned dataset
- `cleaning.ipynb`: full cleaning notebook
