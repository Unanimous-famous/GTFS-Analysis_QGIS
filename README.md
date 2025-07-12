# GTFS-London_QGIS
A practice to use GTFS data for mapping visualisation and analysis. London GTFS data on TransitFeeds, which contains bus transit specifications, was downloaded and used in this project. 
OpenStreetMap was used as the base map (From the top bar, Select Web>QuickMapServices>OSM>OSM Stnadard).

First, the most basic method for using and depicting the stops and the routes by connecting the points of stops (coarse routes mapping). It was done by:
  
  1- Using the option "Add Layer by Delimited Text Layer (⌘+⇧+T):
    
  - Selecting the stops.txt by selecting … at the very top end of the dialoge box, in front of "File name:"
  
  2- Changed the simple marker to a raster image, downloaded one open source and replaced. The stops are shown as:

  <img width="1654" height="1096" alt="image" src="https://github.com/user-attachments/assets/f111a0ee-ec7f-4a15-9d41-bfef2103e81d" />

  
  3- Using the built-in function of 'Points to Path' from the Processing Toolbox to create the routes:
  - First, using the same option as before, add the layer from delimited txt, add stop_times.txt, But this time by selecting/ticking the option “No geometry (attribute table only)”
  - Join the columns of stop_times to stops to also make stops_sequence and trip_id available, as they are needed for using "Points to path" tool:
    - In Layers panel, right-click your stops layer → Properties
    - Go to the Joins tab
    - Click the ‘+’ (Add Join) button
    - Set these fields in the dialog:
    
          | Field        | Value        |
          | ------------ | ------------ |
          | Join layer   | `stop_times` |
          | Join field   | `stop_id`    |
          | Target field | `stop_id`    |
     - Click OK, then OK again to close properties. (You can check if the joining was succesful by opening the attribute table of the stops layer)
  
  *️⃣ This is basically joining the attribute tables and the columns in stop_times to stops layer, as we need stop_sequence and trip_id to depict the route, by right-clicking on the layer in      the layers pane.
  
  - From the top bar, select Processing>Toolbox (⌥+⌘+t), and then in the right pane that is opened, Processing Toolbox, search for "Points to path". In the dialogue box:
    - Set Input layer to your joined stops layer
    - For Group field, select: stop_times_trip_id
    - For Order field, select: stop_times_stop_sequence
    - If it is for practice only you don't need further development or the files, select "Create temporary layeer" for Paths and "Save to temporary directoy" for Directory for text output,        otherwise select appropriate file location and name (e.g., coarse_routes.gpkg).
    - Click Run
  
  The result should resemble:

<img width="1648" height="1044" alt="image" src="https://github.com/user-attachments/assets/80612e26-4fae-4206-a2ba-32dee5268098" />

<img width="2052" height="1256" alt="image" src="https://github.com/user-attachments/assets/f297253d-890d-4ded-acbf-e3694bf03f89" />

As you see, so far, the routes are all visaulised, but not differetiated. For that, the "Paths" (the outcome of the Points to path tool) layer's style should be modified:

  - Right-click the Paths layer → Properties
  - Go to the Symbology tab
  - In the top dropdown, change from “Single symbol” to “Categorized”
  - For Value, choose the field: stop_times_trip_id
  - Click Classify (bottom-left, below the white space)
  - QGIS assigns a random colour to each unique trip. It should resemble:

<img width="1112" height="645" alt="Screenshot 2025-07-12 at 11 20 41" src="https://github.com/user-attachments/assets/a974373a-28b9-4fb6-9557-6e06381ee531" />

To have a more accurate map (fine route mapping), the shapes from the shapes.txt should be used:

  - Add delimited text layer
  - Select shapes.txt
  - In the Geometry Defintion group, select 'Point coordinates' (not 'Well known text' or 'No geometry (only attribute table)') and set:
    - X field → shape_pt_lon
    - Y field → shape_pt_lat

Points, showing stops:
<img width="2174" height="1320" alt="image" src="https://github.com/user-attachments/assets/c74f8d7a-aa5b-4aa1-bd3a-375c6cc92313" />

  - Then again, the Points to path tool should be used, setting:
    - Input layer:	Your loaded shapes.txt layer
    - Group field:	shape_id
    - Order field:	shape_pt_sequence
    - Path:	Temporary layer or save to file
    - Output dir: Temporary dir 

<img width="2174" height="1320" alt="image" src="https://github.com/user-attachments/assets/ec473c34-700a-4fc6-a059-6be95e89824a" />

After changing the single color to categorised as before from the properties:

<img width="1087" height="660" alt="Screenshot 2025-07-12 at 13 17 03" src="https://github.com/user-attachments/assets/17dd4c71-13a0-47b6-b114-15f9e7c5375c" />


*️⃣ These could indeed be simply done by installing [GTFS GO](https://plugins.qgis.org/plugins/GTFS-GO-master/) plugin:

- Simply search it in the Plugin>Manage and install plugins
- After installation, the added icon  <img width="30" height="30" alt="image" src="https://github.com/user-attachments/assets/8954f8e8-5933-4999-a15d-742db0114549" />
- Click on it and in the opened dialogue box:
  - Set repository as: Preset
  - Set GTFS-Datasource: ---Load Local ZipFile--- (if you don't have any GTFS data downloaded, can try it with the built-in GTFS data, e.g. New York Subway)
                         Below it, using …, navigate to the zipfile with GTFS data
    
  - Set the output directory
  - Tick simple routes and stops
  - For ignore shapes.txt: If ticked, will give and estimatory route (Coarse Route Mapping) and if deselected, will output accurate mapping of the routes (Fine Route Mapping) from the           shapes.
  - In case frequency of use of each route is needed, the "aggregate route frequency" should be ticked.
  - Select "Extract on QGIS"

This will similarly create all the stops and routes.
    

