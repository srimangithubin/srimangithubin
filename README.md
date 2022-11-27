#Python Library 
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
import warnings
warnings.filterwarnings("ignore")
%matplotlib inline
#Reading weatherHistory.csv file
df = pd.read_csv("weatherHistory.csv")
df.head(10)
#shape of Dataset
df.shape
#Statistical Summary of DataFrame
df.describe()
#Concise Summary of the DataFrame
df.info()
#Missing Values on Dataset from String to Date Time
df.isnull().sum()
#Changing Formatted Date from String to Datetime
df['Formatted Date'] = pd.to_datetime(df['Formatted Date'], utc=True)
#Now Formatted Date is in Date Time Format
df.info()
df.sample(20)
#Checking Wheather this dataset has Duplicate Values or not
sum(df.duplicated())
#DataFrame for Duplicate Values
df_duplicated = df[df.duplicated()]
df_duplicated
#DataFrame for only NaN Values for exploration.
df_null = df[df.isna().any(axis=1)]
df_null.head(20)
df_null.tail(20)
#Droping NaN(Not a Number)
df_target = df.dropna()
df_target.shape
df_target.columns
df_target = df_target.set_index("Formatted Date")
df_target
#Creating new DataFrame only for Apparent Temperature and Humidity
df_column = ['Apparent Temperature (C)', 'Humidity']
df_monthly_mean = df_target[df_column].resample("MS").mean() #MS-Month Starting
df_monthly_mean.head()
sns.set_style("whitegrid")
sns.FacetGrid(df_monthly_mean, height=8).map(plt.scatter, "Apparent Temperature (C)", "Humidity")
plt.show()
plt.figure(figsize=(14,6))
sns.lineplot(data = df_monthly_mean)
plt.show()
sns.set_style("whitegrid")
sns.FacetGrid(df_target, hue="Summary", height=8).map(plt.scatter, "Apparent Temperature (C)", "Humidity").add_legend()
plt.show()
# For Apparent Temperature (C)
sns.FacetGrid(df_target, hue="Summary", height=10).map(sns.distplot, "Apparent Temperature (C)").add_legend()
plt.show()
# For Humidity
sns.FacetGrid(df_target, hue="Summary", height=10).map(sns.distplot, "Humidity").add_legend()
plt.show()
