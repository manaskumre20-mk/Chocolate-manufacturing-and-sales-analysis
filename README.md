# Chocolate-manufacturing-and-sales-analysis

A small, focused data analysis project that explores chocolate manufacturing and retail sales. The repository contains cleaned CSV star-schema datasets (fact + dimensions) exported from Power BI and a Power BI report (`reports/Chooco Choco.pbix`) that visualizes the cleaned data.

## Table of Contents
- Introduction
- Dataset / Inputs
- Tech Stack
- Installation
- Usage
- Analysis & Business Questions
- Results / Insights
- Project Structure
- Contributing
- License
- Contact

## Introduction
This repository houses cleaned sales and product data for a chocolate manufacturer. It is intended for exploratory data analysis, reproducible transformations (scripts/notebooks), and Power BI reporting. The canonical data is stored in `cleaned_data/` and is consumed by the Power BI report inside `reports/`.

## Dataset / Inputs
- cleaned_data/ (canonical, processed CSVs)
  - `FactSales.csv` — primary fact table with columns such as: OrderID, DateKey (YYYYMMDD), ProductID, GeoID, ChannelID, PromotionID, Quantity, UnitPrice, NetSales, UnitCost, Profit.
  - `DimProduct.csv`, `DimDate.csv`, `DimChannel.csv`, `DimGeography.csv`, `DimPromotion.csv` — dimension tables (star schema).
- raw_data/ — original Power BI CSV exports. Do not overwrite `cleaned_data/` without explicit regeneration steps.
- reports/Chooco Choco.pbix — Power BI report built on the `cleaned_data/` files.

Important data notes
- DateKey is a YYYYMMDD integer-like string. Convert to a date before doing time-series operations.
- Boolean flags are stored as `True`/`False` strings in the CSVs (for example: `IsSugarFree`, `IsOrganic`).
- Treat the CSVs as authoritative for analyses unless you have a documented regeneration procedure.

## Tech Stack
- Data processing: Python (pandas)
- Interactive analysis: Jupyter / VS Code notebooks
- Visualization & reporting: Power BI (`.pbix`)
- File format: CSV (UTF-8)

(Replace or extend these with exact tools/versions in `requirements.txt` or a `setup` section if you add them.)

## Installation
Prerequisites
- Python 3.8+ installed
- Optional: Power BI Desktop (to open `.pbix` files)

Quick setup (example)
```bash
python -m venv .venv
# Windows
.venv\\Scripts\\activate
# macOS / Linux
# source .venv/bin/activate

pip install --upgrade pip
# Install common analysis packages (create a real requirements.txt for reproducible installs)
pip install pandas jupyter matplotlib seaborn
```

If you add `requirements.txt`, replace the above with:
```bash
pip install -r requirements.txt
```

## Usage
Exploratory analysis
- Open `notebooks/` (if present) in Jupyter or VS Code and run cells.
- Quick pandas example:
```python
import pandas as pd
fact = pd.read_csv('cleaned_data/FactSales.csv')
prod = pd.read_csv('cleaned_data/DimProduct.csv')
df = fact.merge(prod, on='ProductID')
print(df.groupby('Category')['NetSales'].sum().sort_values(ascending=False).head())
```

Regenerating `cleaned_data` (placeholder)
- If you add transformation scripts, put them under `scripts/` and document commands here:
```bash
python scripts/regenerate_cleaned_data.py --input raw_data/ --output cleaned_data/
```
Only run such scripts if you understand the transformation logic and validations.

Running the Power BI report
- Open `reports/Chooco Choco.pbix` in Power BI Desktop. It expects `cleaned_data/` CSV files in the repository root or the configured data source path.

## Analysis / Business Questions
Example analyses that fit this dataset:
- What are top-selling products by net sales and profit margin?
- How do promotions affect sales volumes and profit by season?
- Channel and geography performance: which channels/regions have the highest average order value or growth?
- Product-level lifecycle: which products show sustained growth or decline since launch?

Document the specific business questions you investigate in notebooks or BI report pages.

## Results / Insights (How to present)
- Save reproducible analysis in `notebooks/` with supporting code.
- Export charts or tables to `results/` as static files (PNG/CSV) for sharing.
- For PBIX changes, add a short note in `reports/CHANGELOG.md` (recommended) describing what changed and why.

## Project Structure
A quick overview of current files/folders:
```
LICENSE
README.md
cleaned_data/
  DimChannel.csv
  DimDate.csv
  DimGeography.csv
  DimProduct.csv
  DimPromotion.csv
  FactSales.csv
raw_data/
  <original Power BI CSV exports>
reports/
  Chooco Choco.pbix
results/
```

If you add code, follow this layout:
- `scripts/` — reusable transformation or export scripts (CLI friendly)
- `notebooks/` — exploratory analysis (Jupyter)
- `tests/` — data validation tests (pytest or simple assertions)
- `docs/` — any documentation or design notes

## Contributing
- Keep changes small and documented.
- Prefer adding scripts or notebooks rather than modifying `cleaned_data/` directly.
- If you change `reports/Chooco Choco.pbix`, add an entry to `reports/CHANGELOG.md` describing the change and why it was needed.
- Add deterministic checks for data transforms:
  - DateKey length: `FactSales['DateKey'].astype(str).str.len().eq(8).all()`
  - Integrity: all `ProductID` in `FactSales` must appear in `DimProduct`
  - Basic sanity: no negative `Quantity`, `NetSales` >= 0

## License
This repository includes a `LICENSE` file. Follow the license terms included there.

## Contact
For questions or clarification, open an issue on this repository or contact the maintainer:

- Email: `manaskumre20@gmail.com`
- GitHub: `@manaskumre20-mk`

---

If you'd like, I can:
- Add a `requirements.txt` and an example `scripts/regenerate_cleaned_data.py` with deterministic checks, or
- Create an example notebook with the core analyses and charts referenced above.