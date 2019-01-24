# Introduction/Business problem

A Greek company is interested in investing in a Greek restaurant somewhere in London. They do not care about where in London exactly. They care only in having good returns from their investment. In order to do that, they ask from us to find the best place in London in order to build the proposed type of restaurant.

Our purpose is to find the optimal place in London to build a Greek restaurant.

London is a big city with increasingly food industry revenues and we have to persuade them that our proposal worth the trouble.

In order to fulfill our task we are going to use London Postcodes, divided in 8 area categories:

South East, South West, West, North, East Central, East, West Central, North West.

We are going to choose the area of our restaurant based on the smallest number of food/restaurant venues.

After we choose the London Area, we are going to explore the neighborhoods in order to have a view of the venue categories and determine the group or groups of neighborhoods we should build the restaurant.

We are going to choose the final neighborhood based on several criteria as described below.


# Data-Section
We are going to find all the postcodes of London through eight sources/pages from wikipedia. These sources will be used for text mining and creation of the original dataframe of London. In the dataframe, we will include the variables of PostCode(e.g. N1C),Name of the Neighborhood(e.g. Kings Cross Central), Area(e.g. Camden) & Area Category(e.g. North). In "area category" we are going to flag the 8 areas of London.

We are going to proceed to the necessary data cleansing(e.g. non geolocation postcodes) and add for each neighborhood the latitude and longitude information from geocoder.

We are going to use Foursquare location data to extract the venues information for the areas of interest.We are going to create dummies for these categories in order to determine the best London area category based on the smallest frequencies of food restaurants.

Based on these Foursquare location data we are going to group the neighborhoods in 5 clusters.Based on cluster's characteristics we are going to choose the neighborhood group that satisfies criteria of building a Greek restaurant such as type of other venues nearby(e.g. theaters,cinemas,parks etc.), if there are other restaurants that could be damaging for our business(e.g. a lot of ethnic restaurants) etc.

Finaly, during several steps of the project, we are going to use folium library to create the necessary maps in order to give a visual view to the investors of our actions & logic.



# Methodology
After we gathered data and built the appropriate dataframe from our sources, we proceeded in data analysis to find the best area & neighborhood to build our restaurant. We took into consideration the top 20 most common venues from Foursquare, through frequency statistics & aggregations per area and we derived the distributions per area & per venue category. At that point we had to choose the best area of London and we selected the area with the least percentage of restaurants and other types of food industry. 
For our next step and for the selected area, we took into consideration the top 10 most common venues. Through frequency statistics & aggregations per neighborhood, we proceeded in K-means clustering method in order to group our neighborhoods into 5 clusters based on the venues nearby. Geospatial map view is given for our clustering results. We explored the derived clusters and we chose the cluster that there is a probable opportunity to build a greek restaurant. From the selected cluster we chose the optimal neighborhood based on the least percentage of restaurants and other types of food industry and other criteria. Our final decision was taken based on the geospatial factor considering the distance from the centre of london and the existance of other entertainment venues nearby(e.g. theatres) that macthup with a good lunch or dinner.


# Results
In order to decide which area of London is the best, we took into consideration the top 20 most common venues from Foursquare.We selected the area with the least percentage of restaurants and other types of food industry.The selected area was East London.
For East London, we took into consideration the top 10 most common venues for each neighborhood and we grouped our neighborhoods into 5 clusters based on the venues nearby. We explored the derived clusters and chose the 2nd cluster. From the selected cluster, we endedup in the final neighborhood which correspond to Postcode of E1W. Finally, we chose E1W neighborhood based on criteria such as distance from central London(our approach is that there is easier access for a larger number of London citizens). Overall, our criteria to our final selection were to avoid competition as much as possible, distance from London down-town(easy access of large numbers of citizens), existance of other entertainment venues nearby(e.g. parks) that macthup with a good lunch or dinner.


# Discussion
As a first observation, we would say that there are a lot of pizza & italian restaurants almost everywhere in London. So, if you are a tourist and your appetite demands italian or pizza don't worry. My guess is that you won't even need google maps to find one.
A second observation is that we faced a lot of problems in data cleansing due to the fact that there are special non geolocation postcodes that correspond to facilities like banks etc.So, in case you want to work with London's Postcodes in the future becareful in data cleansing.
A third observation is that our final decision took into consideration several factors such as number of food restaurants nearby,distance from central London and if there are other venues nearby such as theatres etc. So, a part of our project is based on analytics appliance in geospatial & pure business analysis and logic.
Our final choice is the optimal due to the fact that there are only two food venues nearby.Furthermore, it is close to central London as you will see in the maps and there are other venues that macthup with a good lunch or dinner such as a park,a pub,a coffee shop etc.


# Conclusion
We considered several factors and we managed to extract the optimal result from a chaotic original view of London. At the beginning, we had 178 Postcodes which corresponded to London neighborhoods. Using analytics and business logic, we narrowed our search at East London (20 neighborhoods). We grouped our selected neighborhoods into 5 groups and we selected the best cluster with a probable opportunity. We chose the final neighborhood with the best likelihood to be productive for a greek restaurant.Our criteria for our decisions during all the analysis were to avoid competition as much as possible, to build the restaurant as closest as possible to central London and to built it near to other entertainment venues.Neighborhood with postcode E1W is the optimum proposal since there are only an Italian and an Indian restaurant from food insutry nearby. Additionally, it is really close to central London and nearby you will find a park,a pub,a coffee shop etc. 

Link
https://rawgit.com/zerocool2545/Capstone-Battle-of-Neighborhoods/master/London%20%282%29.html?fbclid=IwAR1vELtWz_uRIbvKHHd4PSA0QkJRSOYMo4WGoCL44Frzg5wPdC36IYoFU8c
