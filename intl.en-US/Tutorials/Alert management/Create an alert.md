# Create an alert

You can create alerts to set alert rules for specific monitored objects. If a rule is triggered, the system sends an alert notification to the specified alert group in the specified alerting mode. This allows you to take necessary actions to solve the problem.

-   Contacts are created. Only contact groups can be set as the notification receiver of an alert.

Default behaviors of alert notifications:

-   To prevent you from receiving a large number of alert notifications in a short period of time, the system sends only one message for the same alert within 24 hours.
-   If no alerts are generated for the same issue within 5 minutes, the system sends a recovery email to notify you that the alert has been cleared.
-   After a recovery email is sent, the alert status is reset. If this alert is triggered again, it is considered as a new alert.

An alert widget is essentially a data display method for datasets. When you create an alert widget, a dataset is created to store the underlying data of the alert widget.

**Note:** New alerts take effect within 10 minutes. The alert check may have a delay of 1 to 3 minutes.

## Create an alert

To create an alert for an application monitoring job on Java Virtual Machine-Garbage Collection \(JVM-GC\) times in period-over-period comparison, perform the following steps:

1.  Log on to the console. On the **Applications** page, click the application. In the left-side navigation pane, choose **Alarms** \> **Alarm Management**.

2.  On the Alarm Policies page, click **Create Alarm** in the upper-right corner.

3.  In the **Create Alarm** dialog box, set the parameters and click **Save**.

    1.  Set **Alarm Name**. For example, you can enter alert on JVM-GC times in period-over-period comparison.

    2.  From the **Application Site** drop-down list, select an application. From the **Application Group** drop-down list, select a group.

    3.  From the **Type** drop-down list, select the type of metric that you want to monitor. For example, you can select **JVM\_Monitoring**.

    4.  Set Dimension to **Traverse**.

    5.  Set Alert Rules.

        1.  Select **Meet All of the Following Criteria**.
        2.  Edit the alert rule. For example, an alert is triggered when the value of N is 5 and the average value of JVM\_FullGC increases by 100% compared with that in the previous hour.

            **Note:** To add another alert rule, click the plus sign \(**+**\) on the right of the first alert rule.

    6.  Set Notification Mode. For example, select Email.

    7.  Set Notification Receiver. In the **Contact Groups** list, click the name of a contact group. If the contact group appears in the **Selected Groups** list, the setting is successfully completed.

    ![Application Monitoring Alarm](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3608828061/p174561.png)


## Description of basic fields

The following table describes the basic fields in the **Create Alarm** dialog box.

![Create Alarm dialog box](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6128828061/p174562.png)

|Field|Description|Setting|
|-----|-----------|-------|
|Application Site|The monitoring job that is created.|Select a value from the drop-down list.|
|Dimension|The dimension for alert metrics \(datasets\). You can select None, =, or Traverse.|-   If Dimension is set to None, the alert content shows the sum of all values of this dimension.
-   If Dimension is set to =, you must enter the specific content.
-   If Dimension is set to Traverse, the alert content shows the dimension content that actually triggers the alert. |
|Last N Minutes|The system checks whether the data results in the last N minutes meet the trigger condition.|Value range: 1 to 60.|
|Notification Mode|Emails, SMS messages, DingTalk Chatbot, and Webhook are supported.|You can select multiple modes. For more information about how to configure a DingTalk chatbot to send alert notifications, see .|
|Alarm Quiet Period|You can enable or disable Alarm Quiet Period. By default, it is enabled.|-   If Alarm Quiet Period is enabled and an alert is continually triggered, the second alert notification is sent 24 hours after the first alert is triggered. When data is recovered, you receive a data recovery notification and the alert is cleared. If the data triggers the alert one more time, the alert notification is sent again.
-   If Alarm Quiet Period is disabled and an alert is continually triggered, the system sends the alert notification every minute. |
|Alarm Severity|Valid values include Warn, Error, and Fatal.|N/A|
|Notification Time|The time when the alert is sent. No alert notifications are sent outside of this time range, but alert events are recorded.|For more information about alert event records, see .|
|Notification Content|The custom content of the alert.|You can edit the default template. In the template, the four variables, $AlarmName, $AlarmFilter, $AlarmTime, and $AlarmContent, are preset. The rest of the content can be customized. Other preset variables are not supported.|

## Description of complex general fields: period-over-period comparison

-   N-minute-on-N-minute comparison: Assume that β is the data in the last N minutes, and α is the data generated between the Nth minute and the 2Nth minute. The N-minute-on-N-minute comparison is the percentage increase or decrease when β is compared with α. β can be an average, a sum, maximum, or minimum.
-   N-minute-on-N-minute hourly comparison: Assume that β is the data in the last N minutes, and α is the data generated during the last N minutes in the previous hour. The N-minute-on-N-minute hourly comparison is the percentage increase or decrease when β is compared with α. β can be an average, a sum, maximum, or minimum.
-   N-minute-on-N-minute daily comparison: Assume that β is the data in the last N minutes, and α is the data generated during the last N minutes at the same time on the previous day. The N-minute-on-N-minute daily comparison is the percentage increase or decrease when β is compared with α. β can be an average, a sum, maximum, or minimum.

## Description of complex general fields: Alarm Data Revision

You can set Alarm Data Revision to Set 0, Set 1, or Set Null \(Won't Trigger\). This feature is used to fix anomalies in data, including no data, abnormal composite metrics, and abnormal period-over-period comparisons.

-   Set 0: fixes the value that is checked to 0.
-   Set 1: fixes the value that is checked to 1.
-   Set Null \(Won't Trigger\): does not trigger the alert.

Scenarios:

-   Anomaly 1: no data

    User A wants to use the alerting feature to monitor page views. When User A creates an alert, User A selects Browser Monitoring Alarm. User A sets the alert rule: N is 5 and the sum of page views is at most 10. If the page is not accessed, no data is reported and no alert notification is sent. To solve this problem, you can select Set 0 for Alarm Data Revision. If you do not receive data, this is considered that zero data is received. This meets the alert rule and an alert is sent.

-   Anomaly 2: abnormal composite metrics

    User B wants to use the alerting feature to monitor the real-time unit price of a product. When User B creates an alert, User B selects Custom Monitoring Alarm. User B sets the dataset of variable A to the current total price, and the dataset of variable B to the current total number of items. User B also sets the alert rule: N is 3 and the minimum value of the current total price divided by current total items is at most 10. If the current total number of items is 0 and the value of the composite metric does not exist, no alert is sent. The value of the composite metric is the current total price divided by the current total number of items. To solve this problem, you can select Set 0 for Alarm Data Revision. The value of the composite metric, which is the current total price divided by the current total number of items, is considered to be 0. This meets the alert rule and an alert is sent.

-   Anomaly 3: abnormal period-over-period comparisons

    User C wants to use the alerting feature to monitor the CPU utilization of a node machine. When User C creates an alert, User C selects Application Monitoring Alarm, and sets the alert rule: N is 3 and the average CPU utilization of the node machine decreases by 100% compared with the previous monitoring period. If the CPU of the user fails to work in the last N minutes, α cannot be obtained. This means that the period-over-period comparison result does not exist. No alert is sent. To solve this problem, you can select Set 1 for Alarm Data Revision, and consider the period-over-period comparison result as a decrease of 100%. This meets the alert rule and an alert is sent.


You can query and delete alert records in alert management.

