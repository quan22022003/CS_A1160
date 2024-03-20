import pandas as pd
import pycountry as pc

# Read the data
doctor_data = pd.read_csv('doctor.csv')
pharmacist_data = pd.read_csv('pharmacist.csv')

# Define a function to get the continent of a country
def get_continent(country):
    try:
        country_code = pc.country_name_to_country_alpha2(country)
        continent_code = pc.country_alpha2_to_continent_code(country_code)
        continent = pc.convert_continent_code_to_continent_name(continent_code)
        return continent
    except:
        return None

# Add continent information to the dataframes
doctor_data['Continent'] = doctor_data['Location'].apply(get_continent)
pharmacist_data['Continent'] = pharmacist_data['Location'].apply(get_continent)

# Merge the dataframes on continent and period
merged_data = doctor_data.merge(pharmacist_data, on=['Continent', 'Period'], how='inner')

# Display the merged data
print(merged_data)
