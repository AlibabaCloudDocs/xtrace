# Analyze traces

After analyzing the call link information of an application on the call link analysis page, you can filter the call link information by condition. you can also view the link topology, real-time aggregate link table, and call link waterfall chart.

## View trace information

1.  Log on [Tracing Analysis console](https://tracing-sg.console.aliyun.com/).

2.  In the left-side navigation pane, click**Application List**, And inApplication ListSelect a region and click the name of the target application.

3.  In the left-side navigation pane, click**Trace Analysis**, InTrace AnalysisPage to filter trace information, as shown in the following figure.

    -   Click **Comprehensive condition**input box, which can be added simultaneously or separately **Span name**, **IP** and**Label**. Three filter criteria. When you add a filter, you can add one **Span name** or **IP**, And multiple **Label**.
    -   In**Time consumed \(\> ms\)**Enter a specific response time in the input box to query trace information that is later than this time consumption.
    -   Check**Exception**Check box to query trace information with exceptions.
4.  Click**Search**To view the filtered trace information, including:

    -   Time series curves of time consumption and number of spans.
    -   The distribution chart of the number of spans and the time consumed.
    -   You can filter call link information by Span, IP address, or Tag.
    ![Trace Analysis](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7955458061/p63933.png)

5.  From**Group by**List, select Span, IP, or Tag to filter groups. For example, select Tag**IP**.

    ![Trace List](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7955458061/p63959.png)

6.  Click a IP. A list of trace information related to this IP is displayed.

    ![Trace List](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7955458061/p63960.png)


## View the link topology

The link topology mainly displays the topology of the dependency between applications after conditional filtering, as well as the request ratio, call speed, and time consumption ratio between applications. In consideration of the performance experience, the link topology can pull up to 5000 link requests for aggregation.

1.  ClickLink topologyTab to view the link topology.

    ![Trace Topology ](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4876458061/p66600.png)

    **Note:**

    -   Request ratio = number of requests requested by an application for external calls/total number of requests of an application. For example, if 100 requests enter upper-layer application A and only 90 requests from A call lower-layer application B, the request ratio from A to B is 90%. \( In application A, some requests may not go into application B because some requests may be filtered by if judgment.\)
    -   Call multiple = number of spans called by the application/total number of spans of the application. For example, if 100 spans enter upper-layer application A and 300 spans call lower-layer application B from A, the call multiple from A to B is 3. For example, the values of A to B are 90%/3x, indicating that 90% of requests from application A call Application B, and application A Calls application B three times on average.

## View the real-time aggregation link table

Real-time aggregation is a call link table that aggregates call links that have been filtered by conditions based on Span names and application names. In consideration of performance experience, real-time aggregation supports pulling up to 5000 link requests for aggregation.

1.  ClickReal-time aggregationTab to view the real-time aggregation link table.

    ![Trace Real-time Aggregation](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4876458061/p66616.png)

    **Note:**

    -   Requests/request ratio: The request ratio indicates the percentage of requests that call the current Span node. For example, if the total number of requests is 100 and the request ratio is 10%, 10 requests call the current Span. Calculation formula = number of requests for the current Span/total number of requests X 100%.
    -   Span/request multiple: The request multiple indicates the average number of times each request calls the current Span. For example, 1.5x indicates that each request calls the current Span 1.5 times on average. Calculation formula = number of spans/number of requests per Span.
    -   Average self-elapsed time/ratio: the average self-elapsed time does not include the average elapsed time of subspans. For example, if Span A to B, the elapsed time of A is 10 milliseconds, and that of B is 8 milliseconds. the time consumed by A is 2 milliseconds. The formula is as follows: Span elapsed time-Sum \(subspan elapsed time\). If an Asynchronous call is performed, the child time consumed is not subtracted. The formula is as follows: Span time consumed.
    -   Exception count/exception percentage: The exception percentage indicates the percentage of requests with exceptions. For example, 3% indicates that 3% of requests have exceptions. Calculation formula = number of abnormal requests/total number of requests. The number of exception requests is not equal to the number of exceptions. If the request multiple is greater than 1, an exception request may correspond to multiple exceptions.
2.  Hover the mouse over the blue Span name to displayRecommended call linkPrompt information, you can view the call chains associated with this Span. Click a trace id. The trace waterfall chart is displayed. For more information, see[View trace waterfall chart](#section_nn0_y6s_g63).


## View trace waterfall chart

1.  Click a trace ID in the trace list. The trace waterfall chart is displayed.

    InCall linkYou can view the Span name, application name, status, IP address/machine name, log generation time, and Timeline of a call chain.

    **Note:** **IP address**Whether the IP address or machine name is displayed depends onApplication SettingsThe display configuration on the page. For more information, see[Manage apps and tags](/intl.en-US/Use the console/Application management/Manage applications and tags.md).

    ![Page Trace](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4876458061/p63969.png)

2.  Place the cursor over a Span name to view the length, start time, Tag, and log events of the Span.

    ![Overlay Tag and Log Events](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4876458061/p63977.png)


## Set the query time range

You can select a preset time range or enter a custom time range.

-   Click the time option in the upper-right corner of the page, and then click a preset time range, such as **Last 30 minutes**,**This week**,**Last 30 days**.
-   If no preset time range meets your requirements, click **Custom**. Select the start time and end time from the calendar, or enter them manually in the text box. Then, click **OK**.

    **Note:** The date format is`YYYY-MM-DD`, The time format is`HH:MM`.


![Time Picker](../images/p53830.png "Query time range selector")

[Before you begin](/intl.en-US/Setup/Before you begin.md)

[Manage apps and tags](/intl.en-US/Use the console/Application management/Manage applications and tags.md)

