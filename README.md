# World Weather and Travel Analysis

## Project Overview
I used weather data from around the world to plan a vaction.

***Resources***: 
 - Jupyter Notebook
 - Python 3.7.13
 - JSON
 - Pandas, CityPy, NumPy
 - Open Weather Map API, Google Maps API, Google Directions API

## Process
Using NumPy and CityPy, I was able to generate nearly 700 cities around the world based on random latitudes and longitudes.  Using the Open Weather Map API, I created a pandas DataFrame with the information:

![image](https://github.com/jakatz87/World_Weather_Analysis/blob/main/Weather_Database/City_Weather.png)

I then saved the DataFrame as a CSV file to be used for Vacation Planning.

The Vacation Plan was based on Minimum and Maximum temperatures from the cities I created.  For this project, I used a minimum temperature of 60 and a maximum of 85 degrees.  Once cleaned, I had 343 cities that met the criteria.

![image](https://github.com/jakatz87/World_Weather_Analysis/blob/main/Vacation_Search/Clean_cities.png)

To make sure I had places to stay, I used Google’s Maps API to get hotel information from each of the cities with the code:
```
# Iterate through the hotel DataFrame 
for index, row in hotel_df.iterrows():
    # Fetch latitude and longitude from the DataFrame
    lat = row["Lat"]
    lng = row["Lng"]
    
    # Add the latitude and longitude as parameters to the params dictionary
    params["location"] = f"{lat},{lng}"
    
    # Set up the base URL for the Google Directions API to get JSON data
    base_url = "https://maps.googleapis.com/maps/api/place/nearbysearch/json"

    # Make an API request and retrieve the JSON data from the hotel search
    hotels = requests.get(base_url, params=params).json()
    
    # Get the first hotel from the results and store the name, if a hotel isn't found skip the city
    try:
        hotel_df.loc[index, "Hotel Name"] = hotels["results"][0]["name"]
    except (IndexError):
        print("Hotel not found... skipping.")
```

Once that list was cleaned of missing hotels, I had a DataFrame with 321 cities and Hotels that I saved as a CSV file.

![image](https://github.com/jakatz87/World_Weather_Analysis/blob/main/Vacation_Search/city_hotels.png)

Using latitude, longitude, and Hotel Name, I created a Google Map with all the cities that fit my criteria.  Each city had a marker with the Hotel Name, City, Country, and Weather

![image](https://github.com/jakatz87/World_Weather_Analysis/blob/main/Vacation_Search/WeatherPy_vacation_map.png)

To create a vacation plan, I reviewed the map and looked for a destination that had 4 cities within driving distance of each other and decided to take a trip to Brazil.  I was planning to start and end the vacation in Augusto Correa, Brazil with stops in Ipixuna, Paragominas, and Cururupu.

I mapped out the routes and stops with Google’s Directions API, and used a new DataFrame to include hotel information on a new map.

![image](https://github.com/jakatz87/World_Weather_Analysis/blob/main/Vacation_Itinerary/vacation_dataframe.png)

##Results
It's time to pack my bags!
![image](https://github.com/jakatz87/World_Weather_Analysis/blob/main/Vacation_Itinerary/WeatherPy_travel_map.png)

![image](https://github.com/jakatz87/World_Weather_Analysis/blob/main/Vacation_Itinerary/WeatherPy_travel_map_markers.png)

