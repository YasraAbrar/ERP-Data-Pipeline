# Retail ERP Data Migration & Cleaning Pipeline (Odoo)

## Project Overview
Migrating legacy retail data into a modern ERP system (Odoo) requires strict formatting, hierarchical product matrices, and pristine data quality. In this project, I built an automated Python pipeline using **Pandas** and **Regex** to clean, extract, and restructure over 5,700 rows of chaotic, free-text inventory data into a flawless, import-ready Odoo ERP template.

## The Business Problem
The raw data export from the legacy system was highly unstructured:
* **Hidden Attributes:** Metrics like Weight, Capacity, and Dimensions were buried inside the free-text Item Names (e.g., `"Dog Food Adult 2.5 KG"`).
* **Mashed Columns:** Staff routinely entered multiple attributes into single columns (e.g., typing `"1000Ml - L - 25,8X25,8X5,1Cm Black"` entirely inside the "Size" column).
* **Inconsistent Naming:** Colors and sizes had dozens of variations (`"MIX"`, `"Mixed"`, `"Multi"`, `"Mix Color"`).
* **Cartesian Explosions:** Odoo requires a strict Parent/Child matrix hierarchy. Simply importing the raw data would have created thousands of duplicate variations and crashed the ERP inventory logic.

## The Python Solution
I engineered an end-to-end data pipeline to automatically parse and format the data:
1. **Aggressive Regex Parsing:** Created custom Regular Expressions to scan every string, intelligently ripping out Capacities (`ML/L`), Dimensions (`CM/IN`), Weights (`KG/G`), Sizes, and Colors, placing them into dedicated buckets.
2. **Data Unification:** Built synonym dictionaries to instantly convert messy variations (e.g., `"BROWN/COFFEE"`) into a single, clean standard (`"Brown"`). Also enforced strict uniform spacing (e.g., converting `"1.5L"` to `"1.5 L"`).
3. **Odoo Matrix Generation:** Programmed grouping logic to identify products with multiple variants, automatically designating the first item as the "Parent" row (containing the global rules like Product Type, Description, and Category) while formatting the remaining items as "Child" variants (containing only Barcodes and Prices).
4. **Multi-Tab Excel Output:** Automated the export to generate both the `Template for products` and the required `Product Attributes` definitions tab in a single `.xlsx` file.
<img width="841" height="243" alt="Screenshot 2026-05-17 at 10 29 22" src="https://github.com/user-attachments/assets/fdf3401a-a257-4efa-a15f-d4450622ccad" />
<img width="810" height="166" alt="Screenshot 2026-05-17 at 10 28 22" src="https://github.com/user-attachments/assets/5ff92aa8-7765-419e-a51f-25a4612839de" />


## Tech Stack
* **Language:** Python
* **Libraries:** Pandas, NumPy, Re (Regular Expressions), OpenPyXL
* **Concepts:** Data Cleaning, Feature Extraction, String Manipulation, ERP System Architecture, Parent/Child Hierarchies.

## How to Run
1. Clone the repository.
2. Ensure `sample_raw_data.xlsx` is in the same directory.
3. Run the Jupyter Notebook. The script will output a perfectly formatted `FINAL_Odoo_Import_Ready.xlsx` file.
