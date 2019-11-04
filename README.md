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
