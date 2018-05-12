

```python
import os as os
import pandas as pd
import csv as csv
```


```python
hero = pd.read_json("purchase_data.json", orient = "columns")
hero.head()
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
#Total number of players
totalnumber = len(hero["SN"].unique())
#f"The total number of players is: {totalnumber}"
total_df = pd.DataFrame({
    "Total Players": [totalnumber]})
total_df

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
avg_price = hero["Price"].mean()
```


```python
#Purchasing Analysis (Total)
#Total Number of Unique Items (Note: I'm using the Item ID number even though its unique value
#does not match the unique number of Item Names)

total_uniq_item = len(hero["Item ID"].unique())

#f"The total number of unique items is: {total_uniq_item}"
```


```python
# Total Number of Purchases
total_purchase = hero.shape[0]
f"The total number of purchases = {total_purchase}"

# Total Revenue
total_revenue = hero["Price"].sum()
total_revenue

# Average Purchase Price ( can also use hero["Price"].mean())
average_price = total_revenue/total_purchase
#f"The average price is: ${average_price:.3}"
```


```python
new_df = pd.DataFrame({
    "Number of Unique Items": [total_uniq_item],
    "Average Price": [f"${average_price:.3}"],
    "Number of Purchases": [f"{total_purchase}"],
    "Total Revenue": ['${:,}'.format(total_revenue)]
})
new_df
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
      <td>183</td>
      <td>$2,286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
total_unique = hero.drop_duplicates("SN")
```


```python
# Gender Demographics

gender = total_unique["Gender"].value_counts()
gender
# gender = hero["Gender"].value_counts()
males = gender[0]
females = gender[1]
other = gender[2]

total = int(totalnumber)
gender_df = pd.DataFrame({
    "Gender": ["Male", "Female", "Other"],
    "Players Count": [males, females, other],
    "Percentage of Players": [males/total*100, females/total*100, other/total*100]
})

gender_df["Percentage of Players"] = gender_df["Percentage of Players"].map("{:.2f}%".format)
gender_df

# print(f"There are {gender[0]} playing this game")

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
      <th>Gender</th>
      <th>Percentage of Players</th>
      <th>Players Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Male</td>
      <td>81.15%</td>
      <td>465</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Female</td>
      <td>17.45%</td>
      <td>100</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Other</td>
      <td>1.40%</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Purchasing Analysis (Gender)

#PLEASE NOTE!!!! I am using only unique user names or "SN" for gender demographics. If I don't, I end up with 633 male 
#players alone which is well over the total number of unique players

# Purchase Count

hero_female = hero.loc[hero["Gender"] == "Female", :]
hero_male = hero.loc[hero["Gender"] == "Male", :]
hero_other = hero.loc[hero["Gender"] == "Other / Non-Disclosed", :]

total_purchase = hero.shape[0]

purchases_female = hero_female.shape[0]
purchases_male = hero_male.shape[0]
purchases_other = hero_other.shape[0]

# Average Purchase Price
female_avg =hero_female["Price"].mean()
male_avg =hero_male["Price"].mean()
other_avg =hero_other["Price"].mean()

# Total Purchase Value
female_revenue = hero_female["Price"].sum()
male_revenue = hero_male["Price"].sum()
other_revenue = hero_other["Price"].sum()

# Normalized Totals
fem_norm_total = float(female_revenue/purchases_female)
m_norm_total = float(male_revenue/purchases_male)
o_norm_total = float(other_revenue/purchases_other)

#Dataframe
gender_2_df = pd.DataFrame({
    "Gender": ["Male", "Female", "Other"],
    "Purchase Count": [purchases_male, purchases_female, purchases_other],
    "Average Purchase Price": [male_avg, female_avg, other_avg],
    "Total Purchase Value": [male_revenue, female_revenue, other_revenue],
    "Normalized Totals": [m_norm_total, fem_norm_total, o_norm_total]
})

gender_2_df["Average Purchase Price"] = gender_2_df["Average Purchase Price"].map("${:.2f}".format)
gender_2_df["Normalized Totals"] = gender_2_df["Normalized Totals"].map("${:.2f}".format)
gender_2_df["Total Purchase Value"] = gender_2_df["Total Purchase Value"].map("${:.2f}".format)

gender_2_df
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
      <th>Average Purchase Price</th>
      <th>Gender</th>
      <th>Normalized Totals</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>$2.95</td>
      <td>Male</td>
      <td>$2.95</td>
      <td>633</td>
      <td>$1867.68</td>
    </tr>
    <tr>
      <th>1</th>
      <td>$2.82</td>
      <td>Female</td>
      <td>$2.82</td>
      <td>136</td>
      <td>$382.91</td>
    </tr>
    <tr>
      <th>2</th>
      <td>$3.25</td>
      <td>Other</td>
      <td>$3.25</td>
      <td>11</td>
      <td>$35.74</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Age Demographics

bins = [0, 9, 14, 19, 24, 29, 34, 39, 46]
labels_years = ["below 10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"]

# Create the bins in which Data will be held
# Bins are (0 < x <= 25), (25 < x <= 50), (50 < x <= 75), (75 < x <= 100)
# bins = [0, 25, 50, 75, 100]

demo = pd.cut(hero["Age"], bins, labels=labels_years)

# Place the data series into a new column inside of the DataFrame
hero["Age Group"] = demo

```


```python
# Create a GroupBy object based upon "Age Group"
demo_group = hero.groupby("Age Group")
```


```python
demo_loc = hero.loc[:,["Item Name", "Age Group", "Price"]]
```


```python
# Purchase Count
#demo_age_purchase_count = item_data.groupby(["Age Group", "Item Name"]).sum()["Price"].rename("Total Purchase Value")
#purchase_count = demo_loc.groupby(["Item Name"]).sum()["Price"].rename("Total Purchase Value")
#purchase_count = demo_loc["Age Group"].value_counts()

# # Average Purchase Price
# avg_purchase_age = demo_loc.groupeby["Price"].mean()
# avg_purchase_age
avg_purchase_age = demo_loc.groupby(["Age Group"]).mean()["Price"]
# # Total Purchase Value
# #Average price by age group multiplied by the number of purchases

total_count = demo_loc.groupby(["Age Group"]).count()["Price"]

total_value_age = purchase_count * avg_purchase_age
purchase_count
```




    20-24       336
    15-19       133
    25-29       125
    30-34        64
    35-39        42
    10-14        35
    below 10     28
    40+          17
    Name: Age Group, dtype: int64




```python

age_df = pd.DataFrame({
                       "Total": total_count, 
                       "Percentage of Players": total_count/573 * 100
                        
                      })

age_df["Percentage of Players"] = age_df["Percentage of Players"].map("{:.2f}%".format)
age_df
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
      <th>Percentage of Players</th>
      <th>Total</th>
    </tr>
    <tr>
      <th>Age Group</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>below 10</th>
      <td>4.89%</td>
      <td>28</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>6.11%</td>
      <td>35</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>23.21%</td>
      <td>133</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>58.64%</td>
      <td>336</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>21.82%</td>
      <td>125</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>11.17%</td>
      <td>64</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>7.33%</td>
      <td>42</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>2.97%</td>
      <td>17</td>
    </tr>
  </tbody>
</table>
</div>




```python
age_purchase_df = pd.DataFrame({
                       "Total Purchase Value": purchase_count, 
                       "Average Purchase Price": avg_purchase_age,   
                      })


age_purchase_df["Average Purchase Price"] = age_purchase_df["Average Purchase Price"].map("${:.2f}".format)
age_purchase_df["Total Purchase Value"] = age_purchase_df["Total Purchase Value"].map("${:.2f}".format)

age_purchase_df

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
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>10-14</th>
      <td>$2.77</td>
      <td>$35.00</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>$2.91</td>
      <td>$133.00</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>$2.91</td>
      <td>$336.00</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>$2.96</td>
      <td>$125.00</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>$3.08</td>
      <td>$64.00</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>$2.84</td>
      <td>$42.00</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>$3.16</td>
      <td>$17.00</td>
    </tr>
    <tr>
      <th>below 10</th>
      <td>$2.98</td>
      <td>$28.00</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Top Spenders

# Identify the the top 5 spenders in the game by total purchase value, then list (in a table):

# # SN
Top_spenders_2 = hero.groupby(["SN"]).sum()["Price"].rename("Total Purchase Value")

# Top_spenders = Top_spenders_2.sort_values("Price", ascending=False)
User_avg = hero.groupby(["SN"]).mean()["Price"].rename("Average Purchase Value")
# hero["Age"] = pd.cut(hero["Age"], bins, labels=labels_years)



# # Purchase Count
user_purchase_count = hero.groupby(["SN"]).count()["Price"].rename("Number of Purchases")
# #THIS ISN"t MATCHING THE EXAMPLE...
spenders_user_data = pd.DataFrame({"Top Spenders": Top_spenders_2,
                                   "Average Purchase Value": User_avg,
                                   "Number of Purchases": user_purchase_count
})
spenders_user_data["Average Purchase Value"] = spenders_user_data["Average Purchase Value"].map("${:.2f}".format)
spenders_user_data["Top Spenders"] = spenders_user_data["Top Spenders"]
x = spenders_user_data.sort_values("Top Spenders", ascending=False)
x.head()

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
      <th>Average Purchase Value</th>
      <th>Number of Purchases</th>
      <th>Top Spenders</th>
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
      <th>Undirrala66</th>
      <td>$3.41</td>
      <td>5</td>
      <td>17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>$3.39</td>
      <td>4</td>
      <td>13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>$3.18</td>
      <td>4</td>
      <td>12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>$4.24</td>
      <td>3</td>
      <td>12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>$3.86</td>
      <td>3</td>
      <td>11.58</td>
    </tr>
  </tbody>
</table>
</div>




```python
item_data = hero.loc[:, ['Item ID', 'Item Name', 'Price']]
```


```python
total_item_purchase = item_data.groupby(["Item ID", "Item Name"]).sum()["Price"].rename("Total Purchase Value")
```


```python
# Most Popular Items

# Identify the 5 most popular items by purchase count, then list (in a table):

# Item ID
# # Item Name
# # Purchase Count
# # Item Price
# # Total Purchase Value

item_data = hero.loc[:,["Item ID", "Item Name", "Price"]]
total_item_purchase = item_data.groupby(["Item ID", "Item Name"]).sum()["Price"].rename("Total Purchase Value")
item_avg = item_data.groupby(["Item ID", "Item Name"]).mean()["Price"].rename("Total Purchase Value")
item_count = item_data.groupby(["Item ID", "Item Name"]).count()["Price"].rename("Total Purchase Value")
                
most_pop_item = pd.DataFrame({"Total Purchase Value": total_item_purchase,
                              "Item Price": item_avg,
                              "Item Count": item_count
                             })

most_pop_item["Item Price"] = most_pop_item["Item Price"].map("${:.2f}".format)
most_pop_item["Total Purchase Value"] = most_pop_item["Total Purchase Value"]
most_pop_item_final = most_pop_item.sort_values("Item Count", ascending=False)
most_pop_item_final.head(5)

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
      <th>Item Count</th>
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
      <td>25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <th>Arcane Gem</th>
      <td>11</td>
      <td>$2.23</td>
      <td>24.53</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <td>9</td>
      <td>$2.07</td>
      <td>18.63</td>
    </tr>
    <tr>
      <th>175</th>
      <th>Woeful Adamantite Claymore</th>
      <td>9</td>
      <td>$1.24</td>
      <td>11.16</td>
    </tr>
    <tr>
      <th>13</th>
      <th>Serenity</th>
      <td>9</td>
      <td>$1.49</td>
      <td>13.41</td>
    </tr>
  </tbody>
</table>
</div>




```python
most_pop_item_final["Total Purchase Value"] = pd.to_numeric(most_pop_item_final["Total Purchase Value"])

most_profit_item = most_pop_item.sort_values("Total Purchase Value", ascending=False)
#Trying to format a bit because it won't sort with the dollar sign in there. inumerate will not work for me.
# most_pop_item_final["Total Purchase Value"] = most_pop_item_final["Total Purchase Value"].map("${:.2f}".format)
# most_pop_item["Total Purchase Value"] = most_pop_item["Total Purchase Value"].map("${:.2f}".format)

most_profit_item.head(5)
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
      <th>Item Count</th>
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
      <th>34</th>
      <th>Retribution Axe</th>
      <td>9</td>
      <td>$4.14</td>
      <td>37.26</td>
    </tr>
    <tr>
      <th>115</th>
      <th>Spectral Diamond Doomblade</th>
      <td>7</td>
      <td>$4.25</td>
      <td>29.75</td>
    </tr>
    <tr>
      <th>32</th>
      <th>Orenmir</th>
      <td>6</td>
      <td>$4.95</td>
      <td>29.70</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>6</td>
      <td>$4.87</td>
      <td>29.22</td>
    </tr>
    <tr>
      <th>107</th>
      <th>Splitter, Foe Of Subtlety</th>
      <td>8</td>
      <td>$3.61</td>
      <td>28.88</td>
    </tr>
  </tbody>
</table>
</div>


