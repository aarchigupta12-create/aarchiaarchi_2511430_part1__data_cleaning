# aarchiaarchi_2511430_part1__data_cleaning
# Part 1 — Data Cleaning: Raw Orders Dataset

## Business Problem
The company's order export (`raw_orders.csv`) is unreliable for reporting due to
inconsistent formatting, missing fields, and duplicate/conflicting records.
This project cleans the dataset into an analysis-ready form.

## Dataset Used
`data/raw_orders.csv` — 932 orders, 21 columns (order details, customer, product,
sales, cost, profit, payment and order status).

## Tools Used
 Google Colab

## Steps Performed
1. Audited the raw data for missing values, duplicates, and inconsistent formatting.
2. Standardized text fields (`segment`, `region`, `category`, `payment_status`,
   `order_status`, `ship_mode`) — trimmed whitespace, collapsed double spaces,
   normalized casing.
3. Parsed `order_date` and `ship_date`, which mixed 4 different date formats,
   into a single consistent `YYYY-MM-DD` format.
4. Filled missing `region` values using each order's `state` (states map 1:1 to
   a region elsewhere in the data).
5. Filled missing `ship_mode` with `"Unknown"` rather than guessing.
6. Converted `discount` to numeric, filled missing values with 0, and capped
   values to a valid 0–25% range (negative and >25% values were data errors).
7. Removed 20 exact duplicate rows and 12 additional rows sharing an `order_id`
   with a conflicting record (kept first occurrence).
8. Cross-checked `sales` against `quantity × unit_price × (1 − discount)` and
   flagged mismatches instead of overwriting reported sales.

## Key Outputs
- `data/cleaned_orders.csv` — 900 rows, 0 missing values
- Screenshots of before/after audits in `screenshots/`

## Business Insights
- [Fill in after you explore cleaned_orders.csv, e.g. which region/category
  has highest sales or profit — this should come from YOUR analysis]

## Assumptions Made
- Slash-formatted dates (`MM/DD/YYYY`) follow US convention; dash-formatted
  dates (`DD-MM-YYYY`) follow day-first convention. Confirmed by checking
  which position never exceeds 12.
- Missing discount = 0% (no discount applied).
- Discounts outside 0–25% are entry errors and were capped, not deleted.

## Known Limitations
- 22 rows have `ship_date` earlier than `order_date`; root cause unclear,
  left in dataset but flagged.
- 44 rows have a `sales` value that doesn't match the unit-price/discount
  formula; flagged via `sales_mismatch` column rather than corrected, since
  the original error source (price, quantity, or sales field) is unknown.
- When duplicate `order_id`s had conflicting values, the first occurrence
  was kept; the alternate version was discarded rather than reconciled.

## Screenshots
See `screenshots/` folder for the data audit output, cleaning steps, and
final validation of `cleaned_orders.csv`.
