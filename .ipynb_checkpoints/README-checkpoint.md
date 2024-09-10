# phase_1_project

# Objective:

To identify and assess the potential risks associated with different aircraft types for a new aviation business venture. The goal is to provide actionable insights to guide the purchase of aircraft that minimize risk and maximize profitability.

# Business Understanding

***Problem Statement:*** The aviation industry faces challenges related to aircraft accidents, leading to loss of life, property damage, and economic consequences.

***Objective:*** This analysis aims to gain insights into the factors contributing to aircraft accidents, identify areas for improvement, and inform safety recommendations.

***Data:*** The analysis will be based on a large dataset containing information about aircraft accidents.The columns below will be used to recommend the best aircraft to start a business venture: Aircraft Damage, Aircraft Category, Make, Total Fatal Injuries, Total Serious Injuries, Total Minor Injuries, Total Uninjured


## Import Libraries

import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
%matplotlib inline

## Read in the Dataset

### Using the pandas library read in the aviationdata.csv

pd.set_option('display.max_columns', None)
df = pd.read_csv('aviationdata.csv')
df

### Use the shape method toreturn a tuple representing the dimensions of the dataframe

df.shape

### The info method in pandas Dataframe provides a concise summary of the dataframe

df.info()

## Data Cleaning

### Create a copy of the dataframe

df1 = df.copy()
df1.sample(3)

### Filter the Data using the (Amateur.Built column equal to No) and the (Investigation.Type column  equal to Accident)

df1 = df1[(df1['Amateur.Built'] == 'No') & (df1['Investigation.Type'] == 'Accident')]
df1.head()

### Create a Dataframe from only the necessary columns

df1 = df1[['Aircraft.damage', 'Aircraft.Category', 'Make', 'Total.Fatal.Injuries', 'Total.Serious.Injuries', 'Total.Minor.Injuries', 'Total.Uninjured']]
df1

## Handling strucrural Errors

### The dtypes attribute in pandas Dataframe returns a series object containg the data types of each column in the Dataframe

df1.dtypes

### The str.replace() method in Pandas DataFrames is used to remove spaces and unwanted characters from strings within columns.


df1.columns = df1.columns.str.replace('.', '').str.replace(' ', '')

df1

### The str.title() method in Pandas DataFrames is used to convert the first letter of each word in a string to uppercase and the rest to lowercase.

df1 = df1.assign(Make=df1['Make'].str.title())
df1

# Dealing With Missing Data

### The .isna() method in Pandas DataFrames is used to check for missing values in a DataFrame. 

df1.isna().sum()

df1.shape

## Dropping Rows

### The dropna() method in Pandas DataFrames is used to remove rows or columns containing missing values.

df1 = df1.dropna()
df1

df1.shape

df1.isna().sum()

## Creating a new column Total Injured

### Create a column (TotalInjured) that contains the total value of the injured

df1 = df1.assign(TotalInjured=df1['TotalFatalInjuries'] + df1['TotalSeriousInjuries'] + df1['TotalMinorInjuries'])
df1

Total uninjured

mean_uninjured = round(df1['TotalUninjured'].mean())
mean_uninjured

std_uninjured = df1['TotalUninjured'].std()
std_uninjured

Total Injured

mean_injured = round(df1['TotalInjured'].mean())
mean_injured

std_injured = df1['TotalInjured'].mean()
std_injured

## Visualization

### Bar Graph visualizing the number of Total Injuries and Aircraft category

plt.figure(figsize=(8, 6))
plt.bar(df1['AircraftCategory'], df1['TotalInjured'])
plt.xlabel('Aircraft Category')
plt.ylabel('Total Injured')
plt.title('Total Injured by (Aircraft Category)')
plt.show()

X = df1['AircraftCategory'].sample(100).value_counts()
X

df1[df1['AircraftCategory'] == 'Airplane']

## Bar Graph

### A bar graph is a type of chart used to compare categorical data. 

df1 = df1.sample(100)
# Group data and count occurrences
damage_by_make = df1.groupby('Make')['Aircraftdamage'].value_counts().unstack()

# Create a stacked bar chart
damage_by_make.plot(kind='bar', stacked=True, figsize=(10, 6))
plt.title('Aircraft Damage by Make')
plt.xlabel('Make')
plt.ylabel('Count')
plt.legend(title='Aircraft Damage')
plt.show()

## Scatter Plot

### A scatter plot is a type of graph used to visualize the relationship between two numerical variables. 

# Calculate a measure of aircraft size based on total injuries
df1['AircraftSize'] = df1['TotalFatalInjuries'] + df1['TotalSeriousInjuries'] + df1['TotalMinorInjuries']

# Create a scatter plot
plt.figure(figsize=(12, 8))
plt.scatter(df1['AircraftSize'], df1['TotalFatalInjuries'])
plt.title('Relationship between Aircraft Size and Fatal Injuries')
xlabel = 'Aircraft Size (Inferred from Injuries)'
ylabel = 'Total Fatal Injuries'
plt.xlabel(xlabel)
plt.ylabel(ylabel)

# Calculate and display correlation coefficient
correlation = df1['AircraftSize'].corr(df1['TotalFatalInjuries'])
print(f"Correlation Coefficient: {correlation:.2f}")  # Format to 2 decimal places

plt.show()


The scatter plot shows a strong positive correlation between aircraft size (inferred from injuries) and the number of total fatal injuries. This suggests that larger aircraft tend to have more fatal accidents.

The correlation coefficient of 0.90 confirms this strong positive relationship. A value close to 1 indicates a strong positive correlation, while a value close to -1 indicates a strong negative correlation. In this case, the high positive correlation indicates that as aircraft size increases, the number of fatal injuries tends to increase as well. 

## Heat Map

### A heatmap is a 2D visualization technique that uses color to represent the magnitude of individual values within a dataset.



df1['AccidentRate'] = df1['TotalFatalInjuries'] / df1.index  # Assuming index represents aircraft ID

# Create a pivot table to aggregate data
pivot_data = df1.pivot_table(index='AircraftCategory', columns='Make', values='AccidentRate')

# Create a heatmap
sns.heatmap(pivot_data, annot=True, cmap='Blues')
plt.title('Aircraft Accident Rates')
plt.xlabel('Make')
plt.ylabel('Aircraft Category')
plt.show()

### Overall Trend:

Airplane category generally has higher accident rates compared to Helicopter and Balloon.
Helicopter category has the lowest accident rates overall.

### Make-Specific Observations:

Cessna and Piper have relatively high accident rates across multiple categories.
Bellanca and Swearingen have consistently low accident rates.
    

### Category-Specific Observations:

Airplane category shows a wide range of accident rates among different makes.
Helicopter category has more consistent accident rates, with most makes falling within a narrow range.
Balloon category has limited data, but the existing data suggests relatively low accident rates.

## Conclusion:

### Aircraft Category:

The category of aircraft significantly impacts accident rates. Airplanes generally have higher accident rates compared to helicopters and balloons.
Helicopter Safety: Helicopters demonstrate a lower overall accident rate, suggesting they might be inherently safer.

### Make-Specific Observations:

Cessna and Piper: These makes consistently appear in the higher-risk category, indicating potential design or operational issues.
Bellanca and Swearingen: These makes consistently show lower accident rates, suggesting they might have safer designs or operational practices.
