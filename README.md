# EAC-s-Economic-trends-Visualizations

This project cleans and transforms economic indicator data from FAOSTAT for visual analysis in Power BI. The focus is on GDP, GNI, and Manufacturing/Agriculture-related metrics across multiple countries, including Rwanda, Kenya, Uganda, Burundi, South Africa, and Singapore.

 Dataset
- Source: FAOSTAT
- Original File: `FAOSTAT_data_en_8-3-2025.csv`
- Cleaned Output: `FAOSTAT_cleaned.csv`

 Data Cleaning & Transformation

We filtered the dataset to retain only relevant `Item` and `Element` combinations, reshaped it for analysis, and exported it to a Power BI-ready format.

 Filtered Elements:
- Value US$
- Value US$ per capita
- Annual growth US$, 2015 prices
- Annual growth US$ per capita
- Ratio of Value Added (Agriculture, Forestry and Fishing) US$
- Share of Value Added (Total Manufacturing) US$

  Filtered Items:
- Gross Domestic Product
- Gross National Income
- Gross Output (Agriculture)
- Value Added (Agriculture)
- Value Added (Agriculture, Forestry and Fishing)
- Value Added (Manufacture of food and beverages)
- Value Added (Total Manufacturing)

 Python Code Used

```python
import pandas as pd
df = pd.read_csv("FAOSTAT_data_en_8-3-2025.csv")

elements_to_keep = [
    'Value US$',
    'Value US$ per capita',
    'Annual growth US$, 2015 prices',
    'Annual growth US$ per capita',
    'Ratio of Value Added (Agriculture, Forestry and Fishing) US$',
    'Share of Value Added (Total Manufacturing) US$'
]
items_to_keep = [
    'Gross Domestic Product',
    'Gross National Income',
    'Gross Output (Agriculture)',
    'Value Added (Agriculture)',
    'Value Added (Agriculture, Forestry and Fishing)',
    'Value Added (Manufacture of food and beverages)',
    'Value Added (Total Manufacturing)'
]

df_filtered = df[df['Element'].isin(elements_to_keep) & df['Item'].isin(items_to_keep)].copy()

df_filtered = df_filtered[['Area', 'Element', 'Item', 'Year', 'Value']]

df_filtered['Metric'] = df_filtered['Area'] + ' - ' + df_filtered['Item'] + ' - ' + df_filtered['Element']

df_pivoted = df_filtered.pivot_table(index='Year', columns='Metric', values='Value', aggfunc='first')


df_pivoted = df_pivoted.reset_index()
df_pivoted.columns.name = None
df_pivoted.to_csv("FAOSTAT_cleaned.csv", index=False)


