# Create an alert

By creating alerts, you can set alert rules for specific monitored objects. When a rule is triggered, the system sends an alert notification to the specified contact group in the specified alerting mode. This reminds you to take necessary actions to solve the problem.

-   Contacts are created. Only contact groups can be set for the notification receiver of an alert.

Default behaviors of alert notifications:

-   To prevent you from receiving a large number of alert notifications in a short period of time, the system sends only one message for repeated alerts within 24 hours.
-   If no repeated alerts are generated within 5 minutes, the system sends a recovery email to notify you that the alert has been cleared.
-   After a recovery email is sent, the alert status is reset. If this alert arises again, it is deemed as a new one.

An alert widget is essentially a data display method for datasets. When you create an alert widget, a dataset is created to store the underlying data of the alert widget.

**Note:** New alerts take effect within 10 minutes. The alert check may have a delay of 1 to 3 minutes.

To create a Java Virtual Machine-Garbage Collection \(JVM-GC\) alert for an application monitoring job, perform the following steps:

1.  In the **create alarm** dialog box, enter all required information and click **save**.

    1.  Enter the **alert name**, for example, the number of JVM-GC than the same time.

    2.  In **application site** list, select the application in **application group** list, select the application Group.

    3.  In the **type** section, select a metric type, for example, **JVM Monitoring**.

    4.  Set the dimension to **traverse**.

    5.  Configure an alert rule.

        1.  Click **both**. The following rules are met.
        2.  Edit the alert rule. For example, an alert is triggered when the value of N is 5 and the average value of JVM\_FullGC increases by 100% compared with that in the previous hour.

            **Note:** To add another alert rule, click **alert rules** \(+\) on the right of **+**.

    6.  Set Notification Mode. For example, select Email.

    7.  Set Notification Receiver. In the **contact groups** box, click the name of a contact group. If the contact group appears **in the selected groups** box, the setting succeeds.

    ![Application Monitoring Alarm](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3608828061/p174561.png)


## Description of basic fields

The following table describes the basic fields in the **create alarm** dialog box.

![Create Alarm dialog box](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6128828061/p174562.png)

|Field|Description|Remarks|
|-----|-----------|-------|
|Application Site|The monitoring job that has been created.|Select a value from the drop-down list.|
|Type|The type of the metric.|The types for the three alerts are different: -   Application monitoring alert: This displays application entry calls, the statistics for application call types, database metrics, JVM monitoring, host monitoring, and abnormal interface calls.
-   Browser monitoring alert: This shows page metrics, interface metrics, custom metrics, and page interface metrics.
-   Custom monitoring alert: This creates alerts based on existing drilled-down datasets and existing general datasets. |
|Dimension|The dimensions for alert metrics \(datasets\). You can select None, "=", or Traverse.|-   When Dimension is set to None, the alert content shows the sum of all values of this dimension.
-   When Dimension is set to "=", you must enter the specific content.
-   When Dimension is set to Traverse, the alert content shows the dimension content that actually triggers the alert. |
|Last N Minutes|The system checks whether the data results in the last N minutes meet the trigger condition.|Valid values of N: 1 to 60.|
|Notification Mode|Email, SMS, ,Ding Ding Robot, and Webhook are supported.|You can select multiple modes. If you need to set a DingTalk robot alarm see .|
|Alert Quiet Period|You can enable or disable Alert Quiet Period. By default, Alert Quiet Period is enabled.|-   When Alert Quiet Period is enabled: if data remains in the triggered state, the second alert notification is sent 24 hours after the first alert is triggered. When data is recovered, you receive a data recovery notification and the alert is cleared. If the data triggers the alert one more time, the alert notification is sent again.
-   When Alert Quiet Period is disabled: if the alert is continually triggered, the system sends the alert notification every minute. |
|Alert Severity|Valid values include Warn, Error, and Fatal.|-|
|Notification Time|The time when the alert was sent. No alert notification is sent out of this time period, but alert events are recorded.|For more information about viewing alert event records, see . .|
|Notification Content|The custom content of the alert.|You can edit the default template. In the template, the four variables, $AlertName, $AlertFilter, $AlertTime, and $AlertContent, are preset. \(Other preset variables are not supported currently.\) The rest of the content can be customized.|

## Description of complex general fields: period-on-period and period-for-period

-   Minute-on-minute comparison: Assume that β is the data \(optionally average, sum, maximum, or minimum\) in the last N minutes, and α is the data generated between the Nth and 2Nth minute. The minute-on-minute comparison is the percentage increase or decrease when β is compared with α.
-   Minute-for-minute hourly comparison: Assume that β is the data \(optionally average, sum, maximum or minimum\) in the last N minutes, and α is the data generated during the last N minutes in the last hour. The minute-for-minute hourly comparison is the percentage increase or decrease when β is compared with α.
-   Minute-for-minute daily comparison: Assume that β is the data \(optionally average, sum, maximum or minimum\) in the last N minutes, and α is the data generated during the last N minutes at the same time yesterday. The minute-for-minute daily comparison is the percentage increase or decrease when β is compared with α.

## Description of complex general fields: Alert Data Revision Strategy

You can select "Zero fill", "One fill", or "Zero fill null" \(default\). This feature is generally used to fix anomalies in data, including no data, abnormal composite metrics, and abnormal period-on-period and period-for-period comparisons.

-   Zero fill: fixes the value checked to 0.
-   One fill: fixes the value checked to 1.
-   Zero fill null: does not trigger the alert.

Scenarios:

-   Anomaly 1: no data

    User A wants to use the alert feature to monitor the page views. When User A creates the alert, User A selects Browser Monitoring Alert. User A sets the alert rule: N is 5 and the sum of the page views is at most 10. If the page is not accessed, no data is reported and no alert is sent. To solve this problem, you can select "Zero fill" as the alert data revision policy. If you do not receive any data, it considered that zero data is received. This meets the alert rule and an alert is sent.

-   Anomaly 2: abnormal composite metrics

    User B wants to use the alert feature to monitor the real-time unit price of a product. When User B creates the alert, User B selects Custom Monitoring Alert. User B sets the dataset of variable a to the current total price, and the dataset of variable b to the current total items. User B also sets the alert rule that N is 3 and the minimum value of current total price divided by current total items is at most 10. If the current total of items is 0, the value of the composite metric, current total price divided by current total items, does not exist. No alert is sent. To solve this problem, you can select "Zero fill" as the alert data revision policy. The value of the composite metric, current total price divided by current total items, is now considered to be 0. This meets the alert rule and an alert is sent.

-   Anomaly 3: abnormal period-on-period and period-for-period comparisons

    User C wants to use the alert function to monitor the CPU utilization of the node machine. When User C creates the alert, User C selects Application Monitoring Alert, and sets the alert rule: N is 3 and the average user CPU utilization of the node machine decreases by 100% compared with the previous monitoring period. If the CPU of the user fails to work in the last N minutes, α cannot be obtained. This means the period-on-period result does not exist. No alert is sent. To solve this problem, you can select the alert data revision strategy as "One fill", and consider the period-on-period comparison result as a decrease of 100%. This meets the alert rule and an alert is sent.


You can query and delete alert records in alert management.

