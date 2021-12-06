# Lab 1.2: Watson Studio Cognos Dashboard Embedded Visualizations
In this lab we will cover data visualization capability provided by **IBM Watson Studio**, the **IBM Watson Studio Dashboards** service, with a UI-driven capability to build and publish dashboards largely inspired by **IBM Cognos Analytics** capabilities.

## Watson Studio Cognos Dashboard Embedded
**IBM Watson Studio** has a built-in capability to build interactive, publishable dashboard.

## Setting up a dashboard
1. Back to your project's Assets list, select the `[(+) Add to project]` button ![](Lab2-Visualization/20190717_b56ce3a3.png)
1. Select the `Dashboard` button ![](Lab2-Visualization/20190717_c23e6094.png) to create a new dashboard
1. Enter a name, e.g. `NYC Bike Rentals`
1. We will need to create a dashboard service instance, select the `Associate a Cognos Dashboard Embedded service instance` link ![](Lab2-Visualization.assets/Lab2-Visualization-0a40b7f3.png)
1. Select the `[New Service +]` button ![](Lab2-Visualization.assets/Lab2-Visualization-1cb778a4.png)
1. Select the **IBM Cognos Dashboard Embedded** tile ![](Lab2-Visualization.assets/Lab2-Visualization-5afbe65d.png)
1. Choose the 'Lite' plan ![](Lab2-Visualization.assets/Lab2-Visualization-e02a4590.png)
1. Finally, associate the service with the project: ![](Lab2-Visualization.assets/Lab2-Visualization-baccbd62.png), and switch back to the *New dashboard* creation tab.
1. Click the `Reload` link and select the instance, then the `Save` button: ![](Lab2-Visualization/markdown-img-paste-2018051422030148.png)
1. In the `Select a template`, use `Tabbed` and `Freeform`: ![](Lab2-Visualization/markdown-img-paste-20180513233249777.png), then `[OK]` button.

## Adding data to a dashboard
We will now use the data produced by Data Refinery for the NYC bike share dataset.
1. Switch to the `Select` tab and expand `Selected sources` ![](Lab2-Visualization/markdown-img-paste-20180513233447821.png)
1. Select the `201701-citibike-tripdata_cleansed.csv` file: ![](Lab2-Visualization.assets/Lab2-Visualization-1e2d0842.png)
1. The dashboarding has the ability to propose a graph type based on the data. We will start by displaying the `Trip Duration` by `Age`:
    1. Drag&Drop the `Trip_Duration` from the data panel on the left to the dashboard canvas on the right; ![](Lab2-Visualization/markdown-img-paste-2018051323454084.png) The `Trip_Duration` total aggregated sum is displayed as a large number: ![](Lab2-Visualization.assets/Lab2-Visualization-59d5d773.png)
    1. Drop the `Age` field onto the `Trip_Duration` widget: ![](Lab2-Visualization/markdown-img-paste-20180513234943347.png)
    1. **IBM Watson Studio** changes the graph to a more suitable representation, in this case a line graph:![](Lab2-Visualization.assets/Lab2-Visualization-717c150d.png)
    1. You can switch the graph type to a histogram, by selecting the **Column** type of graph ![](Lab2-Visualization.assets/Lab2-Visualization-f597d6c4.png)
    1. The graph will change its rendering: ![](Lab2-Visualization.assets/Lab2-Visualization-78bfc7d7.png)
    1. Unfortunately, our data has not been cleansed enough and we have erroneous values for `Age`. Select the **Fields** button ![](Lab2-Visualization.assets/Lab2-Visualization-2580d768.png).
    1. Right-click on the `Age` label to display the menu, and select the filter menu ![](Lab2-Visualization.assets/Lab2-Visualization-fd31bb78.png)
    1. In the filter definition box, select all values which do not make sense, i.e. values above 100: ![](Lab2-Visualization.assets/Lab2-Visualization-ec920739.png)
    1. then click the `Invert` button and OK. We get a better-looking graph where we can see the trip duration distribution by age ![](Lab2-Visualization.assets/Lab2-Visualization-fd179440.png).
1. Now add a new Freeform tab ![](Lab2-Visualization.assets/Lab2-Visualization-537f201e.png) where we will create a map display of the stations by count of rentals:
    1. Select the two `Start_Station_Latitude` and `Start_Station_Longitude` fields and drop them on the canvas: ![](Lab2-Visualization/markdown-img-paste-20180514000255337.png)
    1. The system automatically creates a map display, that you may want to resize: ![](Lab2-Visualization.assets/Lab2-Visualization-8f36af4b.png)
    1. Unfortunately, there is some parasitic data with erroneous coordinates that show up in the middle of the ocean at coordinate **(0,0)** below the African continent (This virtual place is known as `Null Island`). From the **Fields** tab, select the Latitude's Filter menu ![](Lab2-Visualization.assets/Lab2-Visualization-806d8af4.png)
    1. In the filter definition, select the first `0` value, then `Invert` and OK button. The map will center itself on NYC: ![](Lab2-Visualization.assets/Lab2-Visualization-8ba9b77a.png)
    1. We will now decorate the map view with additional information. Drop the `Start_Station_Name` onto the `Label`: ![](Lab2-Visualization/markdown-img-paste-20180514001557213.png)
    1. We see on the map an outlier to the south, we can filter it out by name, as we can get the `SSP Tech Workshop` label now by hovering over it ![](Lab2-Visualization.assets/Lab2-Visualization-77340b04.png)
    1. Click the Filter button for `Label`/*Start Station Name*, and enter a `Does not begin with` condition for `SSP Tech`: ![](Lab2-Visualization.assets/Lab2-Visualization-ea62d439.png). The corresponding outlier points will disappear from the display.
    9. Add some coloring, by dropping the `Trip_Duration` field onto the `Point color`. The default aggregation is `SUM` which will show stations from where the cumulative trip are longer. This shows that a few stations are issuing longer rides than others, as they show in darker colors: ![](Lab2-Visualization.assets/Lab2-Visualization-f739e3fe.png)
    10. Change the aggregation used for the coloring, now based on the average trip duration. Select `Trip Duration`->`Summarize`->`Average`:![](Lab2-Visualization/markdown-img-paste-20180514003901391.png).
    11. All points now look similar, except for a few outliers, which match specific stations ![](Lab2-Visualization.assets/Lab2-Visualization-c7e2616c.png)
    12. You can remove outlier manually by right-click selecting them on the map and selecting `exclude`: ![](Lab2-Visualization.assets/Lab2-Visualization-ba3f8ad3.png)
1. Correlated graphs selections (Widget connections)
    1. Drop the `End_Station_Latitude/Longitude` on the free space besides the `Start_Station` map to create a new map.
       Now, when clicking on a Station in the first map, the second map adjusts to show the corresponding `End_Station`: ![](Lab2-Visualization.assets/Lab2-Visualization-00f2f62e.png)
    1. Note that the selection groups can be adjusted using the `Widget connections` icon at the top ![](Lab2-Visualization/20180925_f5ea33f5.png)
1. Many other types of graphs can be built, as an exercise, build:
    1. a graph on another tab that will show the distribution of rentals by the hour of the day. You should end up with a **Column** graph such as ![](Lab2-Visualization.assets/Lab2-Visualization-aee29e92.png). Note that the *Length* attribute, set to *Trip Duration*, should be summarized by *Count*.
    1. And last, build a graph that shows usage by day of month: ![](Lab2-Visualization.assets/Lab2-Visualization-1725d947.png). We see the weekly cycle, and probably the impact of weather conditions (There was a severe snowstorm in NY on Jan 7, 2017).
1. Finally, dashboards can be published:
    1. Click the `Share` icon: ![](Lab2-Visualization/markdown-img-paste-20180514011738472.png)
    2. Enable sharing: ![](Lab2-Visualization/markdown-img-paste-2018051401182447.png)
    3. Open the link from another tab or browser to get a web view on the dashboard
