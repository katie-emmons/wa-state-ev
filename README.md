## Electric Vehicle Population Data - 2025 
This dataset from the Washington State Open Data Portal contains information about Battery Electric Vehicles (BEVs) and Plug-in Hybrid Electric Vehicles (PHEVs) that are currently registered through the Washington State Department of Licensing (DOL). Each record represents an individual vehicle and reflects the growing population of electric vehicles in Washington. 

Access & Use Information
* Source: https://data.wa.gov/Transportation/Electric-Vehicle-Population-Data/f6w7-q2d2/about_data
* Public: This dataset is intended for public access and use.
* License: Open Data Commons Open Database License (ODbL) v1.0

The goal of this notebook is to explore the Washington state EV dataset using Vega-Altair, a Python library for creating interactive visualizations: https://altair-viz.github.io/

You can view the entire notebook with interactive visuals on [nbviewer](https://nbviewer.org/github/katie-emmons/wa-state-ev/blob/main/EV%20Data%20Viz.ipynb). 

### Key Findings

**What is the most common EV type?** 

Battery EVs are far more common compared to Plug-in Hybrid EVs, accounting for about 80% of electric vehicles in Washington state. 

![Alt text](https://github.com/katie-emmons/wa-state-ev/blob/main/EV_Types.svg)

**What are the most common EV models?**

The Tesla Model Y is the most common EV model, accounting for about 20% of electric vehicles in Washington. The Tesla Model 3 is a close runner up, accounting for about 15%. The Nissan Leaf accounts for about 5% of Washington state EVs. Other EV models, including the Chevrolet Volt, Kira Niro, and BMW X5, account for about 50% of Washington state EVs. 

![Alt text](https://github.com/katie-emmons/wa-state-ev/blob/main/Top_EV_Models.svg)

**Which WA state counties have the most EVs?**

Perpahs unsurprisingly, the majority of EVs are located in King county, followed by Snohomish and Pierce counties. 

![Alt text](https://github.com/katie-emmons/wa-state-ev/blob/main/EV_Density.svg)





