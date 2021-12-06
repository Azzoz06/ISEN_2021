# Streaming Time Series with SPSS Flow Modeler Hands-on Lab
(C) IBM 2021 - Hybrid Cloud Build Team Europe

Author: philippe.gregoire@fr.ibm.com

# Introduction
## Lab objectives
This lab will show how to use SPSS Flow Modeler within CloudPak for Data on IBM Cloud to implement a timesseries prediction model.

The input dataset represents historical Market volumes over 60 months for a fictitious Broadband carrier on several markets. The goal of this Lab is to implement a prediction of the future market volumes.

## Reference Material:
* https://www.ibm.com/support/knowledgecenter/en/SS3RA7_sub/modeler_mainhelp_client_ddita/clementine/timeser_as_node_general.html

* https://www.ibm.com/support/knowledgecenter/en/SS3RA7_16.0.0/com.ibm.spss.modeler.help/clementine/streamingts_deploymenttab.htm#streamingts_deploymenttab

# Lab sequence instructions
## Add input file as asset
Upload `broadband.csv` as file Data Asset.

## Create SPSS Flow with Modeler
### Setup flow with asset as input
1. Create new Modeler Flow: ![](Lab5_StreamingTimeSeries.assets/20190214_f51e8d61.png)
1. Give it a name, e.g. *SPSS Streaming TS* ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-409cd6aa.png)
3. Once the flow canvas is displayed, from the `Import` palette drawer, add a `Data Asset` node: ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-490ded40.png)
4. Open the node from its menu: ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-7d487c11.png)
5. Set the Data Asset to the `broadband.csv` file ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-d607e298.png)
6. From the `Outputs` palette, drop a `Data Audit` node and wire it to the Data Asset node: ![](Lab5_StreamingTimeSeries.assets/20190214_119a1615.png)
7. Run the flow using the `Play` icon: ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-673cae07.png)
8. Switch to the View output tab on the right panel: ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-232caa76.png)
9. Open the Data Audit ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-6572192b.png)
10. We get a summary of the data in the set, basically `Market_1` to `Market_85` columns with 60 rows each, a `DATE_` field

### Creating a chart
1. A recent addition to SPSS Flow Modeler is the chart builder, implementing SPSS Visualization. You will recognize the graphical component also used in Data Asset exploration and Data Refinery.
1. From the *Graphs* palette drawer, select the *Charts* node and drop it on the canvas ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-0b348b63.png)
1. Wire the Data Asset and Charts nodes together ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-be9162b1.png)
1. Select the Charts node Open menu ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-02275742.png)
1. Launch the Chart Builder ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-482ccf28.png)
1. Select a chart type of Multi-series ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-89fe4f84.png)
1. Select `DATE_` as the X-axis: ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-2932097d.png)
1. Add several axis as Y-axis ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-14a05a14.png), select **line** type.
1. You can optionally select *Separate Y axes* to compare the slopes of the different markets within their own range ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-061d27a8.png)
1. You could similarly chose to Normalize the data ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-3eb98d66.png)
1. We will now save the chart definition to the SPSS flow Graph node, click on add chart definition ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-2242aa75.png)
1. Click on the `[return to modeler button]`

Another way to get an understanding of the fields is to visualize them as a time plot:
1. From the `Graphs` drawer, select a `Time plot` node and wire it to the Data Asset node: ![](Lab5_StreamingTimeSeries.assets/20190214_3cdee63a.png)
1. Open the `Time Plot` node ![](Lab5_StreamingTimeSeries.assets/20190214_e944d710.png)
1. Select the `DATE_` as custom X axis, uncheck *separate panel* and *normalize*: ![](Lab5_StreamingTimeSeries.assets/20190214_76f6dce4.png)
1. Add all the `Market_*` columns: ![](Lab5_StreamingTimeSeries.assets/20190214_b3fac619.png) except `Total`, `YEAR_`, `MONTH_` and `DATE_` (Select all from the upper check box, then unselect the unwanted ones)   
![](Lab5_StreamingTimeSeries.assets/20190214_289f5a05.png)
1. Now Run the graph node: ![](Lab5_StreamingTimeSeries.assets/20190214_af6dc4b0.png)
1. Open the graph ![](Lab5_StreamingTimeSeries.assets/20190214_6b25cb47.png)
1. This gives an idea of how the markets behave, all are more or less increasing, with various amplitudes. ![](Lab5_StreamingTimeSeries.assets/20190214_b1c99040.png) To visualize relative volatility, you may want to re-run the graph node with `Normalize` checked.

### Process data to prepare for prediction
We will want to work on a subset of the markets rather than the 89 ones, and use the `DATE_` column as the time indicator.
1. For the purpose of the lab, we will subselect only the 5 first markets. From `Field Operations` palette drawer drop a `Filter` node and wire it to the Data Asset: ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-4ba7b46a.png)
1. Open the filter node from its menu, select to retain fields, and keep only `Market_1` to `Market_5` and `DATE_`: ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-ed56ad37.png)
1. In order to convert the `DATE_` string to a date-typed field, we will add a `Filler` node and wire it after the `Filter` node. Setup the `DATE_` column to fill-in, with condition `Always` ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-bb102460.png)
1. Set the `Replace with` formula to `to_date('DATE_')` ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-f2f3444b.png) and then `[Save]`
1. We will need to specify which fields are to be used as input or output to the prediction node. Add a *Type* node and wire it after the *Filler* node: ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-96d661da.png)

## Build the Streaming TimeSeries
1. Setup the *Type* node so that the `Market_*` fields have a role of `Both`, while the `DATE_` field has a role of `Input`: ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-c3e58b3b.png)
1. From `Record Operations` add a `Streaming TS` ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-8daf2852.png) node and wire it after the *Type* node.
1. Setup the *Streaming TS* node, add the `Market_*` fields both as *Targets* and *Candidate Input*: ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-b54ad7e6.png)
1. In *OBSERVATIONS AND TIME INTERVAL*, setup `DATE_` as Date/Time field, `Months` for time interval, `1` for the increment![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-1522e0ef.png)
1. In *Build Options - General*, setup *Expert Modeler*, for all models, un-select  *Seasonal* and select *Sophisticated Exponential Smoothing* methods: ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-c25c7da1.png)
1. In *Model Options*, setup forecast to `5` records in the future, and select confidence and residuals computation: ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-a05c4a71.png)
1. Run the *Streaming* node once using its `Run` menu item, in order to materialize the prediction columns. ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-5cdd3808.png)
1. We will get additional columns for the predictions:
   * `$TS-Market_1`: the sliding window prediction
   * `$TSLCI-Market_1` and `$TSUCI-Market_1`: the Lower and Upper Confidence Indicators for the prediction.
     * `$TSResidual-Market_1`: the computed residuals of prediction vs actuals
1. Add a *Time Plot* node from the `Graphs` drawer and wire it after the *Streaming* node. ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-4657fca8.png)
1. Configure the node to be named `Predictions`, using all attributes: ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-630f5a63.png)
1. Now run the *Time plot* node, this constructs the graph with predictions, which can be found in the *View outputs* panel: ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-af74a406.png)
1. Finally, from the view panel, select the eye button to display the predictions graph: ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-9f74356c.png)
1. Stretch Lab extension: You can work with the Graphs/Charts enhanced visualization node to explore and generate additional displays.

## Deploy Streaming TimeSeries model
1. In order to get the output of the deployment, add a `Table` node from the *Outputs* drawer, and wire it to the *Streaming TS* node: ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-d643abce.png)
1. From the *Table* node menu, select `Save branch as model`: ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-e54da7c0.png)
1. In the *Save Model* panel, make sure the `Table` branch is selected as terminal node, give it a name, e.g. `SPSS_Markets_Predictions` and save: ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-4cd19c16.png)
1. We will now deploy the model as a REST API endpoint. Switch back to your project, locate you newly saved model in the *Models* section and select *Promote* from its menu: ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-699ee469.png)
1. In the next *Promote to space* panel, select a target space (we can use the one created earlier, `ChurnDeplSpace`, or create a new one)![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-b0ed28db.png)
1. Once promoted to the space, we can add a deployment, switch to the deployment space ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-b5110149.png)
1. Click the *Deploy* button ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-61156029.png)
1. Select *Online* type, give it a name, e.g. `Online_Market_Predict`, and `[Create]` ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-145097ca.png)
1. From the Deployment tab ![](Lab5_StreamingTimeSeries.assets/Lab5_StreamingTimeSeries-21f824b2.png), you can navigate to the REST service definition, especially the implementation tab will show among other languages the Python client code to invoke prediction.

## Invoke the REST endpoint from a notebook
We could test the deployed service from the Cloud UI interface, but the volume of input data does not make it very practical.   
So, we will use a Python Jupyter notebook to test and display predictions.

1. The test notebook has been prepared for you. It is derived from the Python implementation sample code, but is using the WML Python wrapper library to find the model named `Online_Market_Predict`.
1. Switch back to your project and add an asset of type *Notebook*: ![](Lab5_StreamingTimeSeries.assets/20190215_57058974.png)
1. Switch to the *From file* tab, and choose  `WML_StreamingTS_Predictions.ipynb` notebook, select the 'Default Python 3.5 Free' environment: ![](Lab5_StreamingTimeSeries.assets/20190215_c7fc03df.png)
1. Follow the notebook execution flow, which will call the predictions WML endpoint and chart the results.
