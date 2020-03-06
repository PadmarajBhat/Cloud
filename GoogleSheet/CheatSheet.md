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
