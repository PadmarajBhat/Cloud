##### Ingesting Data into Cloud using App Engine
* .yaml file is used for deployment of an app into app engine( PaaS)
* scripting/automating obtain google cloud instance requires you to have knowledge of command driven google cloud meta data extractor:
    * ```PROJECT_ID=$(gcloud info --format='value(config.project)') ``` : to get the project name.
export BUCKET=$PROJECT_ID-ml```
    * ```export ADDRESS=$(wget -qO - http://ipecho.net/plain)/32``` : to get my ip of the vm where this command runs.
    * ```MYSQLIP=$(gcloud sql instances describe flights --format="value(ipAddresses.ipAddress)")``` : to get the ip address of the sql insteance in your project. Note that there are already some project values like region and name present in the gcloud session which these command leverages to shorten the commands.
    
 * In SQL, you can have variable name defined and then the same can be used in query.
      * ```SET @ARR_DELAY_THRESH = 15;```
      * ```select count(dest) from flights where arr_delay < @ARR_DELAY_THRESH and dep_delay < @DEP_DELAY_THRESH;```
* Beam can read directly the gz file
   ```
   beam.io.ReadFromText('airports.csv.gz')
   
   ```
   runner : --runner=DataflowRunner indicates beam that it has to run in GCP Dataflow. Where first cluster of pool is created which can autoscale then the pipe is executed.


* Execution Detail in the big query gives you the understanding of the query time spent and can be used to compare 2 queries.

##### EDA through Big Query
* CSV file was in google bucket
* was loaded to bigquery through command interface where schema file was also presented to interpret comma seperated fields.
* Notebook was started : AI -> Notebook
* We had to create a notebook instance. Predefine instance configuration were available like TF 1.x, tf2.0, RAPIDS XGBoost. I chose tf1.x as indicated. which gave node [n1-standard-4 (4 vCPUs, 15 GB memory)].
* Jupyter Lab was launched with Python 3 kernel
      * ```!pip freeze``` : gives you list of softwares installed in the kernel environment
* ```!pip install --upgrade google-cloud-bigquery``` : installed bigquery package for python
* like RAPIDS, below query loaded a df
   ```import google.datalab.bigquery as bq
      sql = """
      SELECT ARR_DELAY, DEP_DELAY
      FROM `flights.tzcorr`
      WHERE DEP_DELAY >= 10 AND RAND() < 0.01
      """
      df = bq.Query(sql).execute().result().to_dataframe()
      df.describe()```
      
* Now that we have panda dataframe, EDA principle are good to from this point.
* Now in this case,  only 13k records were processed. In case of BigData, how do we analyze the data?
      * In case of BigData first do the Univariable analysis
      * from that identify the key bi variable analysis
      * finally the multi variable analysis for insights
