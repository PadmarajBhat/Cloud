##### Ingesting Data into Cloud using App Engine
* .yaml file is used for deployment of an app into app engine( PaaS)
* scripting/automating obtain google cloud instance requires you to have knowledge of command driven google cloud meta data extractor:
    * ```PROJECT_ID=$(gcloud info --format='value(config.project)') ``` : to get the project name.
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
