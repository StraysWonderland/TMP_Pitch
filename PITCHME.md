# Travelling Misanthropist Problem
---
## Goal and Problem Definition
@ul
- we want to have a few beers
  - Search for bars nearby
- visit a few bars
  - Plan a roundtrip along k bars.
- @size[1em](**BUT**) 
- We don't like people. How to avoid people?
  - Visit only lowest ranking (and possibly least visited) bars.
@ulend
---
## Technology
**Parser**
- Java
- OSM4J for parsing pbf files.

**Web Application**
- Java
- Springboot for Web-Server
- Javascript as frontend
- Leaflet for map-visualisation
- Foursquare-API for retrieving bars and their ranking
---
## Parsing
- retrieves all edges that are suitable for pedestrians
- retrieves all corresponding nodes
- stores them two text files: de.edges and de.nodes
---
## POIs
- POIs retrieved via foursquare-REST-Api
- Retrieve based on following Properties:
  - Within a 2500m radius of selected ll
  - belongs to any of the following categories: bar, nightclub, stripclub, brewery, festival, weihnachtsmarkt
- Properties to retrieve:
  - Name
  - Address
  - various ratings
---
## Ranking of POIs
- Either select desirable bars yourself or:
- Sort bars based on a ranking function
  - function: alpha x hereNow + beta x (various-userRatings)
  - run TSP along k lowest ranking bars
  - k is defined by the user
---
## TSP: HeldKarp Algorithm
- Input: adjacency matrix of distances between all nodes
- Idea: Every subpath of a path of minimum distance is itself of minimum distance.
- Output: roundtrip allong all selected POIs
---
- Cities numbered from 1 to N
- Assume we start at city 1, distance between city i and city j is di,j. 
- Consider subsets S ∈ {2, ... , N} of cities and, for c ∈ S, let D(S, c) be the minimum distance, starting at city 1, visiting all cities in S and finishing at city c.
---
### Recursive formulation
- First phase: if S = {c}, then D(S, c) = d1,c. Otherwise: D(S, c) = min x∈S−c (D(S − c, x) + dx,c ).
- Second phase: the minimum distance for a complete tour of all cities is M = minc∈{2, ... ,N} (D({2, ... , N}, c) + dc,1 )
- A tour n1 , ... , nN is of minimum distance just when it satisfies M = D({2, ... , N}, nN ) + dnN,1 .
---
# Visualisation
---?image=assets/img/dijkstra.jpg
@snap[south span-100 text-06]
Visualisation of the shortest path between two markers
@snapend
---?image=assets/img/hereNow.jpg
@snap[south span-100 text-06]
All nearby bars visualised via red marker.
Popup shows bar info and how many people are here now.
@snapend
---?image=assets/img/roundtrip.jpg
@snap[south span-100 text-06]
Generated roundtrip between a number of selected bars
@snapend
---
@title[Problems]
### Current Issues
- Foursquare not widely used
- Google Places billing
- Performance of HeldKarp
