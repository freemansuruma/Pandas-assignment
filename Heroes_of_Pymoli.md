

```python
import json
import pandas as pd
import numpy as np
from pandas import DataFrame
```


```python
with open('purchase_data.json') as json_data:
    data = json.load(json_data)
```


```python
#Create dataframe from data being read from the json file
df = pd.DataFrame.from_dict(data)
df.head()
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
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Player count
total_rows = df[['Gender','SN','Age']].drop_duplicates()
total_players = total_rows.count()[0]
df_total_players = DataFrame({'Total Players': [total_players]})
df_total_players
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
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>573</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Purchasing Analysis (Total)
unique_items = df['Item ID'].nunique()
avg_purchase_price = df['Price'].mean()
total_purchases = df['Price'] .count()
total_revenue = df['Price'].sum()

df_purchasing = DataFrame({
    'Number of Unique Items':[unique_items],
    'Average Purchase Price':[avg_purchase_price],
    'Total Number of Purchases':[total_purchases],
    'Total Revenue':[total_revenue]
})
df_purchasing
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
      <th>Average Purchase Price</th>
      <th>Number of Unique Items</th>
      <th>Total Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2.931192</td>
      <td>183</td>
      <td>780</td>
      <td>2286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Gender Demographics
gender = total_rows['Gender'].value_counts()
male = gender[0]
female = gender[1]
other = gender[2]
male_pct = (male/total_players) *100
female_pct = (female/total_players) *100
other_pct = (other/total_players) *100

df_gender = pd.Series(gender).to_frame()
df_gender['Percentage'] = [male_pct.round(2),female_pct.round(2),other_pct.round(2)]
df_gender.rename(columns={'Gender':'Total Count'})
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
      <th>Total Count</th>
      <th>Percentage</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>465</td>
      <td>81.15</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>100</td>
      <td>17.45</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>8</td>
      <td>1.40</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Purchasing Analysis (Gender)
grouped_by_gender = df.groupby('Gender')
purchase_by_gender = grouped_by_gender['Item ID'].count()
total_rev_gender = grouped_by_gender['Price'].sum()
normalized_totals = grouped_by_gender['Price'].mean()
avg_by_gender = grouped_by_gender['Price'].mean()
df_purchasing_gender = purchase_by_gender.to_frame()
df_purchasing_gender['Average Purchase Price',] = avg_by_gender.round(2)
df_purchasing_gender['Total Revenue']=total_rev_gender
df_purchasing_gender['Normalized Total']=normalized_totals.round(2)
df_purchasing_gender.rename(columns={'Item ID':'Purchase Count'})
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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Revenue</th>
      <th>Normalized Total</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>136</td>
      <td>2.82</td>
      <td>382.91</td>
      <td>2.82</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>633</td>
      <td>2.95</td>
      <td>1867.68</td>
      <td>2.95</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>3.25</td>
      <td>35.74</td>
      <td>3.25</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Age Demographics
bins = [0,10,15,20,25,30,35,40,45]
group_name = ['<10','10-14','15-19','20-24','25-29','30-34','35-39','40+']
df['Age Bucket'] = pd.cut(df['Age'],bins, labels=group_name)
grouped_by_age = df.groupby('Age Bucket')
purchase_by_age = grouped_by_age['Item ID'].count()
total_rev_by_age = grouped_by_age['Price'].sum()
avg_by_age = grouped_by_age['Price'].mean()
normalized_total = grouped_by_age['Price'].mean()
df_age = purchase_by_age.to_frame()
df_age['Total Revenue'] = total_rev_by_age
df_age['Average Price'] = avg_by_age.round(2)
df_age['Normalized Total']=normalized_total.round(2)
df_age.rename(columns={'Item ID':'Purchase Count'})
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
      <th>Purchase Count</th>
      <th>Total Revenue</th>
      <th>Average Price</th>
      <th>Normalized Total</th>
    </tr>
    <tr>
      <th>Age Bucket</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>32</td>
      <td>96.62</td>
      <td>3.02</td>
      <td>3.02</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>78</td>
      <td>224.15</td>
      <td>2.87</td>
      <td>2.87</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>184</td>
      <td>528.74</td>
      <td>2.87</td>
      <td>2.87</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>305</td>
      <td>902.61</td>
      <td>2.96</td>
      <td>2.96</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>76</td>
      <td>219.82</td>
      <td>2.89</td>
      <td>2.89</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>58</td>
      <td>178.26</td>
      <td>3.07</td>
      <td>3.07</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>44</td>
      <td>127.49</td>
      <td>2.90</td>
      <td>2.90</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>3</td>
      <td>8.64</td>
      <td>2.88</td>
      <td>2.88</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Top Spenders
group_spenders = df.groupby('SN')['Price'].sum()
top_spenders = group_spenders.sort_values(ascending=False)
top_spenders = top_spenders.iloc[:5]
top_spenders
```




    SN
    Undirrala66    17.06
    Saedue76       13.56
    Mindimnya67    12.74
    Haellysu29     12.73
    Eoda93         11.58
    Name: Price, dtype: float64




```python
#Most Popular Items
group_items = df.groupby('Item Name')['Item Name'].count()
top_items = group_items.sort_values(ascending=False)
top_items.iloc[:5]
```




    Item Name
    Final Critic                            14
    Betrayal, Whisper of Grieving Widows    11
    Arcane Gem                              11
    Stormcaller                             10
    Woeful Adamantite Claymore               9
    Name: Item Name, dtype: int64




```python
#Most Profitable Items
group_prices = df.groupby('Item Name')['Price'].sum()
most_prof = group_prices.sort_values(ascending=False)
most_prof.iloc[:5]
```




    Item Name
    Final Critic                  38.60
    Retribution Axe               37.26
    Stormcaller                   34.65
    Spectral Diamond Doomblade    29.75
    Orenmir                       29.70
    Name: Price, dtype: float64


