# SF_population
An awesome js time-series annotation tool + interesting census data + ihtml magic in a jupyter notebook



```python
import pandas as pd
from glob import glob
```

# Function for parsing the right columns:
(*Yeah, so, the geniuses at census.gov thought it would be nice to let the column identifiers change for every year for the effing metric*)


```python
def get_race_col(df_year):
    lst=list(df_year.iloc[0,:].values)
    column_search=[[x for x in lst if 'Total population' in x][0],
    [x for x in lst if 'One race - White' in x][0],
    [x for x in lst if 'One race - Black' in x][0],
    [x for x in lst if 'One race - Asian' in x][0],
    [x for x in lst if 'Mexican' in x][0],
    [x for x in lst if 'Chinese' in x][0]]
    return column_search
```

# Parsing the data and generating the required csv file for visualizing using storyline.js


```python
csv_files=glob('*_ann.csv')
df_combined=pd.DataFrame(columns=['year','Overall','White','Black','Asian','Mexican','Chinese'])

for i,file_id in enumerate(csv_files):
    print(file_id)
    year=file_id[4:6]
    print(year)
    df_combined.loc[i,'year']=int('20'+year)
    df_year=pd.read_csv(file_id,skiprows=0)
    column_search_year=get_race_col(df_year)
    df_year_2=pd.read_csv(file_id,skiprows=1)
    pop_vec=df_year_2.loc[0,column_search_year].values
    df_combined.loc[i,1:]=pop_vec
    print(pop_vec)
```

    ACS_10_5YR_DP05_with_ann.csv
    10
    [789172 406874 49071 264709 61741 166683]
    ACS_11_5YR_DP05_with_ann.csv
    11
    [797983 408857 49260 267289 62939 170616]
    ACS_12_5YR_DP05_with_ann.csv
    12
    [807755 409445 48419 270503 62736 170897]
    ACS_13_5YR_DP05_with_ann.csv
    13
    [817501 411561 47992 272599 62702 171564]
    ACS_14_5YR_DP05_with_ann.csv
    14
    [829072 410245 47611 278274 64177 175091]
    ACS_15_5YR_DP05_with_ann.csv
    15
    [840763 409681 46825 284426 65649 179644]
    


```python
df_combined
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>year</th>
      <th>Overall</th>
      <th>White</th>
      <th>Black</th>
      <th>Asian</th>
      <th>Mexican</th>
      <th>Chinese</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2010</td>
      <td>789172</td>
      <td>406874</td>
      <td>49071</td>
      <td>264709</td>
      <td>61741</td>
      <td>166683</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2011</td>
      <td>797983</td>
      <td>408857</td>
      <td>49260</td>
      <td>267289</td>
      <td>62939</td>
      <td>170616</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2012</td>
      <td>807755</td>
      <td>409445</td>
      <td>48419</td>
      <td>270503</td>
      <td>62736</td>
      <td>170897</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2013</td>
      <td>817501</td>
      <td>411561</td>
      <td>47992</td>
      <td>272599</td>
      <td>62702</td>
      <td>171564</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2014</td>
      <td>829072</td>
      <td>410245</td>
      <td>47611</td>
      <td>278274</td>
      <td>64177</td>
      <td>175091</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2015</td>
      <td>840763</td>
      <td>409681</td>
      <td>46825</td>
      <td>284426</td>
      <td>65649</td>
      <td>179644</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_combined.to_csv('population_SF_race.csv',index=False)
```

# Now, edit the title and text columns on google sheets. 
For example:
![image.png](attachment:image.png)

# Last steps:
- go to https://storyline.knightlab.com/ and generate the iframe. 
- install ihtml from here: https://github.com/thedataincubator/ihtml
-  Embed and visualize!


```python
import ihtml
import warnings
warnings.filterwarnings('ignore')
```




```python
%%ihtml 550
<iframe src="https://cdn.knightlab.com/libs/storyline/latest/embed/index.html?dataURL=https%3A%2F%2Fdocs.google.com%2Fspreadsheets%2Fd%2F1tVGDusNhpF6dmqufbYhLqApcA7Fl_oMmiw19NtyZmY8%2Fedit%23gid%3D1112356917&amp;dataYCol=black&amp;dataXCol=year&amp;dataDateFormat=%25Y&amp;chartDateFormat=%25Y&amp;chartYLabel=Black%20population&amp;sliderCardTitleCol=title&amp;sliderCardTextCol=text" style="width:90%;height:750px;" frameborder="100" marginwidth="100" marginheight="100" vspace="100" hspace="0"></iframe>
```



        <iframe
            width="100%"
            height="550"
            src="data:text/html;base64,PGlmcmFtZSBzcmM9Imh0dHBzOi8vY2RuLmtuaWdodGxhYi5jb20vbGlicy9zdG9yeWxpbmUvbGF0ZXN0L2VtYmVkL2luZGV4Lmh0bWw/ZGF0YVVSTD1odHRwcyUzQSUyRiUyRmRvY3MuZ29vZ2xlLmNvbSUyRnNwcmVhZHNoZWV0cyUyRmQlMkYxdFZHRHVzTmhwRjZkbXF1ZmJZaExxQXBjQTdGbF9vTW1pdzE5TnR5Wm1ZOCUyRmVkaXQlMjNnaWQlM0QxMTEyMzU2OTE3JmFtcDtkYXRhWUNvbD1ibGFjayZhbXA7ZGF0YVhDb2w9eWVhciZhbXA7ZGF0YURhdGVGb3JtYXQ9JTI1WSZhbXA7Y2hhcnREYXRlRm9ybWF0PSUyNVkmYW1wO2NoYXJ0WUxhYmVsPUJsYWNrJTIwcG9wdWxhdGlvbiZhbXA7c2xpZGVyQ2FyZFRpdGxlQ29sPXRpdGxlJmFtcDtzbGlkZXJDYXJkVGV4dENvbD10ZXh0IiBzdHlsZT0id2lkdGg6OTAlO2hlaWdodDo3NTBweDsiIGZyYW1lYm9yZGVyPSIxMDAiIG1hcmdpbndpZHRoPSIxMDAiIG1hcmdpbmhlaWdodD0iMTAwIiB2c3BhY2U9IjEwMCIgaHNwYWNlPSIwIj48L2lmcmFtZT4="
            frameborder="0"
            allowfullscreen
        ></iframe>
        


