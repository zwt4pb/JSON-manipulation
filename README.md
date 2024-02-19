import requests
import pandas as pd

json_url = "https://raw.githubusercontent.com/jasonwnc/ds2002s24/main/2.%20Python/coll%20bb_SortByConf.json"
response = requests.get(json_url)
data = response.json()

dataframe = pd.json_normalize(data, 'teams', errors='ignore',)
print(dataframe.head(100))

# a. How many teams are there? 
number_of_teams = dataframe['tid'].nunique()
print("There are", number_of_teams, "unique teams.")
# b. How many teams are in the state of Virginia?
number_va_teams = len(dataframe[dataframe["state"] == "VA"])
print("There are", number_va_teams, "teams in the state of Virginia.")
# c. Give me a list of Duplicate Mascots and group them by number....IE. Don't include groups of 1.
duplicatemascots = dataframe[dataframe.duplicated(subset=['name'], keep=False)]['name']
duplicatemascot_counts = duplicatemascots.value_counts()
duplicatemascot_groups = duplicatemascot_counts[duplicatemascot_counts > 1]

print("List of Duplicate Mascots grouped by number:")
print(duplicatemascot_groups)
