# Create an alert

By creating alerts, you can set alert rules for specific monitored objects. When a rule is triggered, the system will send an alert message to the specified contact group in the specified alerting mode. This reminds you to take necessary actions to solve the problem.

-   You have created contacts. You can only set a contact group as the notification receiver of an alert.

Default behaviors of alert notifications:

-   To prevent you from receiving a large number of alert notifications in a short period of time, the system only sends one message for repeated alerts within 24 hours.
-   If no duplicate alerts are generated within five minutes, Application Real-Time Monitoring Service \(ARMS\) sends a recovery email to notify you that the alert has been cleared.
-   After a recovery email is sent, the alert status is reset. If this alert arises again, it is deemed as a new one.

An alert widget is essentially a data display method of datasets. When you create an alert widget, a dataset is created to store the underlying data of the alert widget.

**Note:** New alerts take effect within 10 minutes. The alert check may have a delay of 1 to 3 minutes.

## Create an alert

To create an alert for an application monitoring job on Java Virtual Machine-Garbage Collection \(JVM-GC\) times in corresponding-period comparison, perform the following steps:

1.  Log on to the console. Click the target application in **Applications**. In the left-side navigation pane, choose **Alerts** \> **Alert Policies**.

2.  On the Alert Policies page, click **Create Alert** in the upper-right corner.

3.  In the **Create Alert** dialog box, enter all required information and click **Save**.

    1.  Enter **Alert Name**, for example, alert on JVM-GC times in corresponding-period comparison.

    2.  Select an application for **Application Site** and an application group for **Application Group**.

    3.  In the **Type** drop-down list, select the type of the monitoring metrics, for example, **JVM\_Monitoring**.

    4.  Set Dimension to **Traverse**.

    5.  Set alert rules.

        1.  Select **Meet All of the Following Criteria**.
        2.  Edit the alert rule. For example, an alert is triggered when the value of N is 5 and the average value of JVM\_FullGC increases by 100% compared with that in the previous hour.

            **Note:** To add another alert rule, click **+** on the right of **Alert Rules**.

    6.  Set Notification Mode. For example, select Email.

    7.  Set Notification Receiver. In the **Contact Groups** box, click the name of a contact group. If the contact group appears in the **Selected Groups** box, the setting is successful.


## Description of basic fields

The following table describes the basic fields of the **Create Alert** dialog box.

|Field|Description|Remarks|
|-----|-----------|-------|
|Application Site|The monitoring job that has been created.|Select a value from the drop-down list.|
|Type|The type of the metric.|The types for the three alerts are different: -   Application monitoring alerts: This displays application entry calls, the statistics of application call types, database metrics, JVM monitoring, host monitoring, and abnormal interface calls.
-   Browser monitoring alerts: This shows page metrics, interface metrics, custom metrics, and page interface metrics.
-   Custom monitoring alerts: This creates alerts based on existing drilled-down datasets and existing general datasets. |
|Dimension|The dimensions for alert metrics \(datasets\). You can select None,"=", or Traverse.|-   When it is set to None, the alert content shows the sum of all values of this dimension.
-   When it is set to "=", you need to enter the specific content.
-   When it is set to Traverse, the alert content shows the dimension content that actually triggers the alert. |
|Last N Minutes|The system checks whether the data results in the last N minutes meet the trigger condition.|Range of N: 3 to 3600 minutes.|
|Notification Mode|Email, SMS, and DingTalk chatbot are supported.|You can select multiple modes. If you want to set DingTalk chatbot alert, see Enable DingTalk chatbot alert.|
|Alert Quiet Period|You can enable or disable Alert Quiet Period. By default, it is enabled.|-   When it is enabled: if data remains in the triggered state, the second alert message will only be sent 24 hours after the first alert is triggered. When data recovers, you will receive a data recovery notification and the alert will be cleared. If the data triggers the alert one more time, the alert message is sent again.
-   When it is disabled: if the alert is continually triggered, the system sends the alert message every minute. |
|Alert Severity|Valid values include Warn, Error and Fatal.|None|
|Notification Time|The time when the alert was sent. No alert notification is sent out of this time period, but alert events are recorded.|For more information about alert event history, see Alert management.|
|Notification Content|The custom content of the alert.|You can edit the default template. In the template, the four variables, $AlertName, $AlertFilter, $AlertTime, and $AlertContent, are preset. \(Other preset variables are not supported currently.\) The rest of the content can be customized.|

## Description of complex general fields: Period-over-period comparison

-   N-minute-on-N-minute comparison: Assume that β is the data \(optionally average, sum, maximum, or minimum\) in the last N minutes, and α is the N-minute data starting from 2N minutes ago. The N-minute-on-N-minute comparison is the percentage increase or decrease of β as compared to α.
-   N-minute-on-N-minute hourly comparison: Assume that β is the data \(optionally average, sum, maximum or minimum\) in the last N minutes, and α is the N-minute data from an hour ago. The N-minute-on-N-minute hourly comparison is the percentage increase or decrease of β as compared to α.
-   N-minute-on-N-minute daily comparison: Assume that β is the data \(optionally average, sum, maximum or minimum\) in the last N minutes, and α is the N-minute data a day ago. The N-minute-on-N-minute daily comparison is the percentage increase or decrease of β as compared to α.

## Description of complex general fields: Alert data revision strategy

You can select "Zero fill", "One fill", or "Zero fill null" \(default\). This feature is generally used to fix anomalies in data, including no data, abnormal composite metrics, or abnormal period-on-period comparison.

-   Zero fill: fixes the value checked to 0.
-   One fill: fixes the value checked to 1.
-   Zero fill null: does not trigger the alert.

Scenarios:

-   Anomaly 1: No data

    User A wants to use the alert feature to monitor the page views. When creating the alert, user A selects Browser Monitoring Alert. User A sets the alert rule as follows: N is 5 and the sum of the page views is at most 10. If the page is not hit, no data is reported and no alert is sent. To solve this problem, you can select "Zero fill" as the alert data revision policy. If you do not receive any data, it considered that zero data is received. This meets the alert rule and an alert is sent.

-   Anomaly 2: abnormal composite metrics

    User B wants to use the alert feature to monitor the real-time unit price of a product. When creating the alert, user B selects Custom Monitoring Alert. User B sets the dataset of variable a to the current total price, and the dataset of variable b to the current total items. User B also sets the alert rule as follows: N is 3 and the minimum value of current total price divided by current total items is at most 10. If the current total of items is 0, the value of the composite metric, current total price divided by current total items, does not exist. No alert will be sent. To solve this problem, you can select "Zero fill" as the alert data revision policy. The value of the composite metric, current total price divided by current total items, is now considered as 0. This meets the alert rule and an alert will be sent.

-   Anomaly 3: Abnormal period-on-period comparisons

    User C wants to use the alert function to monitor the CPU utilization of the node machine. When user C creates the alert, C selects Application Monitoring Alert, and sets the alert rule as follows: N is 3 and the average user CPU utilization of the node machine decreases by 100% compared with the previous monitoring period. If the user's CPU fails to work in the last N minutes, the α cannot be obtained. This means the period-on-period result does not exist. No alert is sent. To solve this problem, you can select the alert data revision strategy as "One fill", and consider the period-on-period comparison result as a decrease of 100%. This meets the alert rule and an alert will be sent.


You can query and delete alert records in alert management.

