# basecamp.team15
welcome

# Basecamp Week 2

# Open the NHS data and call it data
import pandas as pd
data = pd.read_csv("nhs_talking_therapies_regions_2024_2025.csv", na_values=["*"])
data.head()

# Look: how big is it, what's in each column, what looks odd?
print(data.shape)
data.info()
data.describe()

# Blanks: count them first, then decide for each column
data.isna().sum()

# Drop rows where an important column is blank
data = data.dropna(subset=["MEASURE_VALUE"])

# Count the blank cells again
data.isna().sum()

# Repeated rows: count, then remove
print(data.duplicated().sum())
data = data.drop_duplicates()

# Spelling: see every version, then tidy the names
print(data["ORG_NAME1"].unique())
data["ORG_NAME1"] = data["ORG_NAME1"].str.strip()

# Check that the values are numbers
data.info()
data["MEASURE_VALUE"].describe()

# Dates: match the format to your data
data["REPORTING_PERIOD_START"] = pd.to_datetime(data["REPORTING_PERIOD_START"], format="%Y-%m-%d")

# Dates: match the format to your data
data["REPORTING_PERIOD_END"] = pd.to_datetime(data["REPORTING_PERIOD_END"], format="%Y-%m-%d")

# Check that the dates are now real dates
data.info()

# Impossible values: keep only rows that could be true
data = data[data["MEASURE_VALUE"] >= 0]
print(data.shape)

# Save the clean dataset
data.to_csv("nhs_talking_therapies_clean.csv", index=False)
print("Saved", len(data), "rows to nhs_talking_therapies_clean.csv")

# Cleaning Decision Log
# To be completed

# Basecamp Week 4

import matplotlib.pyplot as plt

# Keep only monthly referrals from commissioning regions
referrals = data[
    (data["MEASURE_NAME"] == "Count_ReferralsReceived")
    & (data["GROUP_TYPE"] == "CommissioningRegion")
    & (data["ORG_NAME1"] != "Unknown")
].copy()

print(referrals.shape)

# Draw a histogram of referrals
referrals ["MEASURE_VALUE"].plot(kind="hist", bins=30, title="NHS Talking Therapies referrals by region")
plt.xlabel("Number of referrals per region per month")
plt.ylabel("Number of rows")
plt.show()

print("Median:", referrals["MEASURE_VALUE"].median())
print("Middle half:", referrals["MEASURE_VALUE"].quantile(0.25), "to", referrals["MEASURE_VALUE"].quantile(0.75))

# Median monthly referrals in each region, lowest first
by_group = referrals.groupby("ORG_NAME1")["MEASURE_VALUE"].median().sort_values()

by_group.plot(kind="barh", title="Median monthly NHS Talking Therapies referrals by region")
plt.xlabel("Median number of referrals per month")
plt.ylabel("Region")
plt.show()

# How have monthly referrals changed over time?
referrals["REPORTING_PERIOD_START"] = pd.to_datetime(referrals["REPORTING_PERIOD_START"])

monthly = referrals.set_index("REPORTING_PERIOD_START")["MEASURE_VALUE"].resample("MS").median()

monthly.plot(title="Median monthly NHS Talking Therapies referrals across regions")
plt.ylabel("Median referrals per region")
plt.show()

q1 = referrals["MEASURE_VALUE"].quantile(0.25)
q3 = referrals["MEASURE_VALUE"].quantile(0.75)
width = q3 - q1
lower = q1 - 1.5 * width
upper = q3 + 1.5 * width

outliers = referrals[(referrals["MEASURE_VALUE"] < lower) | (referrals["MEASURE_VALUE"] > upper)]
print(len(outliers), "rows outside the fences")
outliers

# Open the NHS data and call it data
import pandas as pd
data = pd.read_csv("nhs_talking_therapies_regions_2024_2025.csv", na_values=["*"])
data.head()

# Look: how big is it, what's in each column, what looks odd?
print(data.shape)
data.info()
data.describe()

# Blanks: count them first, then decide for each column
data.isna().sum()

# Drop rows where an important column is blank
data = data.dropna(subset=["MEASURE_VALUE"])

# Count the blank cells again
data.isna().sum()

# Repeated rows: count, then remove
print(data.duplicated().sum())
data = data.drop_duplicates()

# Spelling: see every version, then tidy the names
print(data["ORG_NAME1"].unique())
data["ORG_NAME1"] = data["ORG_NAME1"].str.strip()

# Check that the values are numbers
data.info()
data["MEASURE_VALUE"].describe()

# Dates: match the format to your data
data["REPORTING_PERIOD_START"] = pd.to_datetime(data["REPORTING_PERIOD_START"], format="%Y-%m-%d")

# Dates: match the format to your data
data["REPORTING_PERIOD_END"] = pd.to_datetime(data["REPORTING_PERIOD_END"], format="%Y-%m-%d")

# Check that the dates are now real dates
data.info()

# Impossible values: keep only rows that could be true
data = data[data["MEASURE_VALUE"] >= 0]
print(data.shape)

# Save the clean dataset
data.to_csv("nhs_talking_therapies_clean.csv", index=False)
print("Saved", len(data), "rows to nhs_talking_therapies_clean.csv")

# Cleaning Decision Log
# To be completed

# Basecamp Week 4

import matplotlib.pyplot as plt

# Keep only monthly referrals from commissioning regions
referrals = data[
    (data["MEASURE_NAME"] == "Count_ReferralsReceived")
    & (data["GROUP_TYPE"] == "CommissioningRegion")
    & (data["ORG_NAME1"] != "Unknown")
].copy()

print(referrals.shape)

# Draw a histogram of referrals
referrals ["MEASURE_VALUE"].plot(kind="hist", bins=30, title="NHS Talking Therapies referrals by region")
plt.xlabel("Number of referrals per region per month")
plt.ylabel("Number of rows")
plt.show()

print("Median:", referrals["MEASURE_VALUE"].median())
print("Middle half:", referrals["MEASURE_VALUE"].quantile(0.25), "to", referrals["MEASURE_VALUE"].quantile(0.75))

# Median monthly referrals in each region, lowest first
by_group = referrals.groupby("ORG_NAME1")["MEASURE_VALUE"].median().sort_values()

by_group.plot(kind="barh", title="Median monthly NHS Talking Therapies referrals by region")
plt.xlabel("Median number of referrals per month")
plt.ylabel("Region")
plt.show()

# How have monthly referrals changed over time?
referrals["REPORTING_PERIOD_START"] = pd.to_datetime(referrals["REPORTING_PERIOD_START"])

monthly = referrals.set_index("REPORTING_PERIOD_START")["MEASURE_VALUE"].resample("MS").median()

monthly.plot(title="Median monthly NHS Talking Therapies referrals across regions")
plt.ylabel("Median referrals per region")
plt.show()

q1 = referrals["MEASURE_VALUE"].quantile(0.25)
q3 = referrals["MEASURE_VALUE"].quantile(0.75)
width = q3 - q1
lower = q1 - 1.5 * width
upper = q3 + 1.5 * width

outliers = referrals[(referrals["MEASURE_VALUE"] < lower) | (referrals["MEASURE_VALUE"] > upper)]
print(len(outliers), "rows outside the fences")
outliers

# Monthly referrals by commissioning region
referrals.boxplot(column="MEASURE_VALUE", by="ORG_NAME1", grid=False, rot=90)
plt.suptitle("")
plt.title("Monthly NHS Talking Therapies referrals by region")
plt.ylabel("Number of referrals per month")
plt.show()
