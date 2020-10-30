# Step 2: Add a protection asset

After you create a game in GameShield, you must add a protection asset for the game.

A game is created. For more information, see [Step 1: Create a game](/intl.en-US/Quick Start/Step 1: Create a game.md).

1.  Log on to the [GameShield console](https://yundun.console.aliyun.com/?p=yxd).

2.  On the Homepage, find the required game, and click **Manage**.

    ![Manage](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1762377951/p3432.png)

3.  On the Protection Target Settings tab, choose **Protection Target Management** \> **Add Protection Target**.

    ![Add a protection asset](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1762377951/p3433.png)

4.  In the New Protection Target dialog box, configure the following parameters and click **OK**.

    |Parameter|Description|
    |---------|-----------|
    |**Protection Target ID**|The ID of a protection asset. Each ID is unique. The ID can be a maximum of 128 characters in length and contain letters, digits, and special characters.**Note:** The supported special characters are hyphens \(-\) and periods \(.\).

If you use SDKs to configure protection, the ID is required to call API operations. |
    |**Remark**|Optional. The remark about the protection asset. The remark can be a maximum of 128 characters in length and contain letters, digits, and special characters, such as hyphens \(-\) and periods \(.\).|
    |**Protocol**|**TCP** is selected by default and cannot be cleared.|
    |**Active Line IP**|The IP address of the active line for the game. You can enter a maximum of 20 IP addresses. Separate multiple IP addresses with commas \(,\).The IP addresses of the active line are always protected by GameShield and are not exposed to clients. Clients can access only the protection nodes that are provided by GameShield. This design provides unlimited protection against attacks for a game. |
    |**Standby Line IP**|The IP address of the standby line for the game. You can enter a maximum of 20 IP addresses. Separate multiple IP addresses with commas \(,\).The IP addresses of the standby line are returned and exposed to clients. These IP addresses are not protected. |

    ![Settings of a protection asset](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1762377951/p3437.png)


A protection asset is added. On the Protection Target Settings tab, you can view the new protection asset and click **Edit** or **Delete** to edit or delete it as required.

![A protection asset is added](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1762377951/p3438.png)

[Step 3: Add your application to GameShield](/intl.en-US/Quick Start/Step 3: Add your application to GameShield.md)

