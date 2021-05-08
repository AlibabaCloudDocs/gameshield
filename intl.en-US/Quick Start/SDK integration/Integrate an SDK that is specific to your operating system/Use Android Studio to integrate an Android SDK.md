# Use Android Studio to integrate an Android SDK

This topic describes how to use Android Studio to integrate an Android SDK.

-   You can obtain an SDK and AccessKey pair for Android from the GameShield console. For more information, see [Obtain an SDK package and AccessKey pair](/intl.en-US/Quick Start/SDK integration/Obtain an SDK package and AccessKey pair.md).
-   You can obtain the following information from the GameShield console.
    -   GroupName: indicates a node group ID. Log on to the GameShield console, open the game management page, and view the target node group ID on the Basic Settings tab.

        ![Basic settings and a node group ID](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9762377951/p64189.png)

    -   Protection Target ID. Log on to the GameShield console, open the Games page, and view a protection target ID on the Protection Target Settings tab.

        ![Protection target settings and a protection target ID](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9762377951/p64191.png)


Key methods include initEx and getProxyTcpDomain. For more information, see [Introduction to core methods](/intl.en-US/Quick Start/SDK integration/Introduction to core methods.md).

You can contact GameShield Technical Support to obtain demo applications.

1.  Open Android Studio.

2.  Create a project and use the default settings to complete creation. Name the project yxd\_sdk\_test.

    The following figure shows the structure of the new project directory.

    ![Create a project directory](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5762377951/p64149.png)

    **Note:** Before proceeding to subsequent operations, you must make sure that the new project is working as expected.

    ![The new project is working as expected.](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5762377951/p64151.png)

3.  Add dependencies

    1.  Copy the yunceng.jar file from the Android SDK to the libs directory. You can drag and drop the file to the libs directory.

        ![Copy yunceng.jar](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5762377951/p64153.png)

    2.  Open Android Studio, choose **File** \> **Project Structure**, click **app**, and click the Dependencies tab.

        ![dependencies](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5762377951/p64154.png)

    3.  Click the plus sign \(**+**\), select **jardependency**, and specify **yunceng.jar**.

        ![Add yunceng.jar](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5762377951/p64156.png)

    4.  Click **OK** to complete the configuration.

4.  Add the .so file. Under the **src** \> **main** directory, create a subdirectory named jniLibs, and copy the libyunceng.so file to the jniLibs directory.

    ![Copy libyunceng.so](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5762377951/p64157.png)

5.  Configure access permissions. Open the AndroidManifest.xml file, and add the following statement to the file as shown in the following figure.

    ```
    <uses-permission android:name="android.permission.INTERNET" />
    ```

    ![Access permissions](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5762377951/p64158.png)

6.  Add the following code to obtain the IP address of a protection target. The following shows the sample code.

    ```
    // Initialize the SDK. One successful initialization for the SDK is required.
    int len = 0;
    ret = YunCeng.initEx(getResources().getString(R.string.appkey), "token");if (0 ! = ret) {
             msg_show.setText("sdk init failed " + Integer.toString(ret));
             return;
    }msg_show.setText("sdk init success ");
    return;
    
    // Use core methods to obtain the IP address of a protection target
    ret = YunCeng.getProxyTcpByDomain("Player ID","GroupName", "Protection target ID","Port number of the origin server", ip, port);
    if (0 == ret) {
            msg_show.setText("get next ip success: " + Integer.toString(ret) + "\nip : " + ip + "port :
    " + port);
    } else {
            msg_show.setText("get next ip failed. : " + Integer.toString(ret));
    }
    ```

7.  Configure ProGuard. If you use ProGuard to perform obfuscation, you must add the following statement to the ProGuard configuration file.

    ```
    -keep class com.aliyun.security.yunceng. ** {*;}
    ```


After you add your application, you can use the SDK to retrieve the IP address and the port that are mapped by GameShield for your application. The method that GameShield uses to map an IP address and a port for your application varies based on your service type. For details about the methods, see the following topics:

-   [TCP applications](/intl.en-US/Quick Start/SDK integration/Use SDKs to integrate applications into GameShield/TCP applications.md)
-   [HTTP and HTTPS applications](/intl.en-US/Quick Start/SDK integration/Use SDKs to integrate applications into GameShield/HTTP and HTTPS applications.md)
-   [HTTP and HTTPS applications with the Browser/Server \(B/S\) architecture](/intl.en-US/Quick Start/SDK integration/Use SDKs to integrate applications into GameShield/HTTP and HTTPS applications with the Browser/Server (B/S) architecture.md)

