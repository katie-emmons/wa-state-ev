## Electric Vehicle Population Data - 2025 
This dataset from the Washington State Open Data Portal contains information about Battery Electric Vehicles (BEVs) and Plug-in Hybrid Electric Vehicles (PHEVs) that are currently registered through the Washington State Department of Licensing (DOL). Each record represents an individual vehicle and reflects the growing population of electric vehicles in Washington. 

Access & Use Information
* Source: https://data.wa.gov/Transportation/Electric-Vehicle-Population-Data/f6w7-q2d2/about_data
* Public: This dataset is intended for public access and use.
* License: Open Data Commons Open Database License (ODbL) v1.0

The goal of this notebook is to explore the Washington State EV dataset using Altair, a Python library for creating interactive visualizations. 

### 1. Import data and packages
```
#!pip install --upgrade altair
#!pip install altair vega_datasets
```
```
import pandas as pd
import altair as alt
from vega_datasets import data
```
```
df = pd.read_csv("Electric_Vehicle_Population_Data_20250825.csv")
```
### 2. Explore the dataset
```
print("Number of records: ", len(df))
```
```
df.head()
```
### 3. Define Questions
**Question 1: What is the most common EV type?**
```
summary = df['Electric Vehicle Type'].value_counts().to_frame(name='Count').reset_index()
```
```
categorical = 'set2'
sequential = 'blues'
```
```
alt.Chart(summary).mark_bar().encode(
    y = 'Electric Vehicle Type',
    x = 'Count', 
    color = alt.Color('Electric Vehicle Type', scale = alt.Scale(range = ['#a6d854', '#8da0cb'])), 
    tooltip = ['Count']
)
```
()
