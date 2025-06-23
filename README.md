# Descriptive Project Title i.e. "Cafe Financial Analysis & Insights" 

## Project Overview
We were provided with a dataset by a café owner who wanted help cleaning the data and drawing insights from it. The café is preparing to launch a new set of marketing campaigns and needs to understand patterns in sales behavior. Our goal was to analyze the dataset to uncover:
* The busiest and most profitable times (by month and day of the week)
* Patterns or trends in customer purchases
* Evaluate and Improve Data Quality: Highlight issues in the dataset (e.g. blanks, errors, unknowns) and explain how they might impact business decisions.

This analysis was approached with the mindset that the business owner is not necessarily data-savvy, so our communication and insights had to remain clear, simple, and action-oriented.



## Business Questions 
- What are the peak sales months?
    - We found that **June** was the most profitable month in terms of total revenue
- Are weekends busier than weekdays?
    - Weekdays were busier, with Thursday having the most transactions.
- Any other findings stand out and relevant to the stakeholders?
    - Identified top-performing and low-performing menu items — useful for inventory and promotions.
    - Discovered typical customer traffic patterns and how busy the café gets on average.
    - Noticed that drinks like tea and coffee underperformed compared to expectations — worth exploring customer preferences
- How reliable was the data? What cleaning was needed?
    - The dataset was not fully reliable at the start — many fields were missing, especially in the location and payment method columns. This limited our ability to make certain conclusions, but we did our best to preserve as much information as possible without discarding too much data.
    - Here’s what we did to clean it:
        - Numerical columns (like Quantity, Price Per Unit, and Grand Total) had missing values. In Excel, we used formulas to fill in these missing values wherever possible using logical calculations.
        - Non-standard missing data entries like "blank", "error", and "unknown" were replaced with NaN so they could be properly processed in Python.
        - Some of the Grand Total values were incorrect, so we recalculated them using Quantity × Price in Excel
        - Many data types were incorrect — we fixed this using pd.to_numeric() and astype(str) with pandas in Python



## Data Cleaning Summary 
- Data type corrections made:
    - Ensured all columns had correct types using pandas functions like `pd.to_numeric()` or `astype()` so calculations wouldn't break. 
- Missing data handling strategy:
    - In Python, we replaced all errors, blanks, and unknowns with NaN and ensured the data was in the right format.
    - In Excel: Itzel used Excel’s special filter to highlight missing values and applied forward-fill logic for dates. For example, when blanks were found in the date column, she filled them using the value directly above. Then compared the monthly totals before and after filling to make sure it didn’t skew the data.
        - She also worked on missing item names, product types, and unit prices.
        - Using formulas like `=IF()`, `=ISBLANK()`, and `=VLOOKUP()`, created a lookup table to fill in missing values. For example:
            - If the item name was blank but the price was available, she used `VLOOKUP` to match that price with a known item from the lookup table and filled it in accordingly.
            - <img src="https://github.com/user-attachments/assets/40596272-49c5-4a0d-a2c0-f331c4b9eb2d" alt="vlookup" style="width:450px;" />
        - For unidentified items, she labeled them as `"ITEM"` to allow analysis to continue without dropping the rows.
        - When using Excel formulas like `=WEEKDAY()` or `=MONTH()`, she made sure not to treat blanks as `“1”` (January/Sunday), which could introduce bias. She compared the grand totals of products before and after the fill to ensure consistency.
 
- Duplicate handling:
    - No duplicate rows were found.
- Any columns dropped or imputed (and why)
    - No major columns were dropped. Instead, we imputed a lot of values in Excel to maximize usable data. For example:
        - Only four of the eight items sold had unique prices. We used these prices to help fill in missing Price Per Unit values.
        - When `Quantity`, `Price`, or `Grand Total` was missing, we used available formulas to back-calculate the missing values.
    - **Dropped columns:** We wanted to keep as much data as possible, but there were a few rows that were too broken to use. Instead of deleting everything with missing values, we set a smart rule:
        - We only dropped a row if:
            - It was missing important info like payment method or location, and It had two or more other problems, like:
                - No item name
                - Quantity was zero
                - Price or total was “no value” or blank
        - This way, we only removed data that was clearly unusable — and kept the rest to get the most out of our analysis.
    - **Transaction Date Imputation:** Some transaction dates were missing. Since the missing entries were positioned directly below valid dates and followed a natural order, we used a forward-fill strategy — assigning the most recent valid date to the missing one. This preserved the dataset’s structure and allowed us to analyze sales by time periods without gaps.


## Key Findings
- Summary stats (mean, median, etc.)
- Insights from trends by month/day
- Additional analysis looked at
- Any bonus feature engineering

## Reflections
- Challenges we faced
- Any biases
- What we would do differently next time

## Takeaways & Recommendations 
- What can you tell the cafe owner and why should they care 

## Folder Structure (EXAMPLE ONLY)
```text
.
├── data/
│   ├── raw/
│   └── cleaned/
├── excel/
│   ├── analysis_workbook.xlsx
│   └── cleaning_template.xlsx
├── notebooks/
│   ├── 01_data_cleaning.ipynb
│   ├── 02_eda_insights.ipynb
│   └── README.md
└── slides/
    └── final_presentation.pptx
