# Macro example

* to create seperate sheet for each equity at NSE
```
function M4() {
  var spreadsheet = SpreadsheetApp.getActive();
  var quoteList = ["NSE:FORCEMOT"
,"NSE:KENNAMET"
,"NSE:AARTIIND"
,"NSE:ABB"
];
  
  quoteList.forEach((x)=>{
    spreadsheet.insertSheet(x);
    spreadsheet.getRange('A1').activate();
    spreadsheet.getActiveCell().setValue(x);
                    
    spreadsheet.getRange('A2').activate();
    spreadsheet.getCurrentCell().setFormula('=GOOGLEFINANCE(A1,"all","1/1/1994","6/3/2020","DAILY")');
                    });
  
};
```

* https://www.codingforentrepreneurs.com/projects/python-google-colab-sheets-drive/google-sheet-pandas-dataframe
* Triggers to the sheet failed with ```We're sorry, the JavaScript engine reported an unexpected error. Error code DEADLINE_EXCEEDED```
  * The manual says the execution took longer period, but according to my calculations it is the same behvior to that of pysheet delayed response.
    * the reason may be : https://www1.nseindia.com/content/press/Download_Real_Time_Tariff_Domestic_01042020.pdf
    * or how about randomizing the list so that map does not yeild same list values
    * how about IST day time run ?
    * how about weekly run ?

* https://colab.research.google.com/notebooks/io.ipynb#scrollTo=u22w3BFiOveA: how to access pydrive, sheet and google drive mount
  * hoping that colab gives extra hourse power. Note that gsheet were not read by pygsheets
  
* merging all dfs together is faster : https://stackoverflow.com/questions/38246166/efficient-way-to-combine-pandas-data-frames-row-wise
* the required format for google finance timestamp : strftime("%m/%d/%Y")
* **Error**: WARNING:googleapiclient.http:Sleeping 7.54 seconds before retry 3 of 3 for request: GET
  * Seems like ignorable warning : may be due to too fast query execution ???
    * yes: It can be ignored as it is loading the data for it.
    
* **Error** : HttpError: <HttpError 503 when requesting https://sheets.googleapis.com/v4/spreadsheets/1ewMEcVHtLs6PBWdFZ3bgImFLcUqXcB4yXVWr4kns9no?fields=properties%2Csheets%2Fproperties%2CspreadsheetId%2CnamedRanges&includeGridData=false&alt=json returned "The service is currently unavailable.">
  * Parallelism also caused burden on the host which has blocked the app
    * sequential execution took 1:30hrs
