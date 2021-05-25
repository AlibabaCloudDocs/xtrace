# Configure a DingTalk chatbot to send alert notifications

After you configure a DingTalk chatbot to send notifications, you can specify DingTalk groups to receive alert notifications. This topic describes how to configure a DingTalk chatbot to send notifications.

## Add a custom DingTalk chatbot and obtain the webhook URL

Perform the following steps to add a custom DingTalk chatbot and obtain the webhook URL:

1.  Run the DingTalk client on a PC, go to the DingTalk group to which you want to add an alert chatbot, and then click the Group Settings icon in the upper-right corner.

2.  In the **Group Settings** panel, click **Group Assistant**.

3.  In the **Group Assistant** panel, click **Add Robot**.

4.  In the **ChatBot** dialog box, click the **+** icon in the **Add Robot** card. Then, click **Custom**.

    ![Add Robot](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2467758061/p43302.png)

5.  In the **Robot details** dialog box, click **Add**.

6.  In the Add Robot dialog box, edit the profile picture, enter a chatbot name, and then select at least one of the options in the **Security Settings** section. Read and select **I have read and accepted DingTalk Custom Robot Service Terms of Service**. Click **Finished**.

    ![Add Robot](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2467758061/p43303.png)

7.  In the Add Robot dialog box, click Copy to save the webhook URL of the chatbot and then click **Finished**.

    ![Add Robot](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2467758061/p43304.png)


A DingTalk chatbot is configured to send notifications. When an alert is triggered, you will receive an alert notification from the specified DingTalk group. The following figure shows an alert notification.

