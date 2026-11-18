# basecamp.team15
welcome

# Download depression dataset
import urllib.request
urllib.request.urlretrieve("https://data.cdc.gov/resource/8pt5-q6wp.csv?$limit=50000", "depression.csv")
print("Downloaded depression.csv")

# Open and rename dataset
import pandas as pd
data = pd.read_csv("Indicators_of_Anxiety_or_Depression_Based_on_Reported_Frequency_of_Symptoms_During_Last_7_Days_20261118.csv")
data.head()

# How many rows and columns?
data.shape

# What kind of data is in each column
data.info()

# Quick numbers for each number column
data.describe()

# Count the blank cells in each column
data.isna().sum()
