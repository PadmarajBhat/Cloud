# Cloud

* Google cloud client stories :https://cloudblog.withgoogle.com/products/media-entertainment/building-an-ecosystem-of-partners-to-help-broadcasters-transform-their-business/amp/

* First Qwiklabs:
  * The hierarchy of the data in Big query is first the dataset and then the table. 
      * Can we query a dataset without a table ?
  * Big Query also enables us to Analyze data without a VM (compute engine or app engine)

* Compute Engine: IAAS
  * You select the underlying the hardware and operating system but maintenance is from google cloud
  
* App Engine: PAAS
 * You focus on the application envoirnment, underlying os and hardware is autoselected and maintained by cloud

* It is a best practice to keep the data and vm instance seperate so that a analytics (big query) can work on data. GCP provides storage services through BigTable, Storage, Spanner, SQL, DataStore.
Dataflow: pipe between data source to another eg. from stream to table
automl: help u to build a ml model suited for the current req. 
dialog flow, vision nlp etc are prebuilt google model for ready to use purpose
Data Studio: can work on dataset which can be either table or csv/json etc or can be on top of a big query results


* BigQuery:
 * let us do serverless Big Data Analysis
 * supports both legacy and standard SQL format
 * in my lab session 6GB of data was **accessed** for couple of queries. Which otherwise would have been too long.
 * Billing is done on the data accessed in the query. This can be calculated with the column(and its shape/size) used in the query.
 * Again, storage is seperated from the querying interface. i.e. support for serverless analysis. Just run query when you need it and then just close the big query window leaving the data reside on cloud storage.
 * Data can be loaded to BigQuery through command line or through UI. You can load csv(or any other types) data to a table
 * Data can also be exported to cloud storage. Therefore, from an external source directly import it to bigquery and through export keep a back up at cloud storage for future use.
 
 * The UNNEST operator takes an ARRAY and returns a table, with one row for each element in the ARRAY. 
 
* DataFlow
 * uses beam for the intermediate read and write
 * in Python : "|" indicates apply and ">>" indicates a name for a single step in the pipeline
 * you can initiate the dataflow job through command prompt. In the UI, you can see the top to bottom steps/stages. It executes in that order.
 * a tick mark appears indicating the completion for each stage.
 * at times the output UI might mislead with end step being tick before to prior stages. Wait for all stages to complete or output dir.
 * any stage can take "side input" (through) big query or real time data.

* TF in Cloud
 * https://playground.tensorflow.org/#activation=tanh&batchSize=10&dataset=circle&regDataset=reg-plane&learningRate=0.03&regularizationRate=0&noise=0&networkShape=4,2&seed=0.90213&showTestData=false&discretize=false&percTrainData=50&x=true&y=true&xTimesY=false&xSquared=false&ySquared=false&cosX=false&sinX=false&cosY=false&sinY=false&collectStats=false&problem=classification&initZero=false&hideText=false   : play ground to understand the impact of different layers
 * precision: How many corrects are predicted are correct.
 * Recall: Are categories predicted correct ? Plays important role when it comes to imbalanced data.
 
 * google cloud packages are available google datalab(notebook)
   ```from google.cloud import bigquery
   client = bigquery.Client()
   trips = client.query(sql).to_dataframe()
   trips
   ```
  * TF 1.x was focussed on graph building and then executing the graph. tf2.0 is eager execution by default.
  * ```tf.estimator.inputs.pandas_input_fn``` : loads the panda to tf; I guess better way is tf.data
  * TrainSpec and EvalSpec does the checkpoints and hence helps to have fault tolerance in case of failure
  * ```from google.datalab.ml import TensorBoard
TensorBoard().start('./taxi_trained')
TensorBoard().list()```

   * I guess , cloud ml does the same job as that of tensor hub
    * make sures input pipe line same for both training and prediction
    * versioning of models
    * availability of the model through rest apis
    * hyper parameter tuning and remembering it
  * Feature Engineering:
    * one hot encoding : we all know the importance of it but what if we cannot anticipant new value to the categorical feature? we will have to rebuilt the model
      * if customer rating is between 1-5. is it a continuous or categorical ? I think it can be categorical but it can be both. What if the rating is not given?
        * the author of the GCP Data Engineer course say that dont use -1. Instead use additional column indicating rating given or not.
           * why cant it be that -1 and then one hot encode it ?
           
           
      * Feature cross: It is programmer explicitly hinting the combined effect of 2 features. for eg. if there are relation between dayofweek and rush hour then feature cross would amplify the crossed value and thus indicating the model of the importance.
      * Bucketizing: a continuous value is grouped into buckets.
      * wide and deep: some features are linearly mapped to ouput and some features are mapped to output through dense layers. Sparse columns are best candidates for the "wide" connection.
      
