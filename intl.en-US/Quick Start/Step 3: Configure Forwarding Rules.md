# Step 3: Configure Forwarding Rules {#task_g4h_d32_xdb .task}

1.   Log on to the Alibaba Cloud Security Game Shield Management console, go to **Business Management**, and select **Game Name**. 
2.   Locate to the business that you added, and click **Forwarding Rule Management**. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13508/15477777523442_en-US.png)

3.   Click **Rule Management** and select **Add a rule**. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13508/15477777523443_en-US.png)

4.   In the Add Rule dialog box, fill in the forwarding rule, and click **OK**. 

    **Note:** If a port corresponds to multiple origin site IPs, the IPs can be entered to the **Origin IPs** field, separated by commas \(up to 20 IPs\). After multiple origins are configured, load balancing is achieved automatically through polling.

    **Note:** Multiple rules for a same port are not allowed, for example, “8000, 8000, 6.6.6.6” and “8000, 8000, 1.1.1.1” cannot appear simultaneously. Under such circumstance, you must create a new business.

    **Note:** Ports below 1024, except port 80 and port 433, are system reserved ports. If you want to add such ports, please contact the Game Shield team.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13508/15477777523446_en-US.png)


