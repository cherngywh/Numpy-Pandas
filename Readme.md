# **Description**<br />
>**1. The player numbers in ages are distributed in bell-shape, largest group is 20-24.**<br />
>**2. Almost all items are evenly purchased, nothing extremely popular.**<br />
>**3. Although male players are almost 5 times more, but indivisual purchases value are almost the same.** 
<br />

```python
import numpy as np
import pandas as pd
import os
```


```python
jsonpath = os.path.join('purchase_data.json')
jsonfile = pd.read_json(jsonpath)
jsonfile.head()
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
# Player Count
## Total Number of Players
TotalPlayers = jsonfile['SN'].nunique()
TP = {'Total Players': [TotalPlayers]}
TotalPlayers_df = pd.DataFrame(TP)

TotalPlayers_df
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
# Purchasing Analysis (Total)
## Number of Unique Items
UniqueItems = jsonfile['Item Name'].nunique()

## Average Purchase Price
AvePurchase = "$" + str(round(jsonfile['Price'].mean(), 2))

## Total Number of Purchases
TotalPurchase = jsonfile['Price'].count()

## Total Revenue
TotalRevenue = "$" + str(round(jsonfile['Price'].sum(), 2))

PA = {'Number of Unique Items':[UniqueItems],
      'Average Price':AvePurchase,
      'Number of Purchases':[TotalPurchase],
      'Total Revenue':TotalRevenue
     }
PurchasingAnalysis_df = pd.DataFrame(PA)

PurchasingAnalysis_df
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
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Number of Unique Items</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>$2.93</td>
      <td>780</td>
      <td>179</td>
      <td>$2286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Gender Demographics
UplayerList = jsonfile.drop_duplicates('SN', keep='first')
TotalCount = UplayerList["Gender"].count()
GenderCount = UplayerList["Gender"].value_counts()

## Percentage and Count of Male Players
MaleCount = GenderCount["Male"]
MalePercentage = round(MaleCount / TotalCount * 100, 2)

## Percentage and Count of Female Players
FemaleCount = GenderCount["Female"]
FemalePercentage = round(FemaleCount / TotalCount * 100, 2)

## Percentage and Count of Other / Non-Disclosed
OtherCount = GenderCount["Other / Non-Disclosed"]
OtherPercentage = round(OtherCount / TotalCount * 100, 2)

GenderDemo_df = pd.DataFrame([
    {'Percentage of Players': MalePercentage, 'Total Count': MaleCount},
    {'Percentage of Players': FemalePercentage, 'Total Count': FemaleCount},
    {'Percentage of Players': OtherPercentage, 'Total Count': OtherCount}
])
GenderDemo_df = GenderDemo_df.set_index([['Male', 'Female', 'Other / Non-Disclosed']])

GenderDemo_df
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
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>81.15</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>17.45</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.40</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Purchasing Analysis (Gender)
MalePurchase_df = jsonfile.loc[jsonfile['Gender'] == 'Male']
FemalePurchase_df = jsonfile.loc[jsonfile['Gender'] == 'Female']
OtherPurchase_df = jsonfile.loc[jsonfile['Gender'] == 'Other / Non-Disclosed']

## Purchase Count
MalePurchaseCount = MalePurchase_df['Price'].count()
FemalePurchaseCount = FemalePurchase_df['Price'].count()
OtherPurchaseCount = OtherPurchase_df['Price'].count()

## Average Purchase Price
MalePurchaseAve = '$'+str(round(MalePurchase_df['Price'].mean(), 2))
FemalePurchaseAve = '$'+str(round(FemalePurchase_df['Price'].mean(), 2))
OtherPurchaseAve = '$'+str(round(OtherPurchase_df['Price'].mean(), 2))

## Total Purchase Value
MalePurchaseTotal = round(MalePurchase_df['Price'].sum(), 2)
FemalePurchaseTotal = round(FemalePurchase_df['Price'].sum(), 2)
OtherPurchaseTotal = round(OtherPurchase_df['Price'].sum(), 2)

## Normalized Totals (normalizing for the # of people in each age group)
MaleNormalized = '$' + str(round((MalePurchaseTotal / MaleCount), 2))
FemaleNormalized = '$' + str(round(FemalePurchaseTotal / FemaleCount, 2))
OtherNormalized = '$' + str(round(OtherPurchaseTotal / OtherCount, 2))

PurchAnalysis_df = pd.DataFrame({'Gender':['Female', 'Male', 'Other / Non-Disclosed'],
                                 'Purchase Count':[FemalePurchaseCount, MalePurchaseCount, OtherPurchaseCount],
                                 'Average Purchase Price':[FemalePurchaseAve, MalePurchaseAve, OtherPurchaseAve],
                                 'Total Purchase Value':['$'+str(FemalePurchaseTotal), '$'+str(MalePurchaseTotal), '$'+str(OtherPurchaseTotal)],
                                 'Normalized Totals':[FemaleNormalized, MaleNormalized, OtherNormalized]},
                                 columns=['Gender', 'Purchase Count', 'Average Purchase Price', 'Total Purchase Value', 'Normalized Totals'])

PurchAnalysis_df = PurchAnalysis_df.set_index('Gender')

PurchAnalysis_df
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
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
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
      <td>$2.82</td>
      <td>$382.91</td>
      <td>$3.83</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>633</td>
      <td>$2.95</td>
      <td>$1867.68</td>
      <td>$4.02</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>$3.25</td>
      <td>$35.74</td>
      <td>$4.47</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Age Demographics
begin = [0]
end = [100]
middle = list(range(9, 41, 5))
bins = begin + middle + end
group_names = ['<10', '10-14', '15-19', '20-24', '25-29', '30-34', '35-39', '40+']
jsonfile['Age Group'] = pd.cut(jsonfile['Age'], bins, labels=group_names)

## Head Count
AgeHeadcount = jsonfile.drop_duplicates('SN', keep='first')
AgeTotal = AgeHeadcount['Age Group'].count()
EveryTotal = AgeHeadcount['Age Group'].value_counts(sort=False)
EveryPercent = round((EveryTotal/AgeTotal)*100, 2)

HeadCount_df = pd.DataFrame({'Total Count':EveryTotal,
                             'Percentage of Players':EveryPercent
})

HeadCount_df
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
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>3.32</td>
      <td>19</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>4.01</td>
      <td>23</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>17.45</td>
      <td>100</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>45.20</td>
      <td>259</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>15.18</td>
      <td>87</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>8.20</td>
      <td>47</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>4.71</td>
      <td>27</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>1.92</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>




```python
AgeDemographics = jsonfile.groupby('Age Group')

## Purchase Count
AgeCount = AgeDemographics['Item Name'].count()

## Average Purchase Price
AgeAve = round(AgeDemographics['Price'].mean(), 2)

## Total Purchase Value
AgeTotal = round(AgeDemographics['Price'].sum(), 2)

## Normalized Totals (normalizing for the # of people in each age group)
UAgeDemo = jsonfile.drop_duplicates('SN', keep='first')
UAgeDemoCount = UAgeDemo['Age Group'].value_counts()

AgeDemo_df = pd.DataFrame({'Purchase Count':AgeCount,
                           'Average Purchase Price':AgeAve.map('${:,.2f}'.format),
                           'Total Purchase Value':AgeTotal.map('${:,.2f}'.format)},
                           columns=['Purchase Count', 'Average Purchase Price', 'Total Purchase Value'])

AgeDemo_df['Normalized Totals'] = (round(AgeTotal / UAgeDemoCount, 2)).map('${:,.2f}'.format)

AgeDemo_df
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
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th>Age Group</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>28</td>
      <td>$2.98</td>
      <td>$83.46</td>
      <td>$4.39</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>35</td>
      <td>$2.77</td>
      <td>$96.95</td>
      <td>$4.22</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>133</td>
      <td>$2.91</td>
      <td>$386.42</td>
      <td>$3.86</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>336</td>
      <td>$2.91</td>
      <td>$978.77</td>
      <td>$3.78</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>125</td>
      <td>$2.96</td>
      <td>$370.33</td>
      <td>$4.26</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>64</td>
      <td>$3.08</td>
      <td>$197.25</td>
      <td>$4.20</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>42</td>
      <td>$2.84</td>
      <td>$119.40</td>
      <td>$4.42</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>17</td>
      <td>$3.16</td>
      <td>$53.75</td>
      <td>$4.89</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Top Spenders
PlayerPurchase = jsonfile.groupby('SN')
PlayerPurchaseCount = PlayerPurchase['Item Name'].count()
PlayerPurchaseAve = round(PlayerPurchase['Price'].mean(), 2)
PlayerPurchaseTotal = round(PlayerPurchase['Price'].sum(), 2)

TopSpender_df = pd.DataFrame({'Purchase Count':PlayerPurchaseCount,
                              'Average Purchase Price':PlayerPurchaseAve.map('${:,.2f}'.format),
                              'Total Purchase Value':PlayerPurchaseTotal.map('${:,.2f}'.format)},
                              columns=['Purchase Count', 'Average Purchase Price', 'Total Purchase Value'])

TopSpender_df = TopSpender_df.sort_values(by='Total Purchase Value', ascending=False)

TopSpender_df.head()
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
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Qarwen67</th>
      <td>4</td>
      <td>$2.49</td>
      <td>$9.97</td>
    </tr>
    <tr>
      <th>Sondim43</th>
      <td>3</td>
      <td>$3.13</td>
      <td>$9.38</td>
    </tr>
    <tr>
      <th>Tillyrin30</th>
      <td>3</td>
      <td>$3.06</td>
      <td>$9.19</td>
    </tr>
    <tr>
      <th>Lisistaya47</th>
      <td>3</td>
      <td>$3.06</td>
      <td>$9.19</td>
    </tr>
    <tr>
      <th>Tyisriphos58</th>
      <td>2</td>
      <td>$4.59</td>
      <td>$9.18</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Most Popular Items
temp_df = jsonfile.groupby(['Item ID','Item Name'])

## Total Purchase Value & Purchase Count
ItemTotal = temp_df['Price'].sum()
ItemCount = temp_df['Item ID'].count()

PopularItem_df = pd.DataFrame({'Total Purchase Value':ItemTotal.map('${:,.2f}'.format),
                               'Purchase Count':ItemCount})

## Item Price
PopularItem_df['Item Price'] = (ItemTotal/ItemCount).map('${:,.2f}'.format)
PopularItem_df = PopularItem_df[['Purchase Count', 'Item Price', 'Total Purchase Value']]
PopularItem_df = PopularItem_df.sort_values(by='Purchase Count', ascending=False)

PopularItem_df.head()
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
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>39</th>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>11</td>
      <td>$2.35</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <th>Arcane Gem</th>
      <td>11</td>
      <td>$2.23</td>
      <td>$24.53</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <td>9</td>
      <td>$2.07</td>
      <td>$18.63</td>
    </tr>
    <tr>
      <th>175</th>
      <th>Woeful Adamantite Claymore</th>
      <td>9</td>
      <td>$1.24</td>
      <td>$11.16</td>
    </tr>
    <tr>
      <th>13</th>
      <th>Serenity</th>
      <td>9</td>
      <td>$1.49</td>
      <td>$13.41</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Most Profitable Items
temp_df = jsonfile.groupby(['Item ID','Item Name'])

## Total Purchase Value & Purchase Count
ItemTotal = temp_df['Price'].sum()
ItemCount = temp_df['Item ID'].count()

PopularItem_df = pd.DataFrame({'Total Purchase Value':ItemTotal.map('${:,.2f}'.format),
                               'Purchase Count':ItemCount})
## Item Price
PopularItem_df['Item Price'] = (ItemTotal/ItemCount).map('${:,.2f}'.format)
PopularItem_df = PopularItem_df[['Purchase Count', 'Item Price', 'Total Purchase Value']]
PopularItem_df = PopularItem_df.sort_values(by='Total Purchase Value', ascending=False)

PopularItem_df.head()
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
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>170</th>
      <th>Shadowsteel</th>
      <td>5</td>
      <td>$1.98</td>
      <td>$9.90</td>
    </tr>
    <tr>
      <th>21</th>
      <th>Souleater</th>
      <td>3</td>
      <td>$3.27</td>
      <td>$9.81</td>
    </tr>
    <tr>
      <th>37</th>
      <th>Shadow Strike, Glory of Ending Hope</th>
      <td>5</td>
      <td>$1.93</td>
      <td>$9.65</td>
    </tr>
    <tr>
      <th>127</th>
      <th>Heartseeker, Reaver of Souls</th>
      <td>3</td>
      <td>$3.21</td>
      <td>$9.63</td>
    </tr>
    <tr>
      <th>120</th>
      <th>Agatha</th>
      <td>5</td>
      <td>$1.91</td>
      <td>$9.55</td>
    </tr>
  </tbody>
</table>
</div>
