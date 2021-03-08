# View application details

On the Application Details page of an application in the Tracing Analysis console, you can view the key metrics, call topology, and traces of the application on each server where it is deployed.

## View key metrics and call topology

On the **Application Details** page of an application, the Overview tab displays all servers where the application is deployed. You can sort the servers by response time, request count, or error count. After you select a server from the server list, you can view the detailed call topology of the application on the Overview tab. You can also view the time series curves of response time, request count, and error count.

1.  Log on [Tracing Analysis console](https://tracing-sg.console.aliyun.com/).

2.  In the left-side navigation pane, click **Applications**. At the top of the Applications page, select a region as required. Then, click the name of the application that you want to check.

3.  In the left-side navigation pane, click **Application Details**. Then, click **All** or select a server from the server list on the left. After that, you can view the call topology and key metrics on the **Overview** tab.

    **Note:** To sort the servers where the application is deployed, you can click the **Response Time**, **Requests**, or **Exceptions** tab, and then click the upward or downward arrow next to the name of the tab. To filter the servers, you can enter a keyword in the search box and click the search icon.


**Note:** To switch to another application in the same region, click the application name drop-down list in the upper-left corner and select another application.

## View traces

The **Call link**

![Tab Traces](../images/p53826.png "Trace tab")

**Note:** **Status** The green icon in the column indicates that the time consumed is less than 500 milliseconds, the yellow icon indicates that the time consumed is between 500 milliseconds and 1000 milliseconds, the red icon indicates that the time consumed is greater than 1000 milliseconds, or the Tag Key is `Error`.

On the **Call link** tab page, you can perform the following operations as needed:

-   In **Time-consuming** enter a time value \(in milliseconds\) in the adjustment box, and click **Query** to filter traces whose time consumption is greater than the specified value.
-   Select **Exception** and click **Query** to filter out abnormal call links.
-   Click **Time consumed** Or **Status** By using the up and down arrows on the right, you can sort the query results in ascending or descending order.
-   Click the TraceID to open it in a new window. Call link Page, and view the waterfall chart of the call link.

## View trace waterfall chart

Onthe Call link page, you can check the log generation time, status, IP address/machine name, service name, and Timeline of the call link are displayed on the page.

**Note:** **IP address** Whether the IP address or machine name is displayed depends on Application Settings the display configuration on the page. For more information, see manage applications and tags.

![Page Traces](../images/p53827.png "Trace page")

Place the cursor over a service name to view the service duration, start time, Tag, and log event information.

![Overlay Tag and Log Events](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3806359851/p53828.png)

## Set the query time range

You can select a preset time range or enter a custom time range.

-   Click the time option in the upper-right corner of the page, and then click a preset time range, such as **Last 30 minutes**,**This week**,**Last 30 days**.
-   If no preset time range meets your requirements, click **Custom**. Select the start time and end time from the calendar, or enter them manually in the text box. Then, click **OK**.

    **Note:** The date format is `YYYY-MM-DD`, The time format is `HH:MM`.


![Time Picker](../images/p53830.png "Query time range selector")

