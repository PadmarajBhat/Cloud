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
