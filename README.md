# Pipelines-Fall25
Data Pipelines through Apache Spark &amp; Databricks

This workshop would not be possible without our wonderful head of technical staff, Sam. (Freshmen, you will get to meet him next semester.) 

Amanda added the finishing touches. 

Workshop Note: On Mac, Chrome (not Safari) is the preferred browser for this project as it will help with formatting, although either should work perfectly fine.

### Instructions
1. **Clone this to your local machine.** Please notify us if you need any help!
2. **Create an account on Databricks Free Edition.** Personal email is recommended for the account, but school email works as well.
    - **Create the notebook**
        - to New > Notebook
        - File > Import > browse from local files.
        - Use the *pipelines_incomplete.ipynb* to follow along for coding, or *pipelines_workshop* if you just want to observe.
        - See the notification of the new notebook created in the top right. Click on the link on the notebook name.
    - **Make sure the environment is in version 2**-- AWS S3 doesn't work on version 4.
        - Go to the right side and click on the environment (a symbol with two lines and circles) and set the environment version to 2 in the drop-down. 
3. **Open your Supabase Account** or create one if you don't have one. 
    - **Create a new bucket:**
        - Go to storage > New bucket - give it a name (probably “bluebikes”)
    - **Get the endpoint and access keys**.
        - In the Storage menu, go to Configuration > Settings
        - Copy the endpoint, but *only from after the // to the supabase.co*. We have already hard-coded the other elements into the ipynb.
        - Copy the access key and secret key into the proper spots in the ipynb file. Be careful as you will have to make new keys if you lose the secret keys.
4. **Write a schema for the data**
    - Let's look back at the data. Go to https://bluebikes.com/system-data.
        - For Mac users, Chrome is recommended as it comes with "Pretty-print" for the JSON files while Safari does not.
        - Scroll down to "Real-time Data" -> click “Get Bluebikes’ GBFS feed."
        - This shows the JSON file/raw data that we’re working with, which is a snapshot of the bluebikes system updated every minute.
        - Click on the url with the name *station_status* and click into stations to see the real data we’re gonna be reading in.
    - Write a schema to match the data using *only* the main fields that we care about.
5. **ETL**
  
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

6. **Do "Streaming" (Really Frequent Batch Processing)**
- **Create a pipeline**
    - Jobs & Pipelines > Create Job
    - Under the task stuff, name it whatever, set the path to your notebook (if should come up).
    - **Add a trigger** Set the schedule (Add Trigger) to continuous - it will run approximately every 30 seconds after the first few minutes (by few it may mean 30)
    - Make sure compute is serverless
    - Then start it going
  
7. **Run SQL and Create a Dashboard**
- **Run an SQL query**
    - Go into SQL Editor in Databricks and notice how you can do *SELECT * FROM bluebikes_stats*
- **Create a dasbboard**
    - Dashboards > Create Dashboard - click the line graph icon at the bottom to add a visual, and either ask the AI or do it yourself - we want a line graph where the x axis is ingestion time and the y axis is - utilization rate - this gives us a live tracker of the peaks and troughs of bluebike usage
    - Play around a little with the size and turn the y axis to having specific boundaries

**YOU DID IT!! BE PROUD OF YOURSELF! :D**


