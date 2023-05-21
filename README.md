# Dynamic-Safe-Routing-for-Hazardous-Material-Transportation

## Introduction

In industrialized countries, many of the materials being transported pose a hazard to human health. These materials are known as hazardous materials (hazmat). A hazardous material can be any substance capable of causing harm to people, property, or the environment. They are typically characterized as being flammable, explosive, or corrosive. Any accidental release of hazardous materials during transportation often results in undesirable consequences. The four most frequently transported hazardous materials in Canada include crude petroleum, gasoline, fuel oils, and non-metallic minerals.

In 2018, there were 19581 reported incidents in the U.S., including explosions, fires, and poisonous gas leakages. These incidents resulted in 4 fatalities, 127 injuries, $80 million in property damage, and substantial effort in evacuating and restoring the affected areas.

Given these serious risks, it is vital to develop a dynamic routing solution for the transport of hazardous materials. This solution should aim to find the optimal route(s) for one or more vehicles to meet the customers' demands, while also minimizing travel time and reducing the risk associated with transportation.

The work showcased here will focus on the data preparation process for hazardous material transportation within the Greater Toronto Area (GTA) and will include case studies for two networks: the GTA Highway Network and the Major Roads Network in Toronto.

Following this, we propose a methodology for predicting the best route based on travel time and the associated transportation risk. 
***Please note that the specifics of this methodology are confidential, and will not be included in this document.***

## Case studies
### Problem characteristics

Our study focuses on the routing problem for a single truck delivering hazardous materials to one customer within the GTA or Toronto. The goal is to establish road network structures that can provide an optimal route for the truck. In our model, the arcs represent the roads in the GTA or Toronto, while the nodes denote intersections among streets. Each arc's travel time and associated risk can dynamically change according to the prevailing traffic, weather conditions, and population density. At any node, we can determine the travel time and risk for all outgoing arcs, enabling dynamic adjustments to the planned route. These adjustments are constrained by factors such as travel time, a risk threshold, and any other costs associated with the transportation.

To accurately assess the travel time and risk for each arc, we need data related to traffic (which helps in defining the travel time), weather, and population for the GTA or Toronto across both networks, which will then be used to ascertain the risk for each arc.

### Data Preparation for the Case Studies

The traffic data we use is provided by CVST, the Connected Vehicles and Smart Transportation research center from the ECE department of the University of Toronto. This data encompasses traffic information for 1829 road segments, which includes highways and main local roads used for transportation, in the GTA from July 2016 to October 2016.

***Please note that this traffic data is confidential, so we will not provide specific details regarding data preprocessing.***

The weather data is sourced from the hourly historical climate data provided by the Government of Canada. This data includes temperature, precipitation, relative humidity, wind speed, and visibility for each weather station for the period from July 2016 to October 2016. 

![image](https://github.com/Xiangzi-Chen/Dynamic-Safe-Routing-for-Hazardous-Material-Transportation/assets/90531358/ed91eb12-a84e-428d-ad35-c125435c9f09)

The figure above presents the structure of the weather data for August 25, 2016. Among all these features, we identified precipitation, wind speed, and visibility as the primary factors potentially affecting travel time and risk. The main weather stations with available hourly weather data in the GTA are Toronto Buttonville, Toronto International Airport, and Toronto City Center.

Population data is derived from the 2016 census profile by Statistics Canada. This data offers population density information for different neighbourhoods in the GTA. By integrating this population data onto the arcs within each neighbourhood, we can more accurately estimate the potential consequences of hazardous material incidents.

### Case study 1: Highway Network for GTA
#### Network Conduction

The first case study involves a routing problem using the GTA highway network, assuming that the driver only transports the material on the highway and exits the highway upon reaching their destination. The network comprises 14 nodes and 42 arcs, accommodating two directions between each pair of nodes. In this case, the arcs represent only highways. Traffic, being a key factor in our study, guided our use of the traffic data to establish the largest possible network.

![image](https://github.com/Xiangzi-Chen/Dynamic-Safe-Routing-for-Hazardous-Material-Transportation/assets/90531358/eadbae04-2add-429b-89bb-8598f7258d8e)

There are 14 nodes, each located at the highway’s main intersection. 

![image](https://github.com/Xiangzi-Chen/Dynamic-Safe-Routing-for-Hazardous-Material-Transportation/assets/90531358/a493fa85-c02b-4552-b3db-f15dd191447b)

The graph above provides a simplified visualization of the GTA highway network, with direct lines representing arcs from node to node. However, the actual arcs are a bit more complex, closely following the actual layout of the GTA's highways. The colors distinguish the two directions of the arcs between each pair of nodes. Please note that the numbers beside the arcs and nodes are merely labels and do not hold any practical significance.

We determined the actual arc length by measuring the point-to-point highway distance using Google Maps. Speed data was extracted from the aforementioned ECE traffic data. Weather and population data were sourced from open government resources. The arc length, speed, and weather data will be used to estimate travel time, while all the collected information will help in assessing the associated risk for this network.

**Please note that specific details regarding how traffic data aligns with each arc are not included in this document.**

With respect to weather data, we only have detailed data for three weather stations. Therefore, we assigned each arc to its nearest weather station by comparing the distance between the midpoint of each arc and the weather stations.

![image](https://github.com/Xiangzi-Chen/Dynamic-Safe-Routing-for-Hazardous-Material-Transportation/assets/90531358/fe66ae83-649f-4789-98a1-0c014f310693)

As shown above, the three points with different colours represent the locations of the weather stations. The green arcs are assigned to the weather station at Toronto International Airport, the red arcs to Toronto City Center, and the blue arcs to Buttonville. With the weather conditions determined for each arc, we can estimate the probability of accidents under various conditions.

![image](https://github.com/Xiangzi-Chen/Dynamic-Safe-Routing-for-Hazardous-Material-Transportation/assets/90531358/7a408e0d-eecf-4e3c-b249-8f1add4a2a94)

Moreover, as evidenced by the probability table presented earlier (not provided here), the accident probabilities for expressways vary depending on whether the pavements are dry or wet. Generally, the overall accident probability is higher for wet pavements. Restricted visibility also elevates the probability of accidents.

In addition, to estimate the potential consequences, we need to calculate the expected number of people within an impact zone of an arc. This requires assigning each arc to its corresponding population region. The graph below illustrates the regions surrounding the GTA highway network. These regions are differentiated by colours, and the points represent the nodes of our network. The arcs correspond to the red highways depicted on the map. Although the coloured regions make the arcs somewhat challenging to distinguish, it's evident that most arcs traverse multiple regions. Therefore, the arcs cannot simply be assigned to specific regions. Instead, we need to calculate the total area exposed to the arcs to estimate the potential population affected by each arc.

![image](https://github.com/Xiangzi-Chen/Dynamic-Safe-Routing-for-Hazardous-Material-Transportation/assets/90531358/5c02dd6e-4490-49c0-a03c-c0d21e9a53c0)

For instance, consider the yellow arc, which crosses five regions in total. We must measure the length of the arc segment within each region and calculate the total potential population exposure for the entire arc. Also, to determine the exposed population, we first need to establish the impact zone. This involves setting an impact radius, measuring the length of the arc piece in each region, and then calculating the area of the impact zone.

![image](https://github.com/Xiangzi-Chen/Dynamic-Safe-Routing-for-Hazardous-Material-Transportation/assets/90531358/1757ebae-ac9d-4c64-ade5-a22e0a08a0a3)

As shown in graph above, we determine the area of the impact zone by multiplying the radius by the length of the small segment of the arc. We then multiply this area by the population density of the respective region to ascertain the exposed population for this small arc segment in a specific region. By summing up the exposed population from each region, we can estimate the total expected population within an impact zone for each arc.

### Case study 2: Major Road Network in Toronto
The second case study focuses on the major road network in Toronto. We utilised the speed segments of major roads in Toronto to construct this network. The graph below, displayed on OpenStreetMap (not provided here), illustrates this network. It consists of 298 nodes and 1094 arcs, which represent both directions on each road.

![image](https://github.com/Xiangzi-Chen/Dynamic-Safe-Routing-for-Hazardous-Material-Transportation/assets/90531358/6e9b79f3-972b-466d-9e30-11bcb126e5eb)

The road network in Toronto is complicated, so we applied some strategies to simplify the progress of conducting the network.

![image](https://github.com/Xiangzi-Chen/Dynamic-Safe-Routing-for-Hazardous-Material-Transportation/assets/90531358/33310b76-dfcf-4904-81b7-4602e24ec861)

1. The network only includes dual carriageways, both for local roads and highways, that have traffic flowing in opposite directions.
2. Intersection points from speed segments of traffic data, where two or more roads meet or intersect, are consolidated. As we built the network using traffic data, one intersection might have four sensors recording the speed of passing vehicles.
3. Highway interchanges, which often have multiple entrances and exits at one location, are simplified into a single node in our network.
4. The network only takes into account express lanes for highways. Our previous assumption was that hazardous material transportation on the highway would exclusively use express lanes, so we considered only the speed data from these lanes.

For weather information of this network, weather station for each arc is assigned to the nearest weather station based on the distance between the midpoint of each arc and the weather stations.

![image](https://github.com/Xiangzi-Chen/Dynamic-Safe-Routing-for-Hazardous-Material-Transportation/assets/90531358/496df31f-2180-4a7e-b03d-af5e4529332e)

We also assigned the population region to get the impact zone for each arc and estimate the consequences. The population region for each arc is based on the region at which each arc’s midpoint is located.

![image](https://github.com/Xiangzi-Chen/Dynamic-Safe-Routing-for-Hazardous-Material-Transportation/assets/90531358/413fb56f-68a0-437a-b6b0-1c9b14a53c88)

Based on the above information, we have the preliminary information we need for this network. Since this is a larger network than the previous one, we are still working on improving the solution.

## Summary and discussion

In this project, we primarily focused on data preparation and two case studies for the safe routing problem concerning hazardous material transportation. We manually constructed two networks, the GTA Highway Network (with 14 nodes and 42 arcs) and the Major Road Network in Toronto (with 298 nodes and 1094 arcs), to test our solution method. For the GTA Highway Network, we employed an extension of the Safe SARSA method to minimize the Q-value for risk, ensuring that the chance constraint regarding travel time was not violated.

However, this method tends to retain all the Q-values for both risk and travel time at each state, requiring substantial memory space for storage, especially for larger networks (e.g., Major Road Network in Toronto). Additionally, the current solution method may yield non-optimal results. We are continually refining our solution method to make it applicable for other networks.
