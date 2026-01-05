# Copilot / AI agent instructions — Chocolate-manufacturing-and-sales-analysis

Purpose: Provide quick, actionable repository-specific guidance so an AI coding agent can be productive right away.

High-level overview
- This repo is a data-analysis project centered on CSV datasets and a Power BI report (`reports/Chooco Choco.pbix`).
- Canonical processed data lives in `cleaned_data/` (star schema): `FactSales.csv` (fact) and `Dim*.csv` (dimensions).
- Raw exports from Power BI are in `raw_data/…/` — **do not** overwrite or replace `cleaned_data/` without an explicit regeneration process.

Key files to inspect
- `cleaned_data/FactSales.csv` — primary fact table (columns include: OrderID, DateKey YYYYMMDD, ProductID, GeoID, ChannelID, PromotionID, Quantity, UnitPrice, NetSales, UnitCost, Profit).
- `cleaned_data/DimProduct.csv` — product metadata (ProductID, ProductName, Category, Subcategory, CocoaPercent, PackSize_g, IsSugarFree, IsOrganic, BasePrice, UnitCost, LaunchDate).
- `cleaned_data/DimDate.csv` — DateKey is in YYYYMMDD and includes fiscal-year label (e.g., `FY2020-21`).
- `reports/Chooco Choco.pbix` — Power BI binary that consumes the `cleaned_data/` files.

Project conventions & patterns
- Naming: Dimension files start with `Dim` and the fact table with `Fact` (e.g., `DimProduct`, `FactSales`). Treat these as a star schema.
- Date keys: `DateKey` is an integer-like YYYYMMDD string; convert to a date type before date operations.
- Boolean flags are stored as `True`/`False` in CSVs (e.g. `IsSugarFree`, `IsOrganic`).
- The PBIX file is a binary artifact — avoid in-place editing without preserving change notes; prefer exporting or documenting changes.

What an agent should do first
1. Inspect `cleaned_data/` to understand schema and pick a small, self-contained change (e.g., add a derived column or a new aggregation).
2. If adding transformation code, create a reproducible script under `scripts/` or a notebook in `notebooks/` that reads `raw_data/` and outputs `cleaned_data/`, and add instructions to `README.md`.
3. Do not change `reports/Chooco Choco.pbix` directly unless necessary; if you must, add a short commit message describing what changed in the PBIX.

Examples (pandas quick-start)
- Load and join fact + product:
```python
import pandas as pd
fact = pd.read_csv('cleaned_data/FactSales.csv')
prod = pd.read_csv('cleaned_data/DimProduct.csv')
df = fact.merge(prod, on='ProductID')
# Top categories by NetSales
print(df.groupby('Category')['NetSales'].sum().sort_values(ascending=False).head())
```

Small, deterministic validations to include with transformation code
- Assert `FactSales['DateKey'].astype(str).str.len().eq(8).all()` (YYYYMMDD length 8).
- Assert all `ProductID` values in `FactSales` are present in `DimProduct`.
- Check no negative `Quantity` values and NetSales >= 0.

Notes about CI / tests
- This repo has no CI or tests configured. If you add transformation code, include small unit tests or data checks (e.g. `tests/test_schema.py`) and document how to run them in `README.md`.

When to ask for clarification
- Ask a human when you need business logic (e.g., how promotions should apply to shipping) or when changing `reports/Chooco Choco.pbix`.

If anything here is missing or unclear, mention which area (data schema, regeneration steps, PBIX handling, or validation) you'd like expanded and I'll iterate.  
