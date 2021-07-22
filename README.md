## Pandas-Challenge
#!/usr/bin/env python
# coding: utf-8

# In[382]:


# Dependencies and Setup
import pandas as pd
import numpy as np

# File to Load (Remember to Change These)
file_to_load = 'Resources/purchase_data.csv'

# Read Purchasing File and store into Pandas data frame
purchase_data = pd.read_csv(file_to_load)


# In[383]:


# Display a sample purchasing data
purchase_data.sample(10)


# # Player Count
# - Display total number of players

# In[384]:


# Calculate unique total players by 'SN'
total_SN = total_SN=len(purchase_df['SN'].unique())
total_SN


# In[385]:


# Create a dataframe to display total players
player_count=pd.DataFrame({'Total Players':[total_SN]})
player_count


# # Purchasing Analysis (Total)
# - Run basic calculations to obtain number of unique items, average price, etc.
# - Create a summary data frame to hold the results
# - Optional: give the displayed data cleaner formatting
# - Display the summary data frame

# In[386]:


# Calculate number of unique items
unique_items=len(purchase_df['Item Name'].unique())
unique_items


# In[387]:


# Calculate average price
average_price=purchase_df['Price'].mean()
average_price


# In[388]:


# Calculate number of purchases
purchase_count=len(purchase_df['Purchase ID'].unique())
purchase_count


# In[389]:


# Calculate total revenue
total_revenue=purchase_df['Price'].sum()
total_revenue


# In[390]:


# Create a dataframe of purchasing analysis
purchasing_analysis=pd.DataFrame({'Number of Unique Items':[unique_items],
                                    'Average Price':[average_price],
                                    'Number of Purchases':[purchase_count],
                                     'Total Revenue':[total_revenue]
                                    })
purchasing_analysis


# In[391]:


# Create a purchasing analysis dataframe with cleaner formatting
purchasing_analysis['Average Price'] = purchasing_analysis['Average Price'].map('${:,.2f}'.format)
purchasing_analysis['Total Revenue'] = purchasing_analysis['Total Revenue'].map('${:,.2f}'.format)
purchasing_analysis


# # Gender Demographics
# - Percentage and Count of Male Players
# - Percentage and Count of Female Players
# - Percentage and Count of Other / Non-Disclosed

# In[392]:


# Create dataframe to focus on player data
player_data = purchase_df[['SN','Age','Gender']].copy()
player_data.sample(10)


# In[393]:


# Remove duplicates from player data
player_data.drop_duplicates(inplace=True)


# In[394]:


# Calculate the total number of unique players by 'Gender'
gender_breakdown = player_data['Gender'].value_counts()
gender_count = pd.DataFrame(gender_breakdown)
gender_count


# In[395]:


# Calculate the percentage breakdown by gender
gender_percentage = (gender_total/total_SN)*100
gender_percentage


# In[396]:


# Create dataframe summarizing gender demographics
gender_count['Percentage of Players'] = gender_percentage
gender_demographics = gender_count
gender_demographics


# In[397]:


# Create gender demographics dataframe with cleaner formatting 
gender_demographics['Percentage of Players'] = gender_demographics['Percentage of Players'].map('{:.2f}%'.format)
gender_demographics


# # Purchasing Analysis (Gender)
# - Run basic calculations to obtain purchase count, avg. purchase price, avg. purchase total per person etc. by gender
# - Create a summary data frame to hold the results
# - Optional: give the displayed data cleaner formatting
# - Display the summary data frame

# In[398]:


# Create gender group
gender_group = purchase_data.groupby(['Gender'])


# In[399]:


# Calculate purchase count by gender
purchase_count = gender_group['Purchase ID'].count()
purchase_count


# In[400]:


# Calculate average purchase price by gender
average_purchase_price = gender_group['Price'].mean()
average_purchase_price


# In[401]:


# Calculate total purchase value by gender
total_purchase_value = round((gender_group['Price'].sum()),2)
total_purchase_value


# In[402]:


# Calculate average total purchase per person by gender
average_total = total_purchase_value/gender_breakdown
average_total


# In[403]:


# Create a dataframe of purchasing analysis (gender)
purchasing_analysis_gender = pd.DataFrame({'Purchase Count': purchase_count,
                                           'Average Purchase Price': average_purchase_price,
                                           'Total Purchase Value': total_purchase_value,
                                           'Avg Total Purchase per Person': average_total,
                                          })
purchasing_analysis_gender


# In[404]:


# Create purchasing analysis (gender) with cleaner formatting
purchasing_analysis_gender['Average Purchase Price'] = purchasing_analysis_gender['Average Purchase Price'].map('${:,.2f}'.format)

purchasing_analysis_gender['Total Purchase Value'] = purchasing_analysis_gender['Total Purchase Value'].map('${:,.2f}'.format)

purchasing_analysis_gender['Avg Total Purchase per Person'] = purchasing_analysis_gender['Avg Total Purchase per Person'].map('${:,.2f}'.format)

purchasing_analysis_gender


# # Age Demographics
# - Establish bins for ages
# - Categorize the existing players using the age bins. Hint: use pd.cut()
# - Calculate the numbers and percentages by age group
# - Create a summary data frame to hold the results
# - Optional: round the percentage column to two decimal points
# - Display Age Demographics Table

# In[405]:


# Create bins and labels for ages
age_bins = [0, 9.99, 14.99, 19.99, 24.99, 29.9, 34.99, 39.99, 999999]
age_labels = ['<10', '10-14', '15-19', '20-24', '25-29', '30-34', '35-39', '40+']


# In[406]:


# Categorize players using the created bins and labels
purchase_data['Age Labels'] = pd.cut(purchase_data['Age'],age_bins,labels=age_labels)
purchase_data


# In[407]:


# Create an age group
age_group = purchase_data.groupby(['Age Labels'])


# In[408]:


# Calculate total number of players by new 'Age Label' category
age_unique = age_group['SN'].nunique()

age_total = pd.DataFrame(age_unique)
age_total


# In[409]:


# Calculate the percentage breakdown by new 'Age Label' category
age_percentage = pd.DataFrame((age_total/total_SN)*100)
age_percentage


# In[410]:


# Merge age_total and age_percentage into one dataframe to display age demographics
age_demographics = pd.merge(age_total, age_percentage, on='Age Labels')
age_demographics


# In[411]:


# Create an age demographics data frame with cleaner formatting
age_demographics = age_demographics.rename(columns={'SN_x':'Total Count',
                                                  'SN_y':'Percentage of Players'
                                                  })
age_demographics['Percentage of Players'] = age_demographics['Percentage of Players'].map('{:.2f}%'.format)

age_demographics


# # Purchasing Analysis (Age)
# - Bin the purchase_data data frame by age
# - Run basic calculations to obtain purchase count, avg. purchase price, avg. purchase total per person etc. in the table below
# - Create a summary data frame to hold the results
# - Optional: give the displayed data cleaner formatting
# - Display the summary data frame

# In[412]:


# Calculate purchase count by age group
purchase_count_age = pd.DataFrame(age_group['Purchase ID'].count())
purchase_count_age


# In[413]:


# Calculate average purchase price by age group 
average_purchase_price_age = pd.DataFrame(age_group['Price'].mean())
average_purchase_price_age


# In[414]:


# Calculate total purchase value by age group
age_total_purchase_value = age_group['Price'].sum()

total_purchase_value_age = pd.DataFrame(age_total_purchase_value)
total_purchase_value_age


# In[415]:


# Calculate average purchase total per person by age group
age_average_total = age_total_purchase_value/age_unique

average_total_age = pd.DataFrame(age_average_total)
average_total_age


# In[416]:


# Merge calculated age dataframes to create a purchasing analysis (age) dataframe
merge_1 = pd.merge(purchase_count_age, average_purchase_price_age, on='Age Labels')


purchasing_analysis_age = pd.merge(merge_1, total_purchase_value_age, on='Age Labels')
purchasing_analysis_age


# In[417]:


# Create a purchasing analysis (age) dataframe with cleaner formatting
purchasing_analysis_age = purchasing_analysis_age.rename(columns={'Purchase ID':'Purchase Count',
                                                              'Price_x':'Average Purchase Price',
                                                              'Price_y':'Total Purchase Value',
                                                             })
purchasing_analysis_age['Avg Purchase Total per Person'] = age_average_total

purchasing_analysis_age


# In[418]:


# Create purchasing analysis (age) dataframe with cleaner formatting
purchasing_analysis_age['Average Purchase Price'] = purchasing_analysis_age['Average Purchase Price'].map('${:,.2f}'.format)
purchasing_analysis_age['Total Purchase Value'] = purchasing_analysis_age['Total Purchase Value'].map('${:,.2f}'.format)
purchasing_analysis_age['Avg Purchase Total per Person'] = purchasing_analysis_age['Avg Purchase Total per Person'].map('${:,.2f}'.format)

purchasing_analysis_age


# # Top Spenders
# - Run basic calculations to obtain the results in the table below
# - Create a summary data frame to hold the results
# - Sort the total purchase value column in descending order
# - Optional: give the displayed data cleaner formatting
# - Display a preview of the summary data frame

# In[419]:


# Create purchase data group
spender_data = purchase_data.groupby('SN')


# In[420]:


# Calculate the purchase count by player
player_purchase = pd.DataFrame(spender_data['Purchase ID'].count())

player_purchase


# In[421]:


# Calculate the average purchase price by player
player_average = pd.DataFrame(spender_data['Price'].mean())
player_average


# In[422]:


# Calculate the total purchase value by player
player_total = pd.DataFrame(spender_data['Price'].sum())
player_total


# In[423]:


# Merge calculated spender data to create a dataframe that displays top spenders
merge1 = pd.merge(player_purchase, player_average, on='SN')

top_spenders = pd.merge(merge1, player_total, on='SN')
top_spenders


# In[424]:


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


# # Most Popular Items
# - Retrieve the Item ID, Item Name, and Item Price columns
# - Group by Item ID and Item Name. Perform calculations to obtain purchase count, average item price, and total purchase value
# - Create a summary data frame to hold the results
# - Sort the purchase count column in descending order
# - Optional: give the displayed data cleaner formatting
# - Display a preview of the summary data frame

# In[702]:


# Retrieve the Item ID, Item Name, and Item Price columns
item_data = purchase_data[['Item ID','Item Name','Price']]
item_data


# In[703]:


# Group by Item ID and Item Name
item_groups = item_data.groupby(['Item ID','Item Name'])


# In[704]:


# Calculate the purchase count by item
item_count = item_groups['Price'].count()

item_purchase = pd.DataFrame(item_count)
item_purchase


# In[705]:


# Calculate the total purchase value by item
item_total = item_groups['Price'].sum()
item_total.max()


# In[706]:


# Calculate price by item
item_price = item_total/item_count


# In[707]:


# Create a dataframe that displays most popular items
item_purchase ['Item Price'] = item_price
item_purchase ['Item Total'] = item_total
item_purchase


# In[708]:


# Create a popular items dataframe with cleaner formatting
item_purchase = item_purchase.rename(columns={'Price':'Purchase Count',
                                            'Item Total':'Total Purchase Value',
                                           })

item_purchase['Item Price'] = item_purchase['Item Price'].map('${:,.2}'.format)
item_purchase.style.format({'Total Purchase Value':'${:,.2f}'})

popular_items = item_purchase.sort_values(['Purchase Count'], ascending=False).head()

popular_items


# # Most Profitable Items
# - Sort the above table by total purchase value in descending order
# - Optional: give the displayed data cleaner formatting
# - Display a preview of the data frame

# In[709]:


# Sort the popular items data frame by total purchase value in descending order
profitable_items = popular_items.sort_values(['Total Purchase Value'],ascending=False).head()
profitable_items


# In[711]:


# Create a profitable items dataframe with cleaner formatting
profitable_items = popular_items.sort_values(['Total Purchase Value'],ascending=False).head()
profitable_items.style.format({"Total Purchase Value":"${:,.2f}"})

profitable_items

