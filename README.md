# Pipelines-Fall25
Data Pipelines through Apache Spark &amp; Databricks


### Instructions (informal/not final)
- Create an account on Databricks Free Edition - would recommend personal email but school is probably fine
    - New > Notebook to create a notebook
    - Copy over imports and spark season builder from the given ode
- Create or Open your Supabase Account
    - Go to storage > New Bucket - call it whatever (probably “bluebikes”)
    - In the Storage menu, go to Configuration > Settings
        - Copy the endpoint, but only from after the // to the supabase.co into the 
        - Copy the access key and secret key into the proper spots in the python
- Explain the data: https://bluebikes.com/system-data
    - Go to realtime data -> click “Get Bluebikes’ GBFS feed here”
        - This shows the json file/raw data that we’re working with which is a snapshot of the bluebikes system updated every minute
        - Click on the url with the name station_status and click into stations to see the real data we’re gonna be reading in
- So now we want to write a schema to match this data
    - Using only the main fields we care about
**Extract**
- Type out requests.get(URL, timeout=10)
- Data = response.json(), print that out, try running - if that works then we have data
- Also station_information to get names, keep just name and id
**Transform**
- Stations_data to get into the list we want
- Now we add an ingestion timestamp to every station
- Now create a spark dataframe reading in the data with the schema above
- Add some calculated columns, typed out with withColumn
- Run a .show(truncate=False) to show something is happening
- Create stats as an aggregated view of the whole system
**Load**
- Convert processed_df to pandas, and then to pyArrow, then write Parquet to buffer, then boot up boto3 with the same credentials
- Then the line to actually put the object in the bucket
- Check supabase to ensure this worked
- Write stats table to memory on databricks as delta table

**SQL/Dashboard**
- Go into SQL Editor and notice how you can do SELECT * FROM bluebikes_stats
- Dashboards > Create Dashboard - click the line graph icon at the bottom to add a visual, and either ask the AI or do it yourself - we want a line graph where the x axis is ingestion time and the y axis is - utilization rate - this gives us a live tracker of the peaks and troughs of bluebike usage
- Play around a little with the size and turn the y axis to having specific boundaries

**Streaming/Fake Streaming**
- Jobs & Pipelines > Create Job
- Under the task stuff, name it whatever, set the path to your notebook (if should come up). Set the schedule (Add Trigger) to continuous - it will run approximately every 30 seconds after the first few minutes (by few it may mean 30)
- Make sure compute is serverless
- Then start it going
