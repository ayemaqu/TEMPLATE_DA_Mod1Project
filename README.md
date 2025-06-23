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
### Summary stats (mean, median, etc.)
- _Monthly Revenue Trends_ (Using Excel)
  - This table summarizes monthly revenue using Excel functions. To calculate how much revenue was generated each month in 2023, Itzel used the `=MONTH()` function to extract month numbers from transaction dates.
 
    - <img src="https://github.com/user-attachments/assets/8c78e6ae-afee-48bd-80c3-4ea4be47004d" alt="table1" style="width:800px;" />

      - This allowed the use of `=SUMIFS()` to total all revenue for each month.
      - Once each month's total was calculated, she used =`SUM()` to find the total yearly revenue, and functions like =MAX() and =MIN() to identify the highest and lowest revenue months.
- The graph below shows the most profitable month: June, Least profitable month: February. The bar graph visualizes these totals clearly, making it easy to spot strong and weak months at a glance
        - <img src="https://github.com/user-attachments/assets/35132d1d-edb8-4b19-b836-8c20d45887da" alt="table1" style="width:800px;" />

### Insights from trends by month/day
- The table below shows trends by month and day
    - <img src="https://github.com/user-attachments/assets/451b1194-62be-4408-a662-6d427d8974da" alt="table2" style="width:900;" />
    
    - This chart displays the total revenue generated for each day of the week across the entire year of 2023. To calculate this, we used Excel’s =WEEKDAY() function to convert each date into a number from 1 to 7, where 1 = Sunday and 7 = Saturday. This allowed us to categorize each transaction by day of the week.
    - To find the total amount spent on each specific day, we used the `=SUMIFS()` function. For example, `=SUMIFS([Grand Total], [Weekday Column], 2)` would return the total for Monday (where 2 represents Monday).
    - We extended this method by adding extra criteria to calculate how much revenue each individual item brought in on each weekday or weekend, allowing us to understand sales patterns more deeply.
        - Finally, we used `=SUM()` to group and compare weekdays versus weekends:
            - Weekday Total =`=SUM(Totals for Days 2–6)`
            - Weekend Total = `=SUM(Totals for Days 1 and 7)`
        - One thing to note: in our dataset, there’s a column labeled “Item” that includes some rows with mystery products — sales that occurred but don’t have a clear item name. These still contributed to the café’s revenue, so we chose to keep them in the analysis to maintain the full picture of weekly performance. The table helps us spot when and how often these unidentified items were sold.
        - Ultimately, this breakdown revealed that Thursday brought in the highest total revenue, making it the most profitable day of the week. This insight can help inform strategy, such as when to run promotions or introduce new menu items.
 
    - Using Excel, we created this PivotTable below to determine which month generated the most revenue — which turned out to be June. We chose this method because it allows us to clearly see not only total revenue per month, but also how much each individual item contributed to that total. This makes it easier to identify which products brought in the most income.
        - <img src="https://github.com/user-attachments/assets/fb66fc97-676e-4253-a953-813778c11613" alt="table2" style="width:900;" />

        - We also included a column labeled **“Mystery Item”** — even though we don’t know exactly what product it represents, we noticed that it still brought in revenue during specific months. Rather than drop this data, we decided to keep it because it still contributes to the overall profit and supports our goal of identifying the most profitable month.


### Additional analysis looked at
- From this visualization below, we can clearly see how much profit was generated each month. The line chart highlights monthly trends and fluctuations in total revenue, making it easier to spot both the strongest and weakest periods of the year. One key insight is that **June is the most profitable month**, but there are also noticeable dips in revenue during several points in the year — most significantly in February, which had the lowest revenue, suggesting a possible loss in customer traffic. Smaller dips also occur between **March and May**, **June to July**, **August to September**, and **October to November**.
    - <img src="https://github.com/user-attachments/assets/c5da7144-b230-4c1b-8072-d6722e4d3bb4" alt="im so hungry" style="width:900;" />

    - These recurring declines suggest opportunities for improvement. To address these slower periods, the business could consider seasonal promotions, bundled deals, or targeted marketing campaigns, especially through social media. Engaging customers during these down months may help stabilize revenue and attract repeat visitors. On the flip side, the business should continue doing what worked well during the high-performing months like June — whether that was strong marketing, product launches, or events — and try to replicate or build on that success.


- This table below was made to analyze weekly sales trends throughout 2023, we created a structure that grouped transaction data into weekly intervals.
    - <img src="https://github.com/user-attachments/assets/7b9265e9-7602-4278-a8b5-e919244507ab" alt="food" style="width:900;" />
        
        - We began with the start date of January 1, 2023, and incremented by 7-day intervals to define each week up to December 31, 2023.
        - Using Excel, we applied the `=SUMIFS()` function to calculate the total quantity of items sold within each week. A typical formula looked like:
            - `=SUMIFS(Quantity, Date, “>=”&Start_Date, Date, “<”&End_Date)`
                - The `>=` operator includes all values from the start date, while `<` ensures the end date is not included—effectively capturing a full 7-day window.
                - The & operator links date values from cells into the formula criteria.
            - This logic was repeated across the dataset to compute weekly totals. We also layered in additional conditions (e.g., item name) to understand how much of each product was sold weekly, which helped us track product demand over time.
 
- The graph provides a visual breakdown of total spending across each day of the week in 2023. Each column represents a day (Sunday through Saturday), while the different shades of purple indicate the contribution of each product category (e.g., cookies, salad, tea) to that day’s total revenue.
    - <img src="https://github.com/user-attachments/assets/9c77ffb4-ba06-473e-9112-3a0431852ced" alt="yum" style="width:900;" />

    - The graph provides a visual breakdown of total spending across each day of the week in 2023. Each column represents a day (Sunday through Saturday), while the different shades of purple indicate the contribution of each product category (e.g., cookies, salad, tea) to that day’s total revenue.
        - Legend: The legend at the top shows which shade corresponds to which item.
        - Labels: Each segment is labeled with the exact dollar amount, providing clear visibility into how much each item contributed per day.
    - To generate this visualization:
        - used the `=WEEKDAY()` function in Excel to convert each transaction date into a weekday index (1 = Sunday, 7 = Saturday).
        - Then, we applied `=SUMIFS()` to calculate the total revenue for each item by day of the week. The general formula was: `=SUMIFS(Grand_Total, Weekday_Column, [Specific_Day])`
            - This allowed us to not only compute the grand total per weekday but also break it down by individual item type.
            - We used additional criteria within `SUMIFS()` to filter by both item and weekday, enabling us to compare weekend vs. weekday performance.
            - For example:
                - `=SUM(Weekday cells 2–6)` gives the total for weekdays.
                - `=SUM(Weekday cells 1 and 7)` gives the total for weekends.

    - This visualization helped identify which days generated the highest revenue and which items performed best on those days — valuable insights for scheduling, promotions, and inventory planning.


### Any bonus feature engineering
- Excel and Python Integration: We used both Excel and Python to engineer new columns that allowed for deeper analysis. One example includes extracting the day of the week from each transaction, and labeling whether it occurred on a weekday or weekend. This helps us identify customer behavior patterns across the week.
- **Location Flagging**: We flagged the location column to mark rows with values like `"UNKNOWN"` or `"ERROR"`. Flagging means we didn’t remove or change any data — we simply created a new column to label problematic or missing entries, making them easier to track and filter during analysis.
- **Month Extraction**: We created a new column by extracting the month from the `Transaction Date` field. This was done in both numeric format (1–12) and text format (e.g., January, February).
    - ✅ Feature Created: `Month` or `Month_Name`
    - ✅ Helps With: Monthly trend analysis, seasonal patterns, and comparing profitability across months



## Reflections
### Challenges we faced
  
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
