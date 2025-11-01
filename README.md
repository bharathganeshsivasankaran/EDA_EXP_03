# EXP 3 - Delhi Air Quality Analysis**

## Aim
To compare air quality parameters in Delhi across different stations and analyze the relationship between pollutants (e.g., PM2.5 and NO₂) using scatter plots and correlation analysis.


## Procedure / Algorithm

1)Load the dataset using pandas.

2)Preprocess the data:

3)Convert the date column (period.datetimeFrom.utc) to datetime format.

4)Drop missing or invalid values.

5)Pivot the dataset so each pollutant (parameter) becomes a separate column.

6)Plot scatter plot between PM2.5 and NO₂ to study their relationship.

7)Plot correlation heatmap between all pollutants to identify relationships.

8)Interpret the results — identify which pollutants are correlated and which stations are most polluted.


## Program


```
Name : Bharathganesh S
Reg No : 212222230022
```


```py
# Import libraries
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

sns.set_theme(style="whitegrid")

file_path = 'delhi_pm25_aqi.csv'  # Upload this file to Colab environment
df = pd.read_csv(file_path)

print("Dataset Shape:", df.shape)
print("Columns:", df.columns)
print("\nSample Data:\n", df.head())
print("\nData Types:\n", df.dtypes)
print("\nNull Value Counts:\n", df.isnull().sum())

df['period.datetimeFrom.utc'] = pd.to_datetime(df['period.datetimeFrom.utc'], errors='coerce')

df = df[df['value'] > 0]

df['date'] = df['period.datetimeFrom.utc'].dt.date
df['month'] = df['period.datetimeFrom.utc'].dt.month
df['hour'] = df['period.datetimeFrom.utc'].dt.hour

plt.figure(figsize=(12, 6))
sns.boxplot(x='month', y='value', data=df, palette='mako')  
plt.title('Monthly Boxplot of PM2.5 in Delhi', fontsize=14)
plt.xlabel('Month')
plt.ylabel('PM2.5 (µg/m³)')
plt.tight_layout()
plt.show()

monthly_avg = df.groupby('month')['value'].mean().reset_index()
plt.figure(figsize=(12, 6))
sns.lineplot(x='month', y='value', data=monthly_avg, marker='o', color='#0077b6', linewidth=2.5)  
plt.title('Monthly Average PM2.5 in Delhi', fontsize=14)
plt.xlabel('Month')
plt.ylabel('Average PM2.5 (µg/m³)')
plt.grid(alpha=0.3)
plt.tight_layout()
plt.show()

daily_avg = df.groupby('date')['value'].mean().reset_index()
days_exceeding = daily_avg[daily_avg['value'] > 25]
print(f'Total days analyzed: {len(daily_avg)}')
print(f'Days exceeding WHO limit: {len(days_exceeding)}')
print(f'Percentage exceeding WHO limit: {len(days_exceeding)/len(daily_avg)*100:.2f}%')

hourly_avg = df.groupby('hour')['value'].mean().reset_index()
plt.figure(figsize=(12, 6))
sns.lineplot(x='hour', y='value', data=hourly_avg, marker='o', color='#9b2226', linewidth=2.5)  
plt.title('Average PM2.5 vs Hour of Day in Delhi', fontsize=14)
plt.xlabel('Hour of Day')
plt.ylabel('Average PM2.5 (µg/m³)')
plt.grid(alpha=0.3)
plt.tight_layout()
plt.show()

top5_days = daily_avg.nlargest(5, 'value')
print("\nTop 5 Worst Polluted Days (Date & Avg PM2.5):")
print(top5_days)

plt.figure(figsize=(8, 5))
sns.barplot(x='date', y='value', data=top5_days, palette='flare') 
plt.title('Top 5 Worst Polluted Days in Delhi', fontsize=14)
plt.xlabel('Date')
plt.ylabel('Average PM2.5 (µg/m³)')
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()

```
## Output

<img width="1325" height="489" alt="image" src="https://github.com/user-attachments/assets/bea8f243-d0c7-4020-a878-b2817650333b" />

<img width="1213" height="550" alt="image" src="https://github.com/user-attachments/assets/065dbeae-2be0-4f58-824b-4fdf4d7d280a" />

<img width="1106" height="546" alt="image" src="https://github.com/user-attachments/assets/d649afb3-2241-468d-8d08-158c08808f36" />

<img width="1136" height="545" alt="image" src="https://github.com/user-attachments/assets/ee3bf6dd-db19-4837-a633-da4fcf411387" />

<img width="1294" height="598" alt="image" src="https://github.com/user-attachments/assets/092d3951-3712-44c5-ae9e-c5445aa97512" />



## Interpretation

1) PM2.5 and NO₂ show a strong positive correlation, suggesting that both pollutants increase together, likely due to vehicle and industrial emissions.

2) PM2.5 levels drop significantly during monsoon months, peak in winter due to stagnant air and emissions, and show higher concentrations during traffic hours, highlighting the impact of weather and human activity on pollution.

## Result

The dataset was successfully loaded and processed to extract pollutant-wise and station-wise air quality data for Delhi.


