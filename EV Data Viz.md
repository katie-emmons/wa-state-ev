## Electric Vehicle Population Data - 2025 
This dataset from the Washington State Open Data Portal contains information about Battery Electric Vehicles (BEVs) and Plug-in Hybrid Electric Vehicles (PHEVs) that are currently registered through the Washington State Department of Licensing (DOL). Each record represents an individual vehicle and reflects the growing population of electric vehicles in Washington. 

Access & Use Information
* Source: https://data.wa.gov/Transportation/Electric-Vehicle-Population-Data/f6w7-q2d2/about_data
* Public: This dataset is intended for public access and use.
* License: Open Data Commons Open Database License (ODbL) v1.0

The goal of this notebook is to explore the Washington State EV dataset using Altair, a Python library for creating interactive visualizations. 

### 1. Import data and packages


```python
#!pip install --upgrade altair
#!pip install altair vega_datasets
```


```python
import pandas as pd
import altair as alt
from vega_datasets import data

alt.data_transformers.disable_max_rows()
```




    DataTransformerRegistry.enable('default')




```python
df = pd.read_csv("Electric_Vehicle_Population_Data_20250825.csv")
```

### 2. Explore the dataset


```python
print("Number of records: ", len(df))
```

    Number of records:  257635



```python
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>VIN (1-10)</th>
      <th>County</th>
      <th>City</th>
      <th>State</th>
      <th>Postal Code</th>
      <th>Model Year</th>
      <th>Make</th>
      <th>Model</th>
      <th>Electric Vehicle Type</th>
      <th>Clean Alternative Fuel Vehicle (CAFV) Eligibility</th>
      <th>Electric Range</th>
      <th>Base MSRP</th>
      <th>Legislative District</th>
      <th>DOL Vehicle ID</th>
      <th>Vehicle Location</th>
      <th>Electric Utility</th>
      <th>2020 Census Tract</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5YJ3E1EB5K</td>
      <td>Yakima</td>
      <td>Yakima</td>
      <td>WA</td>
      <td>98901.0</td>
      <td>2019</td>
      <td>TESLA</td>
      <td>MODEL 3</td>
      <td>Battery Electric Vehicle (BEV)</td>
      <td>Clean Alternative Fuel Vehicle Eligible</td>
      <td>220.0</td>
      <td>0.0</td>
      <td>15.0</td>
      <td>347724772</td>
      <td>POINT (-120.50729 46.60464)</td>
      <td>PACIFICORP</td>
      <td>5.307700e+10</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1C4RJXU67R</td>
      <td>Kitsap</td>
      <td>Port Orchard</td>
      <td>WA</td>
      <td>98367.0</td>
      <td>2024</td>
      <td>JEEP</td>
      <td>WRANGLER</td>
      <td>Plug-in Hybrid Electric Vehicle (PHEV)</td>
      <td>Not eligible due to low battery range</td>
      <td>21.0</td>
      <td>0.0</td>
      <td>35.0</td>
      <td>272165288</td>
      <td>POINT (-122.68471 47.50524)</td>
      <td>PUGET SOUND ENERGY INC</td>
      <td>5.303509e+10</td>
    </tr>
    <tr>
      <th>2</th>
      <td>KNDCD3LD0N</td>
      <td>Snohomish</td>
      <td>Lynnwood</td>
      <td>WA</td>
      <td>98036.0</td>
      <td>2022</td>
      <td>KIA</td>
      <td>NIRO</td>
      <td>Plug-in Hybrid Electric Vehicle (PHEV)</td>
      <td>Not eligible due to low battery range</td>
      <td>26.0</td>
      <td>0.0</td>
      <td>32.0</td>
      <td>203182584</td>
      <td>POINT (-122.29245 47.82557)</td>
      <td>PUGET SOUND ENERGY INC</td>
      <td>5.306105e+10</td>
    </tr>
    <tr>
      <th>3</th>
      <td>5UXKT0C37H</td>
      <td>King</td>
      <td>Auburn</td>
      <td>WA</td>
      <td>98001.0</td>
      <td>2017</td>
      <td>BMW</td>
      <td>X5</td>
      <td>Plug-in Hybrid Electric Vehicle (PHEV)</td>
      <td>Not eligible due to low battery range</td>
      <td>14.0</td>
      <td>0.0</td>
      <td>30.0</td>
      <td>349010287</td>
      <td>POINT (-122.23035 47.3074)</td>
      <td>PUGET SOUND ENERGY INC||CITY OF TACOMA - (WA)</td>
      <td>5.303303e+10</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1N4AZ0CP1D</td>
      <td>Skagit</td>
      <td>Mount Vernon</td>
      <td>WA</td>
      <td>98273.0</td>
      <td>2013</td>
      <td>NISSAN</td>
      <td>LEAF</td>
      <td>Battery Electric Vehicle (BEV)</td>
      <td>Clean Alternative Fuel Vehicle Eligible</td>
      <td>75.0</td>
      <td>0.0</td>
      <td>40.0</td>
      <td>131684150</td>
      <td>POINT (-122.33891 48.41644)</td>
      <td>PUGET SOUND ENERGY INC</td>
      <td>5.305795e+10</td>
    </tr>
  </tbody>
</table>
</div>



### 3. Define Questions

**Question 1: What is the most common EV type?**


```python
# Summarize data so it renders quicker
summary = df['Electric Vehicle Type'].value_counts().to_frame(name='Count').reset_index()
```


```python
# Color palette to use throughout 
categorical = 'set2'
sequential = 'blues'
```


```python
# Simple bar chart showing number of BEVs and PHEVs in WA
alt.Chart(summary).mark_bar().encode(
    y = 'Electric Vehicle Type',
    x = 'Count', 
    color = alt.Color('Electric Vehicle Type', scale = alt.Scale(range = ['#a6d854', '#8da0cb'])), 
    tooltip = ['Count']
)
```





<style>
  #altair-viz-c2c3c8901f8b4a5fb56dd951ddbe2288.vega-embed {
    width: 100%;
    display: flex;
  }

  #altair-viz-c2c3c8901f8b4a5fb56dd951ddbe2288.vega-embed details,
  #altair-viz-c2c3c8901f8b4a5fb56dd951ddbe2288.vega-embed details summary {
    position: relative;
  }
</style>
<div id="altair-viz-c2c3c8901f8b4a5fb56dd951ddbe2288"></div>
<script type="text/javascript">
  var VEGA_DEBUG = (typeof VEGA_DEBUG == "undefined") ? {} : VEGA_DEBUG;
  (function(spec, embedOpt){
    let outputDiv = document.currentScript.previousElementSibling;
    if (outputDiv.id !== "altair-viz-c2c3c8901f8b4a5fb56dd951ddbe2288") {
      outputDiv = document.getElementById("altair-viz-c2c3c8901f8b4a5fb56dd951ddbe2288");
    }

    const paths = {
      "vega": "https://cdn.jsdelivr.net/npm/vega@5?noext",
      "vega-lib": "https://cdn.jsdelivr.net/npm/vega-lib?noext",
      "vega-lite": "https://cdn.jsdelivr.net/npm/vega-lite@5.20.1?noext",
      "vega-embed": "https://cdn.jsdelivr.net/npm/vega-embed@6?noext",
    };

    function maybeLoadScript(lib, version) {
      var key = `${lib.replace("-", "")}_version`;
      return (VEGA_DEBUG[key] == version) ?
        Promise.resolve(paths[lib]) :
        new Promise(function(resolve, reject) {
          var s = document.createElement('script');
          document.getElementsByTagName("head")[0].appendChild(s);
          s.async = true;
          s.onload = () => {
            VEGA_DEBUG[key] = version;
            return resolve(paths[lib]);
          };
          s.onerror = () => reject(`Error loading script: ${paths[lib]}`);
          s.src = paths[lib];
        });
    }

    function showError(err) {
      outputDiv.innerHTML = `<div class="error" style="color:red;">${err}</div>`;
      throw err;
    }

    function displayChart(vegaEmbed) {
      vegaEmbed(outputDiv, spec, embedOpt)
        .catch(err => showError(`Javascript Error: ${err.message}<br>This usually means there's a typo in your chart specification. See the javascript console for the full traceback.`));
    }

    if(typeof define === "function" && define.amd) {
      requirejs.config({paths});
      let deps = ["vega-embed"];
      require(deps, displayChart, err => showError(`Error loading script: ${err.message}`));
    } else {
      maybeLoadScript("vega", "5")
        .then(() => maybeLoadScript("vega-lite", "5.20.1"))
        .then(() => maybeLoadScript("vega-embed", "6"))
        .catch(showError)
        .then(() => displayChart(vegaEmbed));
    }
  })({"config": {"view": {"continuousWidth": 300, "continuousHeight": 300}}, "data": {"name": "data-5d88723e2b54bc11d2b7d4b2b738503a"}, "mark": {"type": "bar"}, "encoding": {"color": {"field": "Electric Vehicle Type", "scale": {"range": ["#a6d854", "#8da0cb"]}, "type": "nominal"}, "tooltip": [{"field": "Count", "type": "quantitative"}], "x": {"field": "Count", "type": "quantitative"}, "y": {"field": "Electric Vehicle Type", "type": "nominal"}}, "$schema": "https://vega.github.io/schema/vega-lite/v5.20.1.json", "datasets": {"data-5d88723e2b54bc11d2b7d4b2b738503a": [{"Electric Vehicle Type": "Battery Electric Vehicle (BEV)", "Count": 205095}, {"Electric Vehicle Type": "Plug-in Hybrid Electric Vehicle (PHEV)", "Count": 52540}]}}, {"mode": "vega-lite"});
</script>




```python
# Now lets view the same data, but this time as a stacked bar chart showing the overall percentange of BEVs vs PHEVs
alt.Chart(summary).transform_joinaggregate(
    TotalEVs='sum(Count)',
).transform_calculate(
    PercentOfWAEVs="datum.Count / datum.TotalEVs"
).mark_bar().encode(
    alt.X('PercentOfWAEVs:Q').axis(format='.0%', title = 'WA State EV Types'), 
    color = alt.Color('Electric Vehicle Type', scale = alt.Scale(range = ['#e78ac3', '#8da0cb'])), 
    tooltip = ['Electric Vehicle Type', alt.Tooltip('PercentOfWAEVs:Q', format='.1%', title='Percentage')]
)
```





<style>
  #altair-viz-8124d0678b3249538363dc1fa8a8ae62.vega-embed {
    width: 100%;
    display: flex;
  }

  #altair-viz-8124d0678b3249538363dc1fa8a8ae62.vega-embed details,
  #altair-viz-8124d0678b3249538363dc1fa8a8ae62.vega-embed details summary {
    position: relative;
  }
</style>
<div id="altair-viz-8124d0678b3249538363dc1fa8a8ae62"></div>
<script type="text/javascript">
  var VEGA_DEBUG = (typeof VEGA_DEBUG == "undefined") ? {} : VEGA_DEBUG;
  (function(spec, embedOpt){
    let outputDiv = document.currentScript.previousElementSibling;
    if (outputDiv.id !== "altair-viz-8124d0678b3249538363dc1fa8a8ae62") {
      outputDiv = document.getElementById("altair-viz-8124d0678b3249538363dc1fa8a8ae62");
    }

    const paths = {
      "vega": "https://cdn.jsdelivr.net/npm/vega@5?noext",
      "vega-lib": "https://cdn.jsdelivr.net/npm/vega-lib?noext",
      "vega-lite": "https://cdn.jsdelivr.net/npm/vega-lite@5.20.1?noext",
      "vega-embed": "https://cdn.jsdelivr.net/npm/vega-embed@6?noext",
    };

    function maybeLoadScript(lib, version) {
      var key = `${lib.replace("-", "")}_version`;
      return (VEGA_DEBUG[key] == version) ?
        Promise.resolve(paths[lib]) :
        new Promise(function(resolve, reject) {
          var s = document.createElement('script');
          document.getElementsByTagName("head")[0].appendChild(s);
          s.async = true;
          s.onload = () => {
            VEGA_DEBUG[key] = version;
            return resolve(paths[lib]);
          };
          s.onerror = () => reject(`Error loading script: ${paths[lib]}`);
          s.src = paths[lib];
        });
    }

    function showError(err) {
      outputDiv.innerHTML = `<div class="error" style="color:red;">${err}</div>`;
      throw err;
    }

    function displayChart(vegaEmbed) {
      vegaEmbed(outputDiv, spec, embedOpt)
        .catch(err => showError(`Javascript Error: ${err.message}<br>This usually means there's a typo in your chart specification. See the javascript console for the full traceback.`));
    }

    if(typeof define === "function" && define.amd) {
      requirejs.config({paths});
      let deps = ["vega-embed"];
      require(deps, displayChart, err => showError(`Error loading script: ${err.message}`));
    } else {
      maybeLoadScript("vega", "5")
        .then(() => maybeLoadScript("vega-lite", "5.20.1"))
        .then(() => maybeLoadScript("vega-embed", "6"))
        .catch(showError)
        .then(() => displayChart(vegaEmbed));
    }
  })({"config": {"view": {"continuousWidth": 300, "continuousHeight": 300}}, "data": {"name": "data-5d88723e2b54bc11d2b7d4b2b738503a"}, "mark": {"type": "bar"}, "encoding": {"color": {"field": "Electric Vehicle Type", "scale": {"range": ["#a6d854", "#8da0cb"]}, "type": "nominal"}, "tooltip": [{"field": "Electric Vehicle Type", "type": "nominal"}, {"field": "PercentOfWAEVs", "format": ".1%", "title": "Percentage", "type": "quantitative"}], "x": {"axis": {"format": ".0%", "title": "WA State EV Types"}, "field": "PercentOfWAEVs", "type": "quantitative"}}, "transform": [{"joinaggregate": [{"op": "sum", "field": "Count", "as": "TotalEVs"}]}, {"calculate": "datum.Count / datum.TotalEVs", "as": "PercentOfWAEVs"}], "$schema": "https://vega.github.io/schema/vega-lite/v5.20.1.json", "datasets": {"data-5d88723e2b54bc11d2b7d4b2b738503a": [{"Electric Vehicle Type": "Battery Electric Vehicle (BEV)", "Count": 205095}, {"Electric Vehicle Type": "Plug-in Hybrid Electric Vehicle (PHEV)", "Count": 52540}]}}, {"mode": "vega-lite"});
</script>



As you can see, Battery Electric Vehicles are far more common than Plug-in Hybrid Electric Vehicles, accounting for about 80% of the Electric Vehicles in Washington State

## **Question 2: What are the most common makes and models of EVs?**


```python
## Summarize EVS by make
df['Make'].value_counts()
```




    Make
    TESLA                     107535
    CHEVROLET                  18602
    NISSAN                     16274
    FORD                       13750
    KIA                        12586
    BMW                        10656
    TOYOTA                     10622
    HYUNDAI                     8638
    RIVIAN                      7816
    VOLVO                       6673
    VOLKSWAGEN                  6607
    JEEP                        6599
    AUDI                        5190
    CHRYSLER                    3817
    MERCEDES-BENZ               2918
    HONDA                       2526
    SUBARU                      2473
    PORSCHE                     1772
    CADILLAC                    1596
    POLESTAR                    1453
    MAZDA                       1282
    MINI                        1202
    MITSUBISHI                  1170
    LEXUS                       1165
    FIAT                         850
    DODGE                        759
    LUCID                        477
    GMC                          472
    GENESIS                      433
    LINCOLN                      417
    ACURA                        335
    SMART                        241
    JAGUAR                       209
    LAND ROVER                   185
    FISKER                       144
    ALFA ROMEO                   100
    BRIGHTDROP                    40
    LAMBORGHINI                   13
    RAM                            9
    BENTLEY                        8
    ROLLS-ROYCE                    6
    TH!NK                          5
    AZURE DYNAMICS                 4
    WHEEGO ELECTRIC CARS           2
    MULLEN AUTOMOTIVE INC.         2
    VINFAST                        2
    Name: count, dtype: int64



As you can see, the top brands are Tesla, Chevrolet, and Nissan. Let's look at the top models within each of those brands. 


```python
## Get the top models for Tesla, Chevrolet, and Nissan
tesla = df.loc[df['Make'] == "TESLA"]
tesla_summary = tesla['Model'].value_counts().to_frame(name='Count').reset_index()

chevy = df.loc[df['Make'] == "CHEVROLET"]
chevy_summary = chevy['Model'].value_counts().to_frame(name='Count').reset_index()

nissan = df.loc[df['Make'] == "NISSAN"]
nissan_summary = nissan['Model'].value_counts().to_frame(name='Count').reset_index()
```


```python
# Now lets visualize the top models for Tesla, Chevrolet, and Nissan in a pie chart
tesla_chart = alt.Chart(tesla_summary).mark_arc().encode(
    theta = 'Count',
    color = alt.Color('Model', scale = alt.Scale(scheme = categorical), legend=alt.Legend(orient='left')), 
    tooltip=['Model:N','Count:Q',alt.Tooltip('percentage:Q', format='.1%', title='Percentage')] # Add the formatted percentage to the tooltip
).transform_joinaggregate(
    total='sum(Count)' # Calculate total sum of 'Count'
).transform_calculate(
    percentage='datum.Count / datum.total' # Calculate percentage for each slice
).properties(title = 'Top Tesla EV Models')

chevy_chart = alt.Chart(chevy_summary).mark_arc().encode(
    theta = 'Count',
    color = alt.Color('Model', scale = alt.Scale(scheme = categorical), legend=alt.Legend(orient='left')), 
    tooltip=['Model:N','Count:Q',alt.Tooltip('percentage:Q', format='.1%', title='Percentage')] # Add the formatted percentage to the tooltip
).transform_joinaggregate(
    total='sum(Count)' # Calculate total sum of 'Count'
).transform_calculate(
    percentage='datum.Count / datum.total' # Calculate percentage for each slice
).properties(title = 'Top Chevrolet EV Models')

nissan_chart = alt.Chart(nissan_summary).mark_arc().encode(
    theta = 'Count',
    color = alt.Color('Model', scale = alt.Scale(scheme = categorical), legend=alt.Legend(orient='left')), 
    tooltip=['Model:N','Count:Q',alt.Tooltip('percentage:Q', format='.1%', title='Percentage')] # Add the formatted percentage to the tooltip
).transform_joinaggregate(
    total='sum(Count)' # Calculate total sum of 'Count'
).transform_calculate(
    percentage='datum.Count / datum.total' # Calculate percentage for each slice
).properties(title = 'Top Nissan EV Models')
```


```python
# We can combine all three pie charts into a single plot using hconcat. Hovering over each section of the pie will show the model name, count, and relative percentage
alt.hconcat(tesla_chart, chevy_chart, nissan_chart).resolve_scale(color='independent')
```





<style>
  #altair-viz-3b5a2cdd3dfe48289fc18119d54b0f2b.vega-embed {
    width: 100%;
    display: flex;
  }

  #altair-viz-3b5a2cdd3dfe48289fc18119d54b0f2b.vega-embed details,
  #altair-viz-3b5a2cdd3dfe48289fc18119d54b0f2b.vega-embed details summary {
    position: relative;
  }
</style>
<div id="altair-viz-3b5a2cdd3dfe48289fc18119d54b0f2b"></div>
<script type="text/javascript">
  var VEGA_DEBUG = (typeof VEGA_DEBUG == "undefined") ? {} : VEGA_DEBUG;
  (function(spec, embedOpt){
    let outputDiv = document.currentScript.previousElementSibling;
    if (outputDiv.id !== "altair-viz-3b5a2cdd3dfe48289fc18119d54b0f2b") {
      outputDiv = document.getElementById("altair-viz-3b5a2cdd3dfe48289fc18119d54b0f2b");
    }

    const paths = {
      "vega": "https://cdn.jsdelivr.net/npm/vega@5?noext",
      "vega-lib": "https://cdn.jsdelivr.net/npm/vega-lib?noext",
      "vega-lite": "https://cdn.jsdelivr.net/npm/vega-lite@5.20.1?noext",
      "vega-embed": "https://cdn.jsdelivr.net/npm/vega-embed@6?noext",
    };

    function maybeLoadScript(lib, version) {
      var key = `${lib.replace("-", "")}_version`;
      return (VEGA_DEBUG[key] == version) ?
        Promise.resolve(paths[lib]) :
        new Promise(function(resolve, reject) {
          var s = document.createElement('script');
          document.getElementsByTagName("head")[0].appendChild(s);
          s.async = true;
          s.onload = () => {
            VEGA_DEBUG[key] = version;
            return resolve(paths[lib]);
          };
          s.onerror = () => reject(`Error loading script: ${paths[lib]}`);
          s.src = paths[lib];
        });
    }

    function showError(err) {
      outputDiv.innerHTML = `<div class="error" style="color:red;">${err}</div>`;
      throw err;
    }

    function displayChart(vegaEmbed) {
      vegaEmbed(outputDiv, spec, embedOpt)
        .catch(err => showError(`Javascript Error: ${err.message}<br>This usually means there's a typo in your chart specification. See the javascript console for the full traceback.`));
    }

    if(typeof define === "function" && define.amd) {
      requirejs.config({paths});
      let deps = ["vega-embed"];
      require(deps, displayChart, err => showError(`Error loading script: ${err.message}`));
    } else {
      maybeLoadScript("vega", "5")
        .then(() => maybeLoadScript("vega-lite", "5.20.1"))
        .then(() => maybeLoadScript("vega-embed", "6"))
        .catch(showError)
        .then(() => displayChart(vegaEmbed));
    }
  })({"config": {"view": {"continuousWidth": 300, "continuousHeight": 300}}, "hconcat": [{"data": {"name": "data-95ab7e0c6d761dc43be0cb9623990385"}, "mark": {"type": "arc"}, "encoding": {"color": {"field": "Model", "legend": {"orient": "left"}, "scale": {"scheme": "set2"}, "type": "nominal"}, "theta": {"field": "Count", "type": "quantitative"}, "tooltip": [{"field": "Model", "type": "nominal"}, {"field": "Count", "type": "quantitative"}, {"field": "percentage", "format": ".1%", "title": "Percentage", "type": "quantitative"}]}, "title": "Top Tesla EV Models", "transform": [{"joinaggregate": [{"op": "sum", "field": "Count", "as": "total"}]}, {"calculate": "datum.Count / datum.total", "as": "percentage"}]}, {"data": {"name": "data-10422ed854002d9159a7aee549b3f6ad"}, "mark": {"type": "arc"}, "encoding": {"color": {"field": "Model", "legend": {"orient": "left"}, "scale": {"scheme": "set2"}, "type": "nominal"}, "theta": {"field": "Count", "type": "quantitative"}, "tooltip": [{"field": "Model", "type": "nominal"}, {"field": "Count", "type": "quantitative"}, {"field": "percentage", "format": ".1%", "title": "Percentage", "type": "quantitative"}]}, "title": "Top Chevrolet EV Models", "transform": [{"joinaggregate": [{"op": "sum", "field": "Count", "as": "total"}]}, {"calculate": "datum.Count / datum.total", "as": "percentage"}]}, {"data": {"name": "data-bc7f52bb6c71d1d82cfe2faa71eb4839"}, "mark": {"type": "arc"}, "encoding": {"color": {"field": "Model", "legend": {"orient": "left"}, "scale": {"scheme": "set2"}, "type": "nominal"}, "theta": {"field": "Count", "type": "quantitative"}, "tooltip": [{"field": "Model", "type": "nominal"}, {"field": "Count", "type": "quantitative"}, {"field": "percentage", "format": ".1%", "title": "Percentage", "type": "quantitative"}]}, "title": "Top Nissan EV Models", "transform": [{"joinaggregate": [{"op": "sum", "field": "Count", "as": "total"}]}, {"calculate": "datum.Count / datum.total", "as": "percentage"}]}], "resolve": {"scale": {"color": "independent"}}, "$schema": "https://vega.github.io/schema/vega-lite/v5.20.1.json", "datasets": {"data-95ab7e0c6d761dc43be0cb9623990385": [{"Model": "MODEL Y", "Count": 53560}, {"Model": "MODEL 3", "Count": 37807}, {"Model": "MODEL S", "Count": 7911}, {"Model": "MODEL X", "Count": 6713}, {"Model": "CYBERTRUCK", "Count": 1496}, {"Model": "ROADSTER", "Count": 48}], "data-10422ed854002d9159a7aee549b3f6ad": [{"Model": "BOLT EV", "Count": 7812}, {"Model": "VOLT", "Count": 4634}, {"Model": "BOLT EUV", "Count": 2906}, {"Model": "EQUINOX", "Count": 1367}, {"Model": "BLAZER", "Count": 1208}, {"Model": "SILVERADO", "Count": 435}, {"Model": "SPARK", "Count": 226}, {"Model": "BRIGHTDROP", "Count": 14}], "data-bc7f52bb6c71d1d82cfe2faa71eb4839": [{"Model": "LEAF", "Count": 13971}, {"Model": "ARIYA", "Count": 2303}]}}, {"mode": "vega-lite"});
</script>




```python
# Now lets visualize the same data but in a bar chart
tesla_bar = alt.Chart(tesla_summary).transform_joinaggregate(
    TotalTeslas='sum(Count)',
).transform_calculate(
    PercentOfTeslass="datum.Count / datum.TotalTeslas"
).mark_bar().encode(
    alt.X('PercentOfTeslass:Q').axis(format='.0%', title = 'Tesla Models'), 
    color = alt.Color('Model', scale = alt.Scale(scheme = categorical), legend=alt.Legend(orient='left')), 
    tooltip = ['Model', alt.Tooltip('PercentOfTeslass:Q', format='.1%', title='Percentage')]
)

nissan_bar = alt.Chart(nissan_summary).transform_joinaggregate(
    TotalTeslas='sum(Count)',
).transform_calculate(
    PercentOfTeslass="datum.Count / datum.TotalTeslas"
).mark_bar().encode(
    alt.X('PercentOfTeslass:Q').axis(format='.0%', title = 'Nissan Models'), 
    color = alt.Color('Model', scale = alt.Scale(scheme = categorical), legend=alt.Legend(orient='left')), 
    tooltip = ['Model', alt.Tooltip('PercentOfTeslass:Q', format='.1%', title='Percentage')]
)

chevy_bar = alt.Chart(chevy_summary).transform_joinaggregate(
    TotalTeslas='sum(Count)',
).transform_calculate(
    PercentOfTeslass="datum.Count / datum.TotalTeslas"
).mark_bar().encode(
    alt.X('PercentOfTeslass:Q').axis(format='.0%', title = 'Chevy Models'), 
    color = alt.Color('Model', scale = alt.Scale(scheme = categorical), legend=alt.Legend(orient='left')), 
    tooltip = ['Model', alt.Tooltip('PercentOfTeslass:Q', format='.1%', title='Percentage')]
)
```


```python
# Again, we can combine these plots and look at them side by side. Hovering over the bar will show the model name and percentage. 
alt.hconcat(tesla_bar, chevy_bar, nissan_bar).resolve_scale(color='independent')
```





<style>
  #altair-viz-03f9b64efb87411c82e6aaa8435aaab0.vega-embed {
    width: 100%;
    display: flex;
  }

  #altair-viz-03f9b64efb87411c82e6aaa8435aaab0.vega-embed details,
  #altair-viz-03f9b64efb87411c82e6aaa8435aaab0.vega-embed details summary {
    position: relative;
  }
</style>
<div id="altair-viz-03f9b64efb87411c82e6aaa8435aaab0"></div>
<script type="text/javascript">
  var VEGA_DEBUG = (typeof VEGA_DEBUG == "undefined") ? {} : VEGA_DEBUG;
  (function(spec, embedOpt){
    let outputDiv = document.currentScript.previousElementSibling;
    if (outputDiv.id !== "altair-viz-03f9b64efb87411c82e6aaa8435aaab0") {
      outputDiv = document.getElementById("altair-viz-03f9b64efb87411c82e6aaa8435aaab0");
    }

    const paths = {
      "vega": "https://cdn.jsdelivr.net/npm/vega@5?noext",
      "vega-lib": "https://cdn.jsdelivr.net/npm/vega-lib?noext",
      "vega-lite": "https://cdn.jsdelivr.net/npm/vega-lite@5.20.1?noext",
      "vega-embed": "https://cdn.jsdelivr.net/npm/vega-embed@6?noext",
    };

    function maybeLoadScript(lib, version) {
      var key = `${lib.replace("-", "")}_version`;
      return (VEGA_DEBUG[key] == version) ?
        Promise.resolve(paths[lib]) :
        new Promise(function(resolve, reject) {
          var s = document.createElement('script');
          document.getElementsByTagName("head")[0].appendChild(s);
          s.async = true;
          s.onload = () => {
            VEGA_DEBUG[key] = version;
            return resolve(paths[lib]);
          };
          s.onerror = () => reject(`Error loading script: ${paths[lib]}`);
          s.src = paths[lib];
        });
    }

    function showError(err) {
      outputDiv.innerHTML = `<div class="error" style="color:red;">${err}</div>`;
      throw err;
    }

    function displayChart(vegaEmbed) {
      vegaEmbed(outputDiv, spec, embedOpt)
        .catch(err => showError(`Javascript Error: ${err.message}<br>This usually means there's a typo in your chart specification. See the javascript console for the full traceback.`));
    }

    if(typeof define === "function" && define.amd) {
      requirejs.config({paths});
      let deps = ["vega-embed"];
      require(deps, displayChart, err => showError(`Error loading script: ${err.message}`));
    } else {
      maybeLoadScript("vega", "5")
        .then(() => maybeLoadScript("vega-lite", "5.20.1"))
        .then(() => maybeLoadScript("vega-embed", "6"))
        .catch(showError)
        .then(() => displayChart(vegaEmbed));
    }
  })({"config": {"view": {"continuousWidth": 300, "continuousHeight": 300}}, "hconcat": [{"data": {"name": "data-95ab7e0c6d761dc43be0cb9623990385"}, "mark": {"type": "bar"}, "encoding": {"color": {"field": "Model", "legend": {"orient": "left"}, "scale": {"scheme": "set2"}, "type": "nominal"}, "tooltip": [{"field": "Model", "type": "nominal"}, {"field": "PercentOfTeslass", "format": ".1%", "title": "Percentage", "type": "quantitative"}], "x": {"axis": {"format": ".0%", "title": "Tesla Models"}, "field": "PercentOfTeslass", "type": "quantitative"}}, "transform": [{"joinaggregate": [{"op": "sum", "field": "Count", "as": "TotalTeslas"}]}, {"calculate": "datum.Count / datum.TotalTeslas", "as": "PercentOfTeslass"}]}, {"data": {"name": "data-10422ed854002d9159a7aee549b3f6ad"}, "mark": {"type": "bar"}, "encoding": {"color": {"field": "Model", "legend": {"orient": "left"}, "scale": {"scheme": "set2"}, "type": "nominal"}, "tooltip": [{"field": "Model", "type": "nominal"}, {"field": "PercentOfTeslass", "format": ".1%", "title": "Percentage", "type": "quantitative"}], "x": {"axis": {"format": ".0%", "title": "Chevy Models"}, "field": "PercentOfTeslass", "type": "quantitative"}}, "transform": [{"joinaggregate": [{"op": "sum", "field": "Count", "as": "TotalTeslas"}]}, {"calculate": "datum.Count / datum.TotalTeslas", "as": "PercentOfTeslass"}]}, {"data": {"name": "data-bc7f52bb6c71d1d82cfe2faa71eb4839"}, "mark": {"type": "bar"}, "encoding": {"color": {"field": "Model", "legend": {"orient": "left"}, "scale": {"scheme": "set2"}, "type": "nominal"}, "tooltip": [{"field": "Model", "type": "nominal"}, {"field": "PercentOfTeslass", "format": ".1%", "title": "Percentage", "type": "quantitative"}], "x": {"axis": {"format": ".0%", "title": "Nissan Models"}, "field": "PercentOfTeslass", "type": "quantitative"}}, "transform": [{"joinaggregate": [{"op": "sum", "field": "Count", "as": "TotalTeslas"}]}, {"calculate": "datum.Count / datum.TotalTeslas", "as": "PercentOfTeslass"}]}], "resolve": {"scale": {"color": "independent"}}, "$schema": "https://vega.github.io/schema/vega-lite/v5.20.1.json", "datasets": {"data-95ab7e0c6d761dc43be0cb9623990385": [{"Model": "MODEL Y", "Count": 53560}, {"Model": "MODEL 3", "Count": 37807}, {"Model": "MODEL S", "Count": 7911}, {"Model": "MODEL X", "Count": 6713}, {"Model": "CYBERTRUCK", "Count": 1496}, {"Model": "ROADSTER", "Count": 48}], "data-10422ed854002d9159a7aee549b3f6ad": [{"Model": "BOLT EV", "Count": 7812}, {"Model": "VOLT", "Count": 4634}, {"Model": "BOLT EUV", "Count": 2906}, {"Model": "EQUINOX", "Count": 1367}, {"Model": "BLAZER", "Count": 1208}, {"Model": "SILVERADO", "Count": 435}, {"Model": "SPARK", "Count": 226}, {"Model": "BRIGHTDROP", "Count": 14}], "data-bc7f52bb6c71d1d82cfe2faa71eb4839": [{"Model": "LEAF", "Count": 13971}, {"Model": "ARIYA", "Count": 2303}]}}, {"mode": "vega-lite"});
</script>



As you can see, the top models for Tesla are the Model Y and the Model 3. The top models for Chevrolet are the Bolt EV and the Volt, and the top Nissan model is the Leaf. Finally, lets look at the top models regardless of brand. 


```python
## Summarize EVS by model name
df['Model'].value_counts()
```




    Model
    MODEL Y        53560
    MODEL 3        37807
    LEAF           13971
    MODEL S         7911
    BOLT EV         7812
                   ...  
    VF 8               2
    S6                 2
    SL-CLASS           2
    RCV                1
    CONTINENTAL        1
    Name: count, Length: 179, dtype: int64




```python
## Lets look at the top 5 models, and put everything else in an "other" category
top_5_models = df['Model'].value_counts().nlargest(5)
df['Top_Models'] = df['Model']
df['Top_Models'] = df['Top_Models'].apply(lambda x: x if x in top_5_models else 'Other')
df['Top_Models'].value_counts()
```




    Top_Models
    Other      136574
    MODEL Y     53560
    MODEL 3     37807
    LEAF        13971
    MODEL S      7911
    BOLT EV      7812
    Name: count, dtype: int64




```python
# Create a summary df so visualizations render quicker
model_summary = df['Top_Models'].value_counts().to_frame(name='Count').reset_index()
```


```python
model_summary.columns
```




    Index(['Top_Models', 'Count'], dtype='object')




```python
# Now we can visualize the top models
alt.Chart(model_summary).mark_arc().encode(
    theta = 'Count',
    color = alt.Color('Top_Models', scale = alt.Scale(scheme = categorical), legend = alt.Legend(orient = 'right', title = 'Model')), 
    tooltip = ['Top_Models:N','Count:Q',alt.Tooltip('percentage:Q', format = '.1%', title = 'Percentage')] # Add the formatted percentage to the tooltip
).transform_joinaggregate(
    total = 'sum(Count)' # Calculate total sum of 'Count'
).transform_calculate(
    percentage = 'datum.Count / datum.total' # Calculate percentage for each slice
).properties(title="Top EV Models in WA")
```





<style>
  #altair-viz-256c4d128818439bb8abd4fdbc624e19.vega-embed {
    width: 100%;
    display: flex;
  }

  #altair-viz-256c4d128818439bb8abd4fdbc624e19.vega-embed details,
  #altair-viz-256c4d128818439bb8abd4fdbc624e19.vega-embed details summary {
    position: relative;
  }
</style>
<div id="altair-viz-256c4d128818439bb8abd4fdbc624e19"></div>
<script type="text/javascript">
  var VEGA_DEBUG = (typeof VEGA_DEBUG == "undefined") ? {} : VEGA_DEBUG;
  (function(spec, embedOpt){
    let outputDiv = document.currentScript.previousElementSibling;
    if (outputDiv.id !== "altair-viz-256c4d128818439bb8abd4fdbc624e19") {
      outputDiv = document.getElementById("altair-viz-256c4d128818439bb8abd4fdbc624e19");
    }

    const paths = {
      "vega": "https://cdn.jsdelivr.net/npm/vega@5?noext",
      "vega-lib": "https://cdn.jsdelivr.net/npm/vega-lib?noext",
      "vega-lite": "https://cdn.jsdelivr.net/npm/vega-lite@5.20.1?noext",
      "vega-embed": "https://cdn.jsdelivr.net/npm/vega-embed@6?noext",
    };

    function maybeLoadScript(lib, version) {
      var key = `${lib.replace("-", "")}_version`;
      return (VEGA_DEBUG[key] == version) ?
        Promise.resolve(paths[lib]) :
        new Promise(function(resolve, reject) {
          var s = document.createElement('script');
          document.getElementsByTagName("head")[0].appendChild(s);
          s.async = true;
          s.onload = () => {
            VEGA_DEBUG[key] = version;
            return resolve(paths[lib]);
          };
          s.onerror = () => reject(`Error loading script: ${paths[lib]}`);
          s.src = paths[lib];
        });
    }

    function showError(err) {
      outputDiv.innerHTML = `<div class="error" style="color:red;">${err}</div>`;
      throw err;
    }

    function displayChart(vegaEmbed) {
      vegaEmbed(outputDiv, spec, embedOpt)
        .catch(err => showError(`Javascript Error: ${err.message}<br>This usually means there's a typo in your chart specification. See the javascript console for the full traceback.`));
    }

    if(typeof define === "function" && define.amd) {
      requirejs.config({paths});
      let deps = ["vega-embed"];
      require(deps, displayChart, err => showError(`Error loading script: ${err.message}`));
    } else {
      maybeLoadScript("vega", "5")
        .then(() => maybeLoadScript("vega-lite", "5.20.1"))
        .then(() => maybeLoadScript("vega-embed", "6"))
        .catch(showError)
        .then(() => displayChart(vegaEmbed));
    }
  })({"config": {"view": {"continuousWidth": 300, "continuousHeight": 300}}, "data": {"name": "data-28f8fb4306a59c780b5780a3141604e4"}, "mark": {"type": "arc"}, "encoding": {"color": {"field": "Top_Models", "legend": {"orient": "right", "title": "Model"}, "scale": {"scheme": "set2"}, "type": "nominal"}, "theta": {"field": "Count", "type": "quantitative"}, "tooltip": [{"field": "Top_Models", "type": "nominal"}, {"field": "Count", "type": "quantitative"}, {"field": "percentage", "format": ".1%", "title": "Percentage", "type": "quantitative"}]}, "title": "Top EV Models in WA", "transform": [{"joinaggregate": [{"op": "sum", "field": "Count", "as": "total"}]}, {"calculate": "datum.Count / datum.total", "as": "percentage"}], "$schema": "https://vega.github.io/schema/vega-lite/v5.20.1.json", "datasets": {"data-28f8fb4306a59c780b5780a3141604e4": [{"Top_Models": "Other", "Count": 136574}, {"Top_Models": "MODEL Y", "Count": 53560}, {"Top_Models": "MODEL 3", "Count": 37807}, {"Top_Models": "LEAF", "Count": 13971}, {"Top_Models": "MODEL S", "Count": 7911}, {"Top_Models": "BOLT EV", "Count": 7812}]}}, {"mode": "vega-lite"});
</script>



## **Question 3: Which Washington state counties have the most EVs?**


```python
# summarize EV count by county
# vehicles registered in WA may reside outside of WA, but for the purposes of this analysis, I only want to look at vehicles located in WA
county_summary = df[df['State'] == 'WA']
county_summary = county_summary['County'].value_counts().to_frame(name='Count').reset_index()
```

As you can see, most EVs are registered in King, Snohomish, and Pierce counties. Let's visualize the number of EVs in each county on a map of Washington state. 


```python
# First we need to pull in a map of the US counties from the vega datasets
# Citation: I used ChatGPT to learn out to how to pull in a map of all US counties
counties = alt.topo_feature(data.us_10m.url, 'counties')
```


```python
# The TopoJSON maps counties by FIPS code, whereas the EV dataset refers to county by name. 
# I found a list of all the WA state counties and their FIPS code here: https://unicede.air-worldwide.com/unicede/unicede_washington_fips.html
# Then I created a csv file with the state code, county code, combined code, and county name
FIPS_codes = pd.read_csv('WA_FIPS.csv')
FIPS_codes.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>FIPS State Code</th>
      <th>FIPS County Code</th>
      <th>FIPS Code</th>
      <th>County Name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>53</td>
      <td>1</td>
      <td>53001</td>
      <td>Adams</td>
    </tr>
    <tr>
      <th>1</th>
      <td>53</td>
      <td>3</td>
      <td>53003</td>
      <td>Asotin</td>
    </tr>
    <tr>
      <th>2</th>
      <td>53</td>
      <td>5</td>
      <td>53005</td>
      <td>Benton</td>
    </tr>
    <tr>
      <th>3</th>
      <td>53</td>
      <td>7</td>
      <td>53007</td>
      <td>Chelan</td>
    </tr>
    <tr>
      <th>4</th>
      <td>53</td>
      <td>9</td>
      <td>53009</td>
      <td>Clallam</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Now we can pull in the FIPS Code and add it to the EV dataset
county_summary['FIPS Code'] = county_summary['County'].map(FIPS_codes.set_index("County Name")["FIPS Code"])
```


```python
# Now we create a choropleth map with the number of EVs by county
# Citation: I used ChatGPT to learn out to how to filter for WA state and create the choropleth map
# The Altair documentation is also useful: https://altair-viz.github.io/altair-tutorial/notebooks/09-Geographic-plots.html

alt.Chart(counties).mark_geoshape(
    stroke="white"
).transform_filter(
    "floor(datum.id / 1000) == 53"
).transform_lookup(
    lookup="id",   
    from_=alt.LookupData(county_summary, "FIPS Code", ["County", "Count"])
).encode(
    color=alt.Color("Count:Q", scale=alt.Scale(scheme = sequential)),
    tooltip=["County:N", "Count:Q"]
).project("mercator").properties(
    width=400,
    height=400,
    title="Electric Vehicles by County in Washington"
)
```





<style>
  #altair-viz-f25c55730ed74048818da21aa974e116.vega-embed {
    width: 100%;
    display: flex;
  }

  #altair-viz-f25c55730ed74048818da21aa974e116.vega-embed details,
  #altair-viz-f25c55730ed74048818da21aa974e116.vega-embed details summary {
    position: relative;
  }
</style>
<div id="altair-viz-f25c55730ed74048818da21aa974e116"></div>
<script type="text/javascript">
  var VEGA_DEBUG = (typeof VEGA_DEBUG == "undefined") ? {} : VEGA_DEBUG;
  (function(spec, embedOpt){
    let outputDiv = document.currentScript.previousElementSibling;
    if (outputDiv.id !== "altair-viz-f25c55730ed74048818da21aa974e116") {
      outputDiv = document.getElementById("altair-viz-f25c55730ed74048818da21aa974e116");
    }

    const paths = {
      "vega": "https://cdn.jsdelivr.net/npm/vega@5?noext",
      "vega-lib": "https://cdn.jsdelivr.net/npm/vega-lib?noext",
      "vega-lite": "https://cdn.jsdelivr.net/npm/vega-lite@5.20.1?noext",
      "vega-embed": "https://cdn.jsdelivr.net/npm/vega-embed@6?noext",
    };

    function maybeLoadScript(lib, version) {
      var key = `${lib.replace("-", "")}_version`;
      return (VEGA_DEBUG[key] == version) ?
        Promise.resolve(paths[lib]) :
        new Promise(function(resolve, reject) {
          var s = document.createElement('script');
          document.getElementsByTagName("head")[0].appendChild(s);
          s.async = true;
          s.onload = () => {
            VEGA_DEBUG[key] = version;
            return resolve(paths[lib]);
          };
          s.onerror = () => reject(`Error loading script: ${paths[lib]}`);
          s.src = paths[lib];
        });
    }

    function showError(err) {
      outputDiv.innerHTML = `<div class="error" style="color:red;">${err}</div>`;
      throw err;
    }

    function displayChart(vegaEmbed) {
      vegaEmbed(outputDiv, spec, embedOpt)
        .catch(err => showError(`Javascript Error: ${err.message}<br>This usually means there's a typo in your chart specification. See the javascript console for the full traceback.`));
    }

    if(typeof define === "function" && define.amd) {
      requirejs.config({paths});
      let deps = ["vega-embed"];
      require(deps, displayChart, err => showError(`Error loading script: ${err.message}`));
    } else {
      maybeLoadScript("vega", "5")
        .then(() => maybeLoadScript("vega-lite", "5.20.1"))
        .then(() => maybeLoadScript("vega-embed", "6"))
        .catch(showError)
        .then(() => displayChart(vegaEmbed));
    }
  })({"config": {"view": {"continuousWidth": 300, "continuousHeight": 300}}, "data": {"url": "https://cdn.jsdelivr.net/npm/vega-datasets@v1.29.0/data/us-10m.json", "format": {"feature": "counties", "type": "topojson"}}, "mark": {"type": "geoshape", "stroke": "white"}, "encoding": {"color": {"field": "Count", "scale": {"scheme": "blues"}, "type": "quantitative"}, "tooltip": [{"field": "County", "type": "nominal"}, {"field": "Count", "type": "quantitative"}]}, "height": 400, "projection": {"type": "mercator"}, "title": "Electric Vehicles by County in Washington", "transform": [{"filter": "floor(datum.id / 1000) == 53"}, {"lookup": "id", "from": {"data": {"name": "data-322436e5b55902f8e410a3e5b8892237"}, "key": "FIPS Code", "fields": ["County", "Count"]}}], "width": 400, "$schema": "https://vega.github.io/schema/vega-lite/v5.20.1.json", "datasets": {"data-322436e5b55902f8e410a3e5b8892237": [{"County": "King", "Count": 128272, "FIPS Code": 53033}, {"County": "Snohomish", "Count": 31810, "FIPS Code": 53061}, {"County": "Pierce", "Count": 21314, "FIPS Code": 53053}, {"County": "Clark", "Count": 15644, "FIPS Code": 53011}, {"County": "Thurston", "Count": 9344, "FIPS Code": 53067}, {"County": "Kitsap", "Count": 8651, "FIPS Code": 53035}, {"County": "Spokane", "Count": 7247, "FIPS Code": 53063}, {"County": "Whatcom", "Count": 6297, "FIPS Code": 53073}, {"County": "Benton", "Count": 3468, "FIPS Code": 53005}, {"County": "Skagit", "Count": 3015, "FIPS Code": 53057}, {"County": "Island", "Count": 2831, "FIPS Code": 53029}, {"County": "Yakima", "Count": 1724, "FIPS Code": 53077}, {"County": "Chelan", "Count": 1601, "FIPS Code": 53007}, {"County": "Clallam", "Count": 1558, "FIPS Code": 53009}, {"County": "Cowlitz", "Count": 1343, "FIPS Code": 53015}, {"County": "Jefferson", "Count": 1334, "FIPS Code": 53031}, {"County": "Mason", "Count": 1266, "FIPS Code": 53045}, {"County": "San Juan", "Count": 1217, "FIPS Code": 53055}, {"County": "Lewis", "Count": 1136, "FIPS Code": 53041}, {"County": "Franklin", "Count": 1042, "FIPS Code": 53021}, {"County": "Grays Harbor", "Count": 972, "FIPS Code": 53027}, {"County": "Grant", "Count": 960, "FIPS Code": 53025}, {"County": "Kittitas", "Count": 936, "FIPS Code": 53037}, {"County": "Walla Walla", "Count": 700, "FIPS Code": 53071}, {"County": "Douglas", "Count": 572, "FIPS Code": 53017}, {"County": "Whitman", "Count": 525, "FIPS Code": 53075}, {"County": "Klickitat", "Count": 458, "FIPS Code": 53039}, {"County": "Okanogan", "Count": 391, "FIPS Code": 53047}, {"County": "Stevens", "Count": 321, "FIPS Code": 53065}, {"County": "Pacific", "Count": 316, "FIPS Code": 53049}, {"County": "Skamania", "Count": 249, "FIPS Code": 53059}, {"County": "Asotin", "Count": 99, "FIPS Code": 53003}, {"County": "Adams", "Count": 99, "FIPS Code": 53001}, {"County": "Wahkiakum", "Count": 89, "FIPS Code": 53069}, {"County": "Pend Oreille", "Count": 88, "FIPS Code": 53051}, {"County": "Lincoln", "Count": 79, "FIPS Code": 53043}, {"County": "Ferry", "Count": 44, "FIPS Code": 53019}, {"County": "Columbia", "Count": 22, "FIPS Code": 53013}, {"County": "Garfield", "Count": 4, "FIPS Code": 53023}]}}, {"mode": "vega-lite"});
</script>



As you can see, the majority of EVs are located in King county, followed up by Snohomish and Pierce counties. This probably makes sense given that EV chargers tend to be located in more populated areas. 
