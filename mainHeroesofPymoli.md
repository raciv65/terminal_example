# Heroes Of Pymoli Data Analysis
## Summary
-There are <576> players, XX% are males and XX are femmales.



## Data

There was a 780 purchase, in was contein in....

There are 576 playes (`TotalPlayers`) which made XXXX


```python
#Setup
import pandas as pd
#Load file
DataFile="Resources/purchase_data.csv"
Purchase_df=pd.read_csv(DataFile)
Purchase_df.head()

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
      <th>Purchase ID</th>
      <th>SN</th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Lisim78</td>
      <td>20</td>
      <td>Male</td>
      <td>108</td>
      <td>Extraction, Quickblade Of Trembling Hands</td>
      <td>3.53</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Lisovynya38</td>
      <td>40</td>
      <td>Male</td>
      <td>143</td>
      <td>Frenzied Scimitar</td>
      <td>1.56</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Ithergue48</td>
      <td>24</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>4.88</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Chamassasya86</td>
      <td>24</td>
      <td>Male</td>
      <td>100</td>
      <td>Blindscythe</td>
      <td>3.27</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Iskosia90</td>
      <td>23</td>
      <td>Male</td>
      <td>131</td>
      <td>Fury</td>
      <td>1.44</td>
    </tr>
  </tbody>
</table>
</div>




```python
TotalPlayers=len(Purchase_df['SN'].unique())
pd.DataFrame({'Total':TotalPlayers}, index=['Players'])
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
      <th>Total</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Players</th>
      <td>576</td>
    </tr>
  </tbody>
</table>
</div>



## Revenue Analysis


```python
UniqueItems=len(Purchase_df['Item ID'].unique())
PriceAverage=round(Purchase_df['Price'].mean(),4)
NumberPurchases=Purchase_df['Purchase ID'].count()
Revenue=round(Purchase_df['Price'].sum(),2)
```


```python
RenevuePurchasing=pd.DataFrame({
    'Unique Items':UniqueItems,
    'Average Purchase Price':'${:.2f}'.format(PriceAverage),
    'Number of Purchases':NumberPurchases,
    'Total Revenue':'${:,}'.format(Revenue)
}, index=['Revenue summary'])
RenevuePurchasing
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
      <th>Unique Items</th>
      <th>Average Purchase Price</th>
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Revenue summary</th>
      <td>179</td>
      <td>$3.05</td>
      <td>780</td>
      <td>$2,379.77</td>
    </tr>
  </tbody>
</table>
</div>



## Gender Demographics
  
 There are player which are


```python
genderGroup=Purchase_df.groupby(['Gender'])
genderCount=genderGroup.nunique()['SN']
genderPercent=genderGroup.nunique()['SN']/TotalPlayers

```


```python
SummaryGender=pd.DataFrame({
    'Players':genderCount,
    'Percent':genderPercent.map('{:.2%}'.format)
}, index=set(Purchase_df['Gender']))
SummaryGender
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
      <th>Players</th>
      <th>Percent</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>484</td>
      <td>84.03%</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>1.91%</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>81</td>
      <td>14.06%</td>
    </tr>
  </tbody>
</table>
</div>



### Purchasing Analysis


```python
genderPurcaseCount=round(genderGroup['Purchase ID'].count(),2)
genderPriceAverage=round(genderGroup['Price'].mean(),2)
genderPurchaseValue=round(genderGroup['Price'].sum(),2)
genderPurcaseAvarangePer=genderPurchaseValue/genderCount # genderCount define in the section above 

```


```python
SummaryGender2=pd.DataFrame({
    'Number of purchasings':genderPurcaseCount,
    'Avarage purchase price':genderPriceAverage.map('${:.2f}'.format),
    'Total purchase vale':genderPurchaseValue.map('${:,}'.format),
    'Avarage Purchase per person':genderPurcaseAvarangePer.map('${:.2f}'.format)
})
SummaryGender2
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
      <th>Number of purchasings</th>
      <th>Avarage purchase price</th>
      <th>Total purchase vale</th>
      <th>Avarage Purchase per person</th>
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
      <td>113</td>
      <td>$3.20</td>
      <td>$361.94</td>
      <td>$4.47</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>652</td>
      <td>$3.02</td>
      <td>$1,967.64</td>
      <td>$4.07</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>15</td>
      <td>$3.35</td>
      <td>$50.19</td>
      <td>$4.56</td>
    </tr>
  </tbody>
</table>
</div>



## Age Demographis


```python
#Establish bins for ages
bins=[0,9,14,19,24,29,34,39,99]
bins_labels=['<10','10-14','15-19','20-24','25-29','30-34','35-39','40+']
Purchase_df['Age category']=pd.cut(Purchase_df['Age'], bins, labels=bins_labels)
AgeCount=Purchase_df.groupby(['Age category']).nunique()['SN']
AgePercent=round(AgeCount/TotalPlayers,4)
```


```python
SummaryAge=pd.DataFrame({
    'Total count':AgeCount,
    'Percent':AgePercent.map('{:.2%}'.format)
},)
SummaryAge
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
      <th>Total count</th>
      <th>Percent</th>
    </tr>
    <tr>
      <th>Age category</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>17</td>
      <td>2.95%</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>22</td>
      <td>3.82%</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>107</td>
      <td>18.58%</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>258</td>
      <td>44.79%</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>77</td>
      <td>13.37%</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>52</td>
      <td>9.03%</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>31</td>
      <td>5.38%</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>12</td>
      <td>2.08%</td>
    </tr>
  </tbody>
</table>
</div>



### Purchasing by Age


```python
AgeGroup=Purchase_df.groupby(['Age category'])
AgeCategory=AgeGroup['Purchase ID'].count()
AgeAveragePrice=round(AgeGroup['Price'].mean(),2)
AgePurchases=round(AgeGroup['Price'].sum(),2)
AgeAvePerCategory=round(AgePurchases/AgeCount,2)

```


```python
SummaryAge2=pd.DataFrame({
    'Purchase Count':AgeCategory,
    'Average purchase price':AgeAveragePrice.map('${:.2f}'.format),
    'Total purchase value':AgePurchases.map('${:,}'.format),
    'Ave Total Purchase per Person':AgeAvePerCategory.map('${:.2f}'.format)
})
SummaryAge2
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
      <th>Purchase Count</th>
      <th>Average purchase price</th>
      <th>Total purchase value</th>
      <th>Ave Total Purchase per Person</th>
    </tr>
    <tr>
      <th>Age category</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>23</td>
      <td>$3.35</td>
      <td>$77.13</td>
      <td>$4.54</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>28</td>
      <td>$2.96</td>
      <td>$82.78</td>
      <td>$3.76</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>136</td>
      <td>$3.04</td>
      <td>$412.89</td>
      <td>$3.86</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>365</td>
      <td>$3.05</td>
      <td>$1,114.06</td>
      <td>$4.32</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>101</td>
      <td>$2.90</td>
      <td>$293.0</td>
      <td>$3.81</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>73</td>
      <td>$2.93</td>
      <td>$214.0</td>
      <td>$4.12</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>41</td>
      <td>$3.60</td>
      <td>$147.67</td>
      <td>$4.76</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>13</td>
      <td>$2.94</td>
      <td>$38.24</td>
      <td>$3.19</td>
    </tr>
  </tbody>
</table>
</div>



## Top Spenders


```python
SNCount=Purchase_df.groupby(['SN']).count()['Purchase ID']
SNAveragePurchasePrice=Purchase_df.groupby(['SN']).mean()['Price']
SNPurchaseValue=Purchase_df.groupby(['SN']).sum()['Price']

```


```python
SummarySN=pd.DataFrame({
    'Count Purchase':SNCount,
    'Avarage Purchase Price':SNAveragePurchasePrice.map('${:.2f}'.format),
    'Total Purchase Value':SNPurchaseValue.map('${:10.2f}'.format)
}, )
SummarySN.sort_values(by=['Total Purchase Value'], ascending=False).head()
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
      <th>Count Purchase</th>
      <th>Avarage Purchase Price</th>
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
      <th>Lisosia93</th>
      <td>5</td>
      <td>$3.79</td>
      <td>$     18.96</td>
    </tr>
    <tr>
      <th>Idastidru52</th>
      <td>4</td>
      <td>$3.86</td>
      <td>$     15.45</td>
    </tr>
    <tr>
      <th>Chamjask73</th>
      <td>3</td>
      <td>$4.61</td>
      <td>$     13.83</td>
    </tr>
    <tr>
      <th>Iral74</th>
      <td>4</td>
      <td>$3.40</td>
      <td>$     13.62</td>
    </tr>
    <tr>
      <th>Iskadarya95</th>
      <td>3</td>
      <td>$4.37</td>
      <td>$     13.10</td>
    </tr>
  </tbody>
</table>
</div>



## Most Popular Items


```python
ItemCount=Purchase_df.groupby(['Item ID','Item Name']).count()['Purchase ID']
ItemPrice=Purchase_df.groupby(['Item ID','Item Name']).mean()['Price']
ItemValue=Purchase_df.groupby(['Item ID','Item Name']).sum()['Price']
```


```python
SummaryItems=pd.DataFrame({
    'Purchase Count':ItemCount,
    'Item Price':ItemPrice.map('${:.2f}'.format),
    'Total Purcahse Value':ItemValue.map('${:10.2f}'.format)
})
SummaryItems.sort_values(by=['Purchase Count'], ascending=False).head()
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
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purcahse Value</th>
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
      <th>92</th>
      <th>Final Critic</th>
      <td>13</td>
      <td>$4.61</td>
      <td>$     59.99</td>
    </tr>
    <tr>
      <th>178</th>
      <th>Oathbreaker, Last Hope of the Breaking Storm</th>
      <td>12</td>
      <td>$4.23</td>
      <td>$     50.76</td>
    </tr>
    <tr>
      <th>145</th>
      <th>Fiery Glass Crusader</th>
      <td>9</td>
      <td>$4.58</td>
      <td>$     41.22</td>
    </tr>
    <tr>
      <th>132</th>
      <th>Persuasion</th>
      <td>9</td>
      <td>$3.22</td>
      <td>$     28.99</td>
    </tr>
    <tr>
      <th>108</th>
      <th>Extraction, Quickblade Of Trembling Hands</th>
      <td>9</td>
      <td>$3.53</td>
      <td>$     31.77</td>
    </tr>
  </tbody>
</table>
</div>



### Most Porfitable Items


```python
SummaryItems.sort_values(by=['Total Purcahse Value'], ascending=False).head()
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
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purcahse Value</th>
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
      <th>92</th>
      <th>Final Critic</th>
      <td>13</td>
      <td>$4.61</td>
      <td>$     59.99</td>
    </tr>
    <tr>
      <th>178</th>
      <th>Oathbreaker, Last Hope of the Breaking Storm</th>
      <td>12</td>
      <td>$4.23</td>
      <td>$     50.76</td>
    </tr>
    <tr>
      <th>82</th>
      <th>Nirvana</th>
      <td>9</td>
      <td>$4.90</td>
      <td>$     44.10</td>
    </tr>
    <tr>
      <th>145</th>
      <th>Fiery Glass Crusader</th>
      <td>9</td>
      <td>$4.58</td>
      <td>$     41.22</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>8</td>
      <td>$4.35</td>
      <td>$     34.80</td>
    </tr>
  </tbody>
</table>
</div>




```python
SummaryItems.sort_values(by=['Item Price'], ascending=False).head()
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
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purcahse Value</th>
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
      <th>63</th>
      <th>Stormfury Mace</th>
      <td>2</td>
      <td>$4.99</td>
      <td>$      9.98</td>
    </tr>
    <tr>
      <th>139</th>
      <th>Mercy, Katana of Dismay</th>
      <td>5</td>
      <td>$4.94</td>
      <td>$     24.70</td>
    </tr>
    <tr>
      <th>173</th>
      <th>Stormfury Longsword</th>
      <td>2</td>
      <td>$4.93</td>
      <td>$      9.86</td>
    </tr>
    <tr>
      <th>147</th>
      <th>Hellreaver, Heirloom of Inception</th>
      <td>3</td>
      <td>$4.93</td>
      <td>$     14.79</td>
    </tr>
    <tr>
      <th>128</th>
      <th>Blazeguard, Reach of Eternity</th>
      <td>5</td>
      <td>$4.91</td>
      <td>$     24.55</td>
    </tr>
  </tbody>
</table>
</div>



## Conslusions


```python

```


```python

```
