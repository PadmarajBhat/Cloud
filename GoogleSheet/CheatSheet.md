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
