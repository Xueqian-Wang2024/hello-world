---
jupyter: python3
---

```{python}
import pandas as pd

# Load dataset
data = pd.read_csv('unicef_metadata.csv')

# Display the first few rows of the dataframe and its summary
data.head(), data.info()
```

```{python}
data_2020 = data[data['year'] == 2020]
data_2020.head()
```

```{python}
additional_data = pd.DataFrame({
    'alpha_3_code': ['AFG', 'ALB', 'DZA', 'ASM', 'AND'],
    'Healthcare Expenditure (% of GDP)': [9.5, 6.8, 5.2, 7.1, 10.2]
})

additional_data.head()
```

```{python}
merged_data = pd.merge(data_2020, additional_data, on='alpha_3_code', how='left')

merged_data.head()
```

```{python}
import matplotlib.pyplot as plt

# Scatter Plot of GDP per capita vs Healthcare Expenditure
plt.figure(figsize=(10, 6))
plt.scatter(merged_data['GDP per capita (constant 2015 US$)'], merged_data['Healthcare Expenditure (% of GDP)'])
plt.title('GDP per Capita vs. Healthcare Expenditure (% of GDP) in 2020')
plt.xlabel('GDP per Capita (constant 2015 US$)')
plt.ylabel('Healthcare Expenditure (% of GDP)')
plt.grid(True)
plt.show()
```

```{python}
#| scrolled: true
# Filter data for select countries and pivot for plotting
selected_countries = ['United States', 'China', 'India', 'Brazil']
life_expectancy_trends = data[data['country'].isin(selected_countries)]
life_expectancy_trends = life_expectancy_trends.pivot(index='year', columns='country', values='Life expectancy at birth, total (years)')

# Line Graph of Life Expectancy over the years
plt.figure(figsize=(12, 7))
for country in selected_countries:
    plt.plot(life_expectancy_trends.index, life_expectancy_trends[country], label=country)
plt.title('Life Expectancy Trends')
plt.xlabel('Year')
plt.ylabel('Life Expectancy at Birth (years)')
plt.legend(title='Country')
plt.grid(True)
plt.show()
```

```{python}
# Total number of countries
total_countries = len(countries)

# Number of charts
n_charts = 4

# Number of countries per chart
countries_per_chart = np.ceil(total_countries / n_charts).astype(int)

# Create the bar charts
fig, axs = plt.subplots(n_charts, 1, figsize=(12, 160))  

for i in range(n_charts):
    start_idx = i * countries_per_chart
    end_idx = start_idx + countries_per_chart
    subset_countries = countries[start_idx:end_idx]
    subset_expenditures = military_expenditures[start_idx:end_idx]
    
    axs[i].bar(subset_countries, subset_expenditures, color='blue')
    axs[i].set_title(f'Military Expenditure by Country in 2020 (Chart {i+1})')
    axs[i].set_xlabel('Country')
    axs[i].set_ylabel('Military Expenditure (% of GDP)')
    axs[i].set_xticks(np.arange(len(subset_countries))) 
    axs[i].set_xticklabels(subset_countries, rotation=90)
    axs[i].grid(axis='y')

plt.tight_layout()
plt.show()
```




