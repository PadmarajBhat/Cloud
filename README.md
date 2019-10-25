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
