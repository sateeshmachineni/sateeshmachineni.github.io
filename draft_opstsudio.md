Create reports using SQLOperations studio

Microsoft released SQL Operations studio for Mac,Linux and Windows.This editor is highly configurable,one particular thing caught my attention was creating Insights.. you can think of this as SSMS Inbuilt reports..

Let's see how we can create a new Insight using OpsStudio..

Try connecting to SQLOperations studio and run below query to get [wait stats](https://www.mssqltips.com/sqlservertip/1949/sql-server-sysdmoswaitstats-dmv-queries/)

```sql

WITH Waits AS
 (
 SELECT 
   wait_type, 
   wait_time_ms / 1000. AS wait_time_s,
   100. * wait_time_ms / SUM(wait_time_ms) OVER() AS pct,
   ROW_NUMBER() OVER(ORDER BY wait_time_ms DESC) AS rn
 FROM sys.dm_os_wait_stats
 WHERE wait_type 
   NOT IN
     ('CLR_SEMAPHORE', 'LAZYWRITER_SLEEP', 'RESOURCE_QUEUE',
   'SLEEP_TASK', 'SLEEP_SYSTEMTASK', 'SQLTRACE_BUFFER_FLUSH', 'WAITFOR',
   'CLR_AUTO_EVENT', 'CLR_MANUAL_EVENT')
   ) -- filter out additional irrelevant waits
   
SELECT top 10 W1.wait_type,
 CAST(W1.wait_time_s AS DECIMAL(12, 2)) AS wait_time_s,
 CAST(W1.pct AS DECIMAL(12, 2)) AS pct,
 CAST(SUM(W2.pct) AS DECIMAL(12, 2)) AS running_pct
FROM Waits AS W1
 INNER JOIN Waits AS W2 ON W2.rn <= W1.rn
GROUP BY W1.rn, 
 W1.wait_type, 
 W1.wait_time_s, 
 W1.pct
HAVING SUM(W2.pct) - W1.pct < 95
 order by wait_time_s desc;
 
 ```
 
 and save this query as `Waitstats.sql`.
 
 
 Now create insight by clicking view charts and create insight as shown below
 
 
 
 Finally you will be presented with below screen ,which is JSON.IF you want to know more about JSON [click here](https://www.copterlabs.com/json-what-it-is-how-it-works-how-to-use-it/)
 
 Below is the JSON i have,you can change size and name of widget as per your wish.I have changed mine to below
 
 ```sql
 
 {
    "name": "WaitStats",
    "gridItemConfig": {
        "sizex": 3,
        "sizey": 3
    },
    "widget": {
        "insights-widget": {
            "type": {
                "horizontalBar": {
                    "dataDirection": "vertical",
                    "dataType": "point",
                    "legendPosition": "none",
                    "labelFirstColumn": false,
                    "columnsAsLabels": false,
                    "encoding": "hex",
                    "imageFormat": "jpeg"
                }
            },
            "queryFile": "c:\\sqlops-windows-0.23.6\\Queries\\waitstats.sql"
        }
    }
}
```
 
 Now press <kbd>CTRL+SHIFT+P</KBD> and type User Settings as shown below
 
 <img  src="/img/output_CarRyl.gif"/>
 
 
 Now type Database in settings search tab and hover mouse over `dashboard.database.widgets`  and click EDIT as shown below
 
 <img  src="/img/Settings.png"/>
 
 Copy the Insights in the settings and save it
 
 <img  src="/img/databasewidgets.png"/>
 
 You can view the widget by right clicking the database and clicking on manage.Below is my Widget
 
 <img  src="/img/insights.png"/>
 
 
 
 
 
 
