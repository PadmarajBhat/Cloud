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
