#DoctorandPharmacistlinechart
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import pycountry_convert as pc
medDoc = pd.read_csv('doctors.csv')
pharm = pd.read_csv('pharmarcist.csv')
lifeExp = pd.read_csv('expectancy.csv')
medProf = medDoc.merge(pharm, on=['Location', 'Period'], how='inner')
medProf['Total Density'] = medProf['First Tooltip_x'] + medProf['First Tooltip_y']

# Drop unnecessary columns
columns_to_drop = ['Indicator_x', 'Indicator_y']
medProf = medProf.drop(columns=columns_to_drop)

# Rename the columns
medProf = medProf.rename(columns={'First Tooltip_x': 'Pharmacists Density','First Tooltip_y': 'Doctors Density' })
medProf_lifeExp = medProf.merge(lifeExp, on=['Location', 'Period'], how='inner')
columns_to_drop = ['Indicator']
medProf_lifeExp = medProf_lifeExp.drop(columns=columns_to_drop)

# Rename the columns
medProf_lifeExp = medProf_lifeExp.rename(columns={'Dim1': 'Sex','First Tooltip': 'Years' })
def get_continent(country):
    try:
        country_code = pc.country_name_to_country_alpha2(country)
        continent_code = pc.country_alpha2_to_continent_code(country_code)
        continent = pc.convert_continent_code_to_continent_name(continent_code)
        return continent
    except:
        return None

medProf_lifeExp['Continent'] = medProf_lifeExp['Location'].apply(get_continent)
continent_data = medProf_lifeExp.groupby(['Continent', 'Period']).agg({
    'Total Density': 'mean'
}).reset_index()

# Plotting
fig, ax = plt.subplots(figsize=(10, 6))

# Iterate over continents
for continent, data in continent_data.groupby('Continent'):
    ax.plot(data['Period'], data['Total Density'], label=continent)

# Adding labels and title
ax.set_xlabel('Year')
ax.set_ylabel('Total Density')
ax.set_title('Total Density of Doctors and Pharmacists by Continent')
ax.legend()

# Show plot
plt.tight_layout()
plt.show()



#Linechartlifeexpectancy
lifeExp['Continent'] = lifeExp['Location'].apply(get_continent)
continent_lifeExp = lifeExp.groupby(['Continent', 'Period']).agg({
    'First Tooltip': 'mean'
}).reset_index()

# Plotting
fig, ax = plt.subplots(figsize=(10, 6))

# Iterate over continents
for continent, data in continent_lifeExp.groupby('Continent'):
    ax.plot(data['Period'], data['First Tooltip'], label=continent)

# Adding labels and title
ax.set_xlabel('Year')
ax.set_ylabel('Life Expectancy')
ax.set_title('Life Expectancy by Continent')
ax.legend()
ax.set_xticks(continent_lifeExp['Period'].unique())
# Show plot
plt.tight_layout()
plt.show()
#ScatterplotDoctorandPharmacist
medProf['Continent'] = medProf['Location'].apply(get_continent)
continent_medProf = medProf.groupby(['Continent', 'Period']).agg({
    'Total Density': 'mean'
}).reset_index()

# Plotting scatterplot
fig, ax = plt.subplots(figsize=(12, 9))

# Iterate over continents
for continent, data in continent_medProf.groupby('Continent'):
    ax.scatter(data['Period'], data['Total Density'], label=continent)

# Adding labels and title
ax.set_xlabel('Year')
ax.set_ylabel('Total Density')
ax.set_title('Total Density of Doctors and Pharmacists by Continent')
ax.legend()

# Set x-ticks to match the years in the data
ax.set_xticks(continent_medProf['Period'].unique())

# Show plot
plt.tight_layout()
plt.show()

#Scatterplotlifeexpectancy
# Aggregate data by continent
lifeExp['Continent'] = lifeExp['Location'].apply(get_continent)
continent_lifeExp = lifeExp.groupby(['Continent', 'Period']).agg({
    'First Tooltip': 'mean'
}).reset_index()

# Plotting scatterplot
fig, ax = plt.subplots(figsize=(10, 6))

# Iterate over continents
for continent, data in continent_lifeExp.groupby('Continent'):
    ax.scatter(data['Period'], data['First Tooltip'], label=continent)

# Adding labels and title
ax.set_xlabel('Year')
ax.set_ylabel('Life Expectancy')
ax.set_title('Life Expectancy by Continent')
ax.legend()

# Set x-ticks to match the years in the data
ax.set_xticks(continent_lifeExp['Period'].unique())

# Show plot
plt.tight_layout()
plt.show()

#heatmapDoctorandPharmacist
# Aggregate data by continent
medProf['Continent'] = medProf['Location'].apply(get_continent)
continent_medProf_filtered = medProf[medProf['Period'].isin([2000, 2010, 2016])]
continent_medProf_aggregated = continent_medProf_filtered.groupby(['Continent', 'Period']).agg({
    'Total Density': 'mean'
}).reset_index()

# Pivot the data to create a matrix for the heatmap
heatmap_data = continent_medProf_aggregated.pivot(index='Continent', columns='Period', values='Total Density')


# Plotting heatmap
plt.figure(figsize=(10, 6))
sns.heatmap(heatmap_data, cmap='YlGnBu', annot=True, fmt=".1f", linewidths=.5)
plt.title('Total Density of Doctors and Pharmacists by Continent and Year')
plt.xlabel('Year')
plt.ylabel('Continent')
plt.tight_layout()
plt.show()

#heatmapexpectancy
# Aggregate data by continent
lifeExp['Continent'] = lifeExp['Location'].apply(get_continent)
continent_lifeExp = lifeExp.groupby(['Continent', 'Period']).agg({
    'First Tooltip': 'mean'
}).reset_index()

# Pivot the data to create a matrix for the heatmap
heatmap_data = continent_lifeExp.pivot(index='Continent', columns='Period', values='First Tooltip')

# Plotting heatmap
plt.figure(figsize=(10, 6))
sns.heatmap(heatmap_data, cmap='YlGnBu', annot=True, fmt=".1f", linewidths=.5)
plt.title('Life Expectancy by Continent and Year')
plt.xlabel('Year')
plt.ylabel('Continent')
plt.tight_layout()
plt.show()

#RegressionDoctorPharmacist
model = ols('Q("Total Density") ~ Period', data=continent_medProf).fit()

# Print regression summary
print(model.summary())

#Regressionexpectancy
model = ols('Q("First Tooltip") ~ Period', data=continent_lifeExp).fit()

# Print regression summary
print(model.summary())
