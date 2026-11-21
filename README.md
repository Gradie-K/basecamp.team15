# basecamp.team15
welcome

# Download depression dataset
import urllib.request
urllib.request.urlretrieve("https://files.digital.nhs.uk/37/BB8EC9/nhstalkingtherapies_time_series_jan_2024_jan_2025_key_measures.csv", "depression_uk.csv")
print("Downloaded depression_uk.csv")

# Open the rename dataset
import pandas as pd
data = pd.read_csv("nhs_talking_therapies_regions_2024_2025.csv")
data.head()

# How many rows and columns?
data.shape

# What kind of data is in each column
data.info()

# Quick numbers for each number column
data.describe()

# Count the blank cells in each column
data.isna().sum()

# Check which rows have suppressed values
data[data[“MEASURE_VALUE”] == “*”]

# Convert the values into numbers
data[“MEASURE_VALUE”] = pd.to_numeric(data[“MEASURE_VALUE’], errors=“coerce”)

# Count the blank cells again
data.isna().sum()

# Drop the rows with no measure value
data = data.dropna(subset=[“MEASURE_VALUE”])

# Check the remaining rows
data.shape

# How many rows are exact copies of an earlier row?
data.duplicated().sum()

# Keep the first copy of each row and drop the repeats
data = data.drop_duplicates()
data.duplicated().sum()

# Every spelling and how many times each appears
data[“ORG_NAME1”].value_counts()

# Tidy the space and capitals
data{“ORG_NAME1”] = data[“ORG_NAME1].str.trip().str.title()

data[“ORG_NAME1”].value_counts()

# Check that the values are now numbers
data.info()

#  Check the numerical values
data[“MEASURE_VALUE”].describe()

# Turn the text into real dates. %Y-%m-%d means year/month/day
data[“REPORTING_PERIOD_START] = pd.to_datetime(data[“REPORTING_PERIOD_START”], format=%Y-%m-%d”)

data[“REPORTING_PERIOD_START”}.describe()

# Turn the text into real dates. %Y-%m-%d means year/month/day
data[“REPORTING_PERIOD_END`] = pd.to_datetime(data[“REPORTING_PERIOD_END”], format=%Y-%m-%d”)

data[“REPORTING_PERIOD_END”}.describe()

# Check that the dates are now real dates
data.info()

# Count the impossible rows first
print("Negative values", (data["MEASURE_VALUE"] < 0).sum())
print("Dates in the future:", (data["REPORTING_PERIOD_START"] > pd.Timestamp.today()).sum())
print("Start date after end date:", (data["REPORTING_PERIOD_START"] > data["REPORTING_PERIOD_END"]).sum())

# Keep only the rows that could be true
data = data[(data[“MEASURE_VALUE”] >= 0) & (data[REPORTING_PERIOD_START”] <= pd.Tiestamp.today()) & (data[“REPORTING_PERIOD_START”] <= dat[“REPORtING_PERIOD_END])] l.copy()

data.shape

# Save the clean dataset
data.to_csv(“depression_uk_clean.csv”, index=False)
print(“Saved”c len(data), “rows to depression_uk_clean.csv”)

# Cleaning Decision Log
