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
