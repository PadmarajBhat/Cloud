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
