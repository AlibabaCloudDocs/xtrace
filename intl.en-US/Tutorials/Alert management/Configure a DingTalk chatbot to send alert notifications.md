# Configure a DingTalk chatbot to send alert notifications

After you configure a DingTalk chatbot to send alert notifications, you can specify DingTalk groups to receive alert notifications. This topic describes how to configure a DingTalk chatbot to send alert notifications.

## Add a custom DingTalk chatbot and obtain the webhook URL

Perform the following steps to add a custom DingTalk chatbot and obtain the webhook URL:

1.  Run the DingTalk client on a PC, go to the DingTalk group to which you want to add an alert chatbot, and then click the Group Settings icon in the upper-right corner.

2.  In the **Group Settings** panel, click **Group Assistant**.

3.  In the **Group Assistant** panel, click **Add Robot**.

4.  In the **ChatBot** dialog box, click the **+** icon in the **Add Robot** card. Then, click **Custom**.

    ![Add Robot](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3124462261/p43302.png)

5.  In the **Robot details** dialog box, click **Add**.

6.  In the Add Robot dialog box, edit the profile picture, enter a chatbot name, and then select at least one of the options in the **Security Settings** section. Read the DingTalk Custom Robot Service Terms of Service and select **I have read and accepted DingTalk Custom Robot Service Terms of Service**. Click **Finished**.

    ![Add Robot](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2467758061/p43303.png)

7.  In the Add Robot dialog box, click Copy to save the webhook URL of the chatbot and then click **Finished**.

    ![Add Robot](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2467758061/p43304.png)


## Create a contact

You can create a contact or add the webhook URL to the existing contact information. In this example, a contact is created.

1.  Log on [Tracing Analysis console](https://tracing-sg.console.aliyun.com/).

2.  In the left-side navigation pane, choose **Alert History** \> **Alert Contacts**.

3.  On the Contacts tab, click **Create Contact** in the upper-right corner.

4.  In the **Create Contact** dialog box, enter the webhook URL obtained in [Add a custom DingTalk chatbot and obtain the webhook URL](#section_3tb_85o_qd0). Then, click **OK**.


## Create a contact group

You must create a contact group, because notifications for alerts triggered by an alert rule can be sent to alert contact groups instead of alert contacts.

1.  Log on [Tracing Analysis console](https://tracing-sg.console.aliyun.com/).

2.  In the left-side navigation pane, choose **Alert History** \> **Alert Contacts**.

3.  On the Contact Groups tab, click **New Contact Group** in the upper-right corner.

4.  In the Create Contact Group dialog box, enter a name in the **Group Name** field, search for the contact created in [Create a contact](#section_tvi_of1_0xy), add the contact to the contact list, and then click **OK**.


## Create an alert rule

If no alert rule is created, create an alert rule first. You can configure DingTalk chatbots to send notifications for frontend monitoring, application monitoring, custom monitoring, and Prometheus monitoring alerts. In this example, an application monitoring alert rule is created.

1.  Log on [Tracing Analysis console](https://tracing-sg.console.aliyun.com/).

2.  In the left-side navigation pane, choose **Alert History** \> **Alert Rules and History**.

3.  On the Alarm Policies page, click **Create Alarm** in the upper-right corner.

4.  In the **Create Alarm** dialog box, set the parameters and click **Save**.

    1.  Enter a name in the **Alarm Name** field. For example, you can enter Alert on JVM-GC times in period-over-period comparison.

    2.  From the **Application Site** drop-down list, select an application.

    3.  From the **Type** drop-down list, select the type of metric that you want to monitor. For example, you can select **JVM\_Monitoring**.

    4.  Set the **Dimension** parameter to **Traverse**.

    5.  Set an alert rule.

        1.  Select **Meet All of the Following Criteria** for the Alarm Rules parameter.
        2.  Edit the alert rule. For example, an alert is triggered when the value of N is 5 and the average value of JVM\_FullGC increases by 100% compared with that in the previous hour.

            **Note:** To add another alert rule, click the **+** icon next to **Last N Minutes**.

    6.  Select **Ding Ding Robot** for the **Notification Mode** parameter.

    7.  In the Notification Receiver section, add the contact group created in [Create a contact group](#section_1kq_68o_xg6) to the receiver list. In the **Contact Groups** list, click the name of the contact group. If the contact group appears in the **Selected Groups** list, the setting is successful.


## Edit an alert rule

Perform the following steps to edit an alert rule. In this example, an application monitoring alert rule is edited.

1.  Log on [Tracing Analysis console](https://tracing-sg.console.aliyun.com/).

2.  In the left-side navigation pane, choose **Alert History** \> **Alert Rules and History**.

3.  On the **Alarm Policies** page, enter the alert rule name in the search box and click **Search**.

    **Note:** You can enter part of an alert rule name in the search box to perform a fuzzy search.

4.  Find the alert rule that you want to edit and click **Edit** in the **Actions** column.

5.  In the Edit Alarm dialog box, modify related parameters and click **Save**.

    1.  Select **Ding Ding Robot** for the Notification Mode parameter.

    2.  In the Notification Receiver section, add the contact group created in [Create a contact group](#section_1kq_68o_xg6) to the receiver list. In the **Contact Groups** list, click the name of the contact group. If the contact group appears in the **Selected Groups** list, the setting is successful.


A DingTalk chatbot is configured to send alert notifications. When an alert is triggered, you will receive an alert notification from the specified DingTalk group. The following figure shows an alert notification.

