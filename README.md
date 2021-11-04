# Pandas Challenge - Heroes of Pymoli Data Analysis

Congratulations! After a lot of hard work in the data wrangling mines, you've landed a job as Lead Analyst for an independent gaming company. You've been assigned the task of analyzing the data for their most recent fantasy game Heroes of Pymoli.

Like many others in its genre, the game is free-to-play, but players are encouraged to purchase optional items that enhance their playing experience. As a first task, the company would like you to generate a report that breaks down the game's purchasing data into meaningful insights.

-----

# Observable Trends

* The majojrity of the population, 484 out of the 576 total unique players, were Male and totaled 84.03% of the total player count. 81 of 576 of the players were Female, making up 14.06% of the total player count. 11 out of the 576 total players reported Other/Non-Disclosed and were the smallest population, making up 1.91% of the population.

* The largest age demographic is between 20-24 years old, making up 44.79% of the total players. The second largest age group being between 15-19, totaling 18.58% of the population.

* The most popular items purchased were also the most profitable items. 'Final Critic" was purchased 13 times and has a total purchase value of $59.99. 'Oathbreaker, Last hope of the Breaking Storm' was purchased 12 times and has a total purchase value of $50.76.

-----

# Set up jupyter notebook

```python
# Dependencies and Setup
import pandas as pd
import numpy as np

# File to Load (Remember to Change These)
file_to_load = 'Resources/purchase_data.csv'

# Read Purchasing File and store into Pandas data frame
purchase_data = pd.read_csv(file_to_load)

# Display a sample purchasing data
purchase_data.sample(10)
```

-----

# Player Count

* Display total number of players

```Python
# Calculate unique total players by 'SN'
total_SN = total_SN=len(purchase_df['SN'].unique())
total_SN

# Create a dataframe to display total players
player_count=pd.DataFrame({'Total Players':[total_SN]})
player_count
```

-----

# Purchasing Analysis (Total)

* Run basic calculations to obtain number of unique items, average price, etc.
* Create a summary data frame to hold the results
* Optional: give the displayed data cleaner formatting
* Display the summary data frame

```Python
# Calculate number of unique items
unique_items=len(purchase_df['Item Name'].unique())
unique_items

# Calculate average price
average_price=purchase_df['Price'].mean()
average_price

# Calculate number of purchases
purchase_count=len(purchase_df['Purchase ID'].unique())
purchase_count

# Calculate total revenue
total_revenue=purchase_df['Price'].sum()
total_revenue

# Create a dataframe of purchasing analysis
purchasing_analysis=pd.DataFrame({'Number of Unique Items':[unique_items],
                                    'Average Price':[average_price],
                                    'Number of Purchases':[purchase_count],
                                     'Total Revenue':[total_revenue]
                                    })
purchasing_analysis

# Create a purchasing analysis dataframe with cleaner formatting
purchasing_analysis['Average Price'] = purchasing_analysis['Average Price'].map('${:,.2f}'.format)
purchasing_analysis['Total Revenue'] = purchasing_analysis['Total Revenue'].map('${:,.2f}'.format)
purchasing_analysis
```
-----


# Gender Demographics

* Percentage and Count of Male Players

* Percentage and Count of Female Players

* Percentage and Count of Other / Non-Disclosed

```Python
# Create dataframe to focus on player data
player_data = purchase_df[['SN','Age','Gender']].copy()
player_data.sample(10)

# Remove duplicates from player data
player_data.drop_duplicates(inplace=True)

# Calculate the total number of unique players by 'Gender'
gender_breakdown = player_data['Gender'].value_counts()
gender_count = pd.DataFrame(gender_breakdown)
gender_count

# Calculate the percentage breakdown by gender
gender_percentage = (gender_total/total_SN)*100
gender_percentage

# Create dataframe summarizing gender demographics
gender_count['Percentage of Players'] = gender_percentage
gender_demographics = gender_count
gender_demographics

# Create gender demographics dataframe with cleaner formatting 
gender_demographics['Percentage of Players'] = gender_demographics['Percentage of Players'].map('{:.2f}%'.format)
gender_demographics
```

-----

# Purchasing Analysis (Gender) 

* Run basic calculations to obtain purchase count, avg. purchase price, avg. purchase total per person etc. by gender

* Create a summary data frame to hold the results

* Optional: give the displayed data cleaner formatting

* Display the summary data frame

```Python
# Create gender group
gender_group = purchase_data.groupby(['Gender'])

# Calculate purchase count by gender
purchase_count = gender_group['Purchase ID'].count()
purchase_count

# Calculate average purchase price by gender
average_purchase_price = gender_group['Price'].mean()
average_purchase_price

# Calculate total purchase value by gender
total_purchase_value = round((gender_group['Price'].sum()),2)
total_purchase_value

# Calculate average total purchase per person by gender
average_total = total_purchase_value/gender_breakdown
average_total

# Create a dataframe of purchasing analysis (gender)
purchasing_analysis_gender = pd.DataFrame({'Purchase Count': purchase_count,
                                           'Average Purchase Price': average_purchase_price,
                                           'Total Purchase Value': total_purchase_value,
                                           'Avg Total Purchase per Person': average_total,
                                          })
purchasing_analysis_gender

# Create purchasing analysis (gender) with cleaner formatting
purchasing_analysis_gender['Average Purchase Price'] = purchasing_analysis_gender['Average Purchase Price'].map('${:,.2f}'.format)
purchasing_analysis_gender['Total Purchase Value'] = purchasing_analysis_gender['Total Purchase Value'].map('${:,.2f}'.format)
purchasing_analysis_gender['Avg Total Purchase per Person'] = purchasing_analysis_gender['Avg Total Purchase per Person'].map('${:,.2f}'.format)
purchasing_analysis_gender
```

-----

# Age Demographics
* Establish bins for ages

* Categorize the existing players using the age bins. Hint: use pd.cut()

*Calculate the numbers and percentages by age group

* Create a summary data frame to hold the results

* Optional: round the percentage column to two decimal points

* Display Age Demographics Table

```Python
# Create bins and labels for ages
age_bins = [0, 9.99, 14.99, 19.99, 24.99, 29.9, 34.99, 39.99, 999999]
age_labels = ['<10', '10-14', '15-19', '20-24', '25-29', '30-34', '35-39', '40+']

# Categorize players using the created bins and labels
purchase_data['Age Labels'] = pd.cut(purchase_data['Age'],age_bins,labels=age_labels)
purchase_data

# Create an age group
age_group = purchase_data.groupby(['Age Labels'])

# Calculate total number of players by new 'Age Label' category
age_unique = age_group['SN'].nunique()
age_total = pd.DataFrame(age_unique)
age_total

# Calculate the percentage breakdown by new 'Age Label' category
age_percentage = pd.DataFrame((age_total/total_SN)*100)
age_percentage

# Merge age_total and age_percentage into one dataframe to display age demographics
age_demographics = pd.merge(age_total, age_percentage, on='Age Labels')
age_demographics

# Create an age demographics data frame with cleaner formatting
age_demographics = age_demographics.rename(columns={'SN_x':'Total Count',
                                                  'SN_y':'Percentage of Players'
                                                  })
age_demographics['Percentage of Players'] = age_demographics['Percentage of Players'].map('{:.2f}%'.format)
age_demographics
```

-----


# Purchasing Analysis (Age)

* Bin the purchase_data data frame by age

* Run basic calculations to obtain purchase count, avg. purchase price, avg. purchase total per person etc. in the table below

* Create a summary data frame to hold the results

* Optional: give the displayed data cleaner formatting

* Display the summary data frame

```Python
# Calculate purchase count by age group
purchase_count_age = pd.DataFrame(age_group['Purchase ID'].count())
purchase_count_age

# Calculate average purchase price by age group 
average_purchase_price_age = pd.DataFrame(age_group['Price'].mean())
average_purchase_price_age

# Calculate total purchase value by age group
age_total_purchase_value = age_group['Price'].sum()
total_purchase_value_age = pd.DataFrame(age_total_purchase_value)
total_purchase_value_age

# Calculate average purchase total per person by age group
age_average_total = age_total_purchase_value/age_unique
average_total_age = pd.DataFrame(age_average_total)
average_total_age

# Merge calculated age dataframes to create a purchasing analysis (age) dataframe
merge_1 = pd.merge(purchase_count_age, average_purchase_price_age, on='Age Labels')
purchasing_analysis_age = pd.merge(merge_1, total_purchase_value_age, on='Age Labels')
purchasing_analysis_age

# Create a purchasing analysis (age) dataframe with cleaner formatting
purchasing_analysis_age = purchasing_analysis_age.rename(columns={'Purchase ID':'Purchase Count',
                                                              'Price_x':'Average Purchase Price',
                                                              'Price_y':'Total Purchase Value',
                                                             })
purchasing_analysis_age['Avg Purchase Total per Person'] = age_average_total
purchasing_analysis_age

# Create purchasing analysis (age) dataframe with cleaner formatting
purchasing_analysis_age['Average Purchase Price'] = purchasing_analysis_age['Average Purchase Price'].map('${:,.2f}'.format)
purchasing_analysis_age['Total Purchase Value'] = purchasing_analysis_age['Total Purchase Value'].map('${:,.2f}'.format)
purchasing_analysis_age['Avg Purchase Total per Person'] = purchasing_analysis_age['Avg Purchase Total per Person'].map('${:,.2f}'.format)
purchasing_analysis_age
```

-----

# Top Spenders

* Run basic calculations to obtain the results in the table below
* Create a summary data frame to hold the results
* Sort the total purchase value column in descending order
* Optional: give the displayed data cleaner formatting
* Display a preview of the summary data frame

```Python
# Create purchase data group
spender_data = purchase_data.groupby('SN')

# Calculate the purchase count by player
player_purchase = pd.DataFrame(spender_data['Purchase ID'].count())
player_purchase

# Calculate the average purchase price by player
player_average = pd.DataFrame(spender_data['Price'].mean())
player_average

# Calculate the total purchase value by player
player_total = pd.DataFrame(spender_data['Price'].sum())
player_total

# Merge calculated spender data to create a dataframe that displays top spenders
merge1 = pd.merge(player_purchase, player_average, on='SN')
top_spenders = pd.merge(merge1, player_total, on='SN')
top_spenders

# Create a top spenders dataframe with cleaner formatting
top_spenders = top_spenders.rename(columns={'Purchase ID':'Purchase Count',
                                            'Price_x':'Average Purchase Price',
                                            'Price_y':'Total Purchase Value'
                                           })
top_spenders['Purchase Count'] = top_spenders['Purchase Count'].map('${:,.2f}'.format)
top_spenders['Average Purchase Price'] = top_spenders['Average Purchase Price'].map('${:,.2f}'.format)
top_spenders['Total Purchase Value'] = top_spenders['Total Purchase Value'].map('${:,.2f}'.format)
sorted_spenders = top_spenders.sort_values(['Total Purchase Value'], ascending=False).head()
sorted_spenders
```

-----

# Most Popular Items

* Retrieve the Item ID, Item Name, and Item Price columns

* Group by Item ID and Item Name. Perform calculations to obtain purchase count, average item price, and total purchase value

* Create a summary data frame to hold the results

* Sort the purchase count column in descending order

* Optional: give the displayed data cleaner formatting

*Display a preview of the summary data frame

```Python
# Retrieve the Item ID, Item Name, and Item Price columns
item_data = purchase_data[['Item ID','Item Name','Price']]
item_data

# Group by Item ID and Item Name
item_groups = item_data.groupby(['Item ID','Item Name'])

# Calculate the purchase count by item
item_count = item_groups['Price'].count()
item_purchase = pd.DataFrame(item_count)
item_purchase

# Calculate the total purchase value by item
item_total = item_groups['Price'].sum()
item_total.max()

# Calculate price by item
item_price = item_total/item_count

# Create a dataframe that displays most popular items
item_purchase ['Item Price'] = item_price
item_purchase ['Item Total'] = item_total
item_purchase

# Create a popular items dataframe with cleaner formatting
item_purchase = item_purchase.rename(columns={'Price':'Purchase Count',
                                            'Item Total':'Total Purchase Value',
                                           })
item_purchase['Item Price'] = item_purchase['Item Price'].map('${:,.2}'.format)
item_purchase.style.format({'Total Purchase Value':'${:,.2f}'})
popular_items = item_purchase.sort_values(['Purchase Count'], ascending=False).head()
popular_items
```

-----

# Most Profitable Items

* Sort the above table by total purchase value in descending order

* Optional: give the displayed data cleaner formatting

* Display a preview of the data frame

```Python
# Sort the popular items data frame by total purchase value in descending order
profitable_items = popular_items.sort_values(['Total Purchase Value'],ascending=False).head()
profitable_items

# Create a profitable items dataframe with cleaner formatting
profitable_items = popular_items.sort_values(['Total Purchase Value'],ascending=False).head()
profitable_items.style.format({"Total Purchase Value":"${:,.2f}"})
profitable_items
```