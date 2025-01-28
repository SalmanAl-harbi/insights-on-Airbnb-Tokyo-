# insights-on-Airbnb-Tokyo-
python analysis using calendar and listing dataset of Tokyo 
## For the following analysis I have download the data from https://data.insideairbnb.com/united-kingdom/england/greater-Tokyo/2024-09-23/data/calendar.csv.gz
``` diff
import pandas as pd
calendar=pd.read_csv("calendar.csv")
```
## I want to know the number of available and unavailable rooms
``` diff
calendar.available.value_counts()
```
<img width="283" alt="image" src="https://github.com/user-attachments/assets/14e5e909-d401-4620-8d19-0e784c051b54" />

f(false) means not available, t(true) means available 

## 2 purpose: calculates the percentage of available (t) and unavailable (f) dates.
Explanation value_counts(normalize=True) give proportion. Multiplaying by 100 converts the proportion into percentage


``` diff
availability_percentage = calendar['available'].value_counts(normalize=True) * 100
availability_percentage
```
<img width="400" alt="image" src="https://github.com/user-attachments/assets/39fcf98c-cdb3-4a40-a859-b906631c9be0" />

## 3 Let's Count the busiest day!
Hint We will be counting the most unavailable days (given by f)
``` diff
busiest_dates = calendar[calendar['available'] == 'f']['date'].value_counts()
print("Busiest Dates:")
print(busiest_dates.head())
```
<img width="400" alt="image" src="https://github.com/user-attachments/assets/535ec314-d951-4a00-82d5-e52088b4410d" />

## 4 Plot a bar graph to show availability percentage
``` diff
import matplotlib.pyplot as plt
availability_percentage.plot(kind='bar', color=['green', 'red'])
plt.title('Availability Percentages')
plt.ylabel('Percentage')
plt.xlabel('Available (t/f)')
plt.show()
```
<img width="679" alt="image" src="https://github.com/user-attachments/assets/aa574f48-8eb4-4a73-a958-a62891b202be" />

## 5 Plot the busiest day
``` diff
busiest_dates.head(10).plot(kind='bar', color='orange')
plt.title('Top 10 Busiest Dates')
plt.ylabel('Number of Listings Unavailable')
plt.xlabel('Date')
plt.show()
```
<img width="679" alt="image" src="https://github.com/user-attachments/assets/3a765dc4-3136-4498-8948-f3ddbc20cd6e" />

## 📄 Download listings dataset of Tokyo from
https://data.insideairbnb.com/united-kingdom/england/greater-Tokyo/2024-09-23/visualisations/listings.csv

``` diff
import pandas as pd
listings = pd.read_csv('listings.csv')
```
<img width="985" alt="image" src="https://github.com/user-attachments/assets/460f4e0a-915b-4386-ae5c-61f74d8d0f98" />

``` diff
listings.columns
```
<img width="772" alt="image" src="https://github.com/user-attachments/assets/ccac05f5-c72b-4d68-837b-05c2edd2936d" />

## ✅ Room type and price is given seperately
Let`s Combine and visualize them
``` diff
import matplotlib.pyplot as plt
price_by_room = listings.groupby('room_type')['price'].mean()
print(price_by_room)

# Plot price by room type
price_by_room.plot(kind='bar', color='cyan')
plt.title('Average Price by Room Type')
plt.ylabel('Average Price')
plt.xlabel('Room Type')

plt.show()
```
<img width="772" alt="image" src="https://github.com/user-attachments/assets/c4c7c9c2-5b82-4b84-9bc7-bb42223e5d62" />

## ✅ Which are the top 10 neighborhoods with the most listings?
``` diff
neighborhood_counts = listings['neighbourhood'].value_counts().head(10)
print("Top 10 Neighborhoods by Listings:")
print(neighborhood_counts)

# Plot neighborhoods with most listings
neighborhood_counts.plot(kind='bar', color='lightcoral')
plt.title('Top 10 Neighborhoods by Listings')
plt.ylabel('Number of Listings')
plt.xlabel('Neighborhood')
plt.xticks(rotation=90)
plt.show()
```
<img width="772" alt="image" src="https://github.com/user-attachments/assets/58a637fb-66e0-4c64-9539-c93fae3af489" />

## ✅ What is the distribution of the room types (Private room, Entire home/apt) in the listings data?
```diff
room_type_counts = listings['room_type'].value_counts()
labels = room_type_counts.index
sizes = room_type_counts.values

plt.figure(figsize=(8, 6))
plt.pie(sizes, labels=labels, autopct='%1.1f%%', startangle=30)
plt.axis('equal')
plt.title('Distribution of Room Types')
plt.show()
```
<img width="829" alt="image" src="https://github.com/user-attachments/assets/062067be-6ab9-4a9d-821a-22bea7df4512" />

## ✅ How does the price vary across different neighborhoods?
```diff
neighborhood_prices = listings.groupby('neighbourhood')['price'].mean().sort_values().head(15)

plt.figure(figsize=(12, 8))
neighborhood_prices.plot(kind='bar', color='skyblue')
plt.xlabel('Neighborhoods')
plt.ylabel('Average Price')
plt.title('Average Price by Neighborhood')
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```
<img width="989" alt="image" src="https://github.com/user-attachments/assets/01fb033b-df65-4520-8e76-1f3274bf49f3" />

## ✅ Geographical Distribution of Listings (Price Colored)
``` diff
import matplotlib.pyplot as plt
import seaborn as sns
```
```diff
plt.figure(figsize=(10, 6))
sns.scatterplot(data=listings, x='longitude', y='latitude', hue='price', palette='viridis', size='price', sizes=(10, 200))
plt.title('Geographical Distribution of Listings (Price Colored)')
plt.xlabel('Longitude')
plt.ylabel('Latitude')
plt.show()
```
<img width="989" alt="image" src="https://github.com/user-attachments/assets/5451c8a5-e1f3-4efa-82e6-386c0feb29dd" />

## Let us see the listings on a real map
Hotter Areas (Red/Yellow): High Density: The areas that appear in red or yellow (the "hot" colors) indicate higher density or concentration of listings. This means there are more listings in these areas. Popular Locations: These regions might be more popular or in high demand. It could be near tourist attractions, popular neighborhoods, or central areas in Tokyo where people tend to stay more often. Colder Areas (Green/Blue):

Low Density: Areas with blue or green (the "cold" colors) indicate a lower concentration of listings. These regions have fewer listings available. Less Popular Locations: These areas might be less popular or further from key attractions. If you're looking at pricing or other factors, lower density could imply less competition in these regions, which might indicate more affordable areas or less tourist traffic. Density Patterns:

Clustered Areas: If you notice clusters of heatmap intensity, they represent hotspots. These might correspond to high-traffic areas like resorts, beaches, or urban centers. Spread-Out Listings: If the heatmap shows a more uniform distribution, it could suggest that listings are more evenly spread across the region, which may reflect a more balanced demand for rentals across different areas of Tokyo.
``` diff
import folium
from folium.plugins import HeatMap
import pandas as pd


Tokyo_data = listings[['latitude', 'longitude', 'price']]  # Example, you may add more columns

# Create a base map centered around Tokyo
m = folium.Map(location=[35.675475631089924, 139.77986227390716], zoom_start=10)

# Prepare the data for the heatmap
heat_data = [[row['latitude'], row['longitude']] for index, row in Tokyo_data.iterrows()]

# Add the heatmap to the map
HeatMap(heat_data).add_to(m)

# Save the map as an HTML file to view in a browser
m.save('Tokyo_heatmap.html')

<img width="961" alt="image" src="https://github.com/user-attachments/assets/06aaece8-0398-4155-a82b-a76d1dc5eb2d" />


# If you're using Jupyter Notebook, you can display the map directly in the notebook:
m
```
<img width="949" alt="image" src="https://github.com/user-attachments/assets/f84b6052-89ad-4fde-9e12-99d7b24e6bdd" />
