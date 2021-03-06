# Understanding Your GCP Costs
* Managing Billing Accounts
  * quick revision: resource hierarchy : organization --> folder levels --> projects 
  * billing users:
    * viewer : has access to only view the bills associated with a project or set of project
    * user : can add project to billing account
    * administrator : has all the previlages of other 2 and assign user to the account
  * Manage billing account access:
    * periodically check for billing viwers
    * viwers can be given permission to view a project and hence they would get only one project billing view.
  * in the lab:
    * overview of billing functionalities like reports, break down and alerts.
    * filters can be put per product or SKU (stock keeping unit) to understand what actually cnsumes the bigger pie in the billing
    * multi-region project can also put filters to region wise expenditure trend
    * duration can also be there to see the year/month/date range expenditure.
  * In the lab Analyzing Billing Data with BigQuery :
    * the billing data can be exported for bigquery analysis. why ? because unlike anyother storage in GCP, it does not require any dedicated storage set up as in db or schema etc. and SQL querying can ease up analysis. Further more Jupyter notebook can fetch the big query output to dataframe.
    * key fields look for are Line item, cost, project name, etc
    
* In the lab, Visualizing Billing Data with Data Studio :
  * the lab focussed on pie chart and hence only metric was changed to num which represeted the total number of entries per line item
  * then I had to select the pie chart from the chart type
  * there was one cruicial thing, that was sorting. Well, from the concept wise it was not a big deal but the auto changes of the pie chart based on ascending or descending sort was something new.

* In the lab, Examining BigQuery Billing Data in Google Sheets:
 * they talked about connecting to big query through sheets and query editor in sheets decides the sheet data content.
 * the edit to query refreshes the data in the sheet
 * sheet has to be periodically refreshed if the underlying source changes which can be automated through macro and script editor.
 * fantastic thing was to use **explore** where NLP driven query processor resulting in data output. It could understand the simple english query to create pivot or bar charts. of course, charts and pivot can later be edited. 


# Optimizing Your GCP Costs
* Setting up budgets and alerts for cost management:
  * An alert can be set for either number of resources or rate of usage (like api access)
  * alert can be put on actual or forecasted ( it might alert way ahead of actual consumption )
  * budget can be set for overall project or indivisual components.
  
* Setting Up Cost Controls with Quota:
  * Quota can restrict the users from allocating resources which helps in controlling the budget
  * User has to have product owner previlage to set up quotas
  * Quotas not only help to control billing but also control unintentional access of resources like test system accessing too many big query resources.
  * Big query has fine grain control over its feature like streaming and data volume etc. So idea here is to constantly review billing report and fine tune quotas as per budget constraints
* Committed use discounts:
  * committment from resources creator for a longer term like 1 or 3 years usage. There can be significant discount in this case.
  * this has to be purchased before use of the resource to avail the discount. Irrespective of usage of the resource, a discounted rate will be part of the monthly bill
  * by reviewing the CUD report we can decide if further committed resources has to be purchased. 
  * it differs from the sustained usage discount with the fact that it is post usage discount where cud is pre purchased.
  
* Setting up advanced cost control:
  * pubsub can be used in the budget creation to have the programmatic notification to stop the resources as an example. May be later technical and IT team can investigate the extreme use.
  * cloud function for the quick and serverless code execution. Since both pubsub and functions are only trigger based their consumption is negligible when it comes to overall compute engine consumption.
  * budget can be set on billing account that encompasses multiple projects and products.
  * set different quotas for dev, test and production with constant monitoring over resources.
  * perday limit resets everyday

* popular stacks:
   * MEAN : Mongodb, express.js, Angular.js and node.js
   * LAMP: Linux, Apache, mysql and PHP

* Lab 1:
  * Big query execution indicates that the data consumption is over a TB
  * second run of the same sql query was fetched from cache
  * on setting the quota for perday usage size the big query execution failed giving quota warning. Note that in this case cache was explicitly disabled so that query re-fetches the data.  
  
* Lab 2:
  * Stack driver helps to monitor various metrics depending on the resource types. email notification can be sent on the basis of alert.
  * can it raise a pubsub topic ? 
  * new thing was uptime check for a resource. this may be suitable for system rarely used but the much needed whenever required. so you would want to keep uptime check and requests/response or any other alerts
  
* Lab 3:
  * Cloud function to listen to a topic in pubsub.
  * function code simply reads the message in the topic and writes it to console.
  * it is a known fact that platform on which runs is abstract. But can a script read the system info and share the same ?
     * I guess we can use navigator as indicated in https://stackoverflow.com/questions/9514179/how-to-find-the-operating-system-version-using-javascript
  * cloud function can be easily tested through gcloud command:
    * ```gcloud functions call helloWorld --data '{"message":"Hello World!"}'```
   * to view logs through gcloud (you can also view it through google cloud console)
    * ```gcloud functions logs read helloWorld```
  
* Lab 4: 
  * a new concept to me that there can be seperate static external ip address.
  ```
  gcloud compute addresses create $USED_IP --project=$PROJECT_ID --region=us-central1
  ```
    * creates no compute instance but a static ip address for a project that belong to us-central1. *In use by* field is empty for the created ip address.Now this ip can be assigned to any VM that is yet to be created (while creating it).
    * Like attaching, even *external **static** ip address* can be detached from a resource( VM instance) and can be attached to some other resource. Now when *external* ip address is detached resources will have internal ip addres.
    
   * in this lab a cloud function runs to search for all the unused ip address and then it deletes them.
   
* Lab 5:
  * In this final lab, a cloud function converts the storage class of a bucket to Nearline through a python program.
  * It would have been best if the bucket is cahnged to different class based on the storage access but it was a dummy to change the bucket statically to different class
      * stack monotoring alert to raise pubsub even to indicate that for 5 min (or 5 years) the bucket is not accessed
      * cloud function gets triggered based on that and reads the alert bucket name 
      * then the cloud function changes the class to nearline
      * there can be one more alert to change it back to region as soon as 10-20 access request come to it.
