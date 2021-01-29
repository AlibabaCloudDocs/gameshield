# Best practice for achieving fast data transmission

This topic describes how to design a fast transmission plan with GameShield to satisfy your business requirements.

## Background

GameShield provides nodes in some regions and allows the nodes to establish connections to origin servers and clients to achieve fast data transmission. For example, if your origin server is located in the China \(Shenzhen\) region, GameShield can establish connections starting from China \(Beijing\) to China \(Shenzhen\) for your gamers in northern China. In this way, these gamers can enjoy fast access to your origin server.

However, the routing algorithm of GameShield focuses on defending against attacks. It only selects the fastest transmission link from a client to the connected node instead of the entire transmission link.

If you need fast data transmission, we recommend that you design a transmission plan based on your own business requirements.

## Plan description

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/1148281/156704663753861_en-US.png)

-   Nodes in all supported regions can establish connections to clients. These nodes can be used only for your gaming service.
-   GameShield selects a node in a region to establish the optimal connection to the origin.
-   Two nodes within the border are connected over the Alibaba Cloud network while cross-border transmission between two nodes is through the CN2 network.
-   Although fast transmission plans do not affect the capability of GameShield against DDoS attacks and HTTP flood attacks, some users may experience poorer services while GameShield is in defense mode.

## Supported regions

Currently nodes are available in the following regions:

-   Regions where nodes can connect to clients: China \(Hangzhou\), China \(Beijing\), and China \(Shenzhen\) in what regions BGP is deployed, Wuhan where only China Telecom provides transmission links, and Shijiazhuang where only China Unicom provides transmission links.
-   Regions where nodes can connect to origin servers:
    -   Regions in Mainland China: China \(Hangzhou\), China \(Shanghai\), China \(Beijing\), and China \(Shenzhen\) in what regions BGP is deployed.
    -   Regions outside Mainland China: China \(Hong Kong\) and Singapore in what regions BGP is deployed.

## Procedure

1.  When initializing SDK, you need to obtain endpoints that are used for accessing nodes from clients based on different `GroupName` values, but the same origin IP address and port.

    The core interface is `YunCeng.getProxyTcpByDomain(Token, GroupName, Dip, Dport)` with multiple `GroupName` values input. With this interface, you can obtain the following endpoints:

    ```
    Link 1 from China (Hangzhou) to China (Hong Kong): https://yxd.example.com:54723
    Link 2 from China (Shenzhen) to China (Hong Kong): https://yxd.example.com:45712
    Link 3 from China (Beijing) to China (Hong Kong): https://yxd.example.com:56371
    Link 4 from a region where Alibaba Cloud Anti-DDoS Pro is activated to China (Hong Kong) with filing not required or other traditional transmission links: https://gf.example.com
    ```

    **Note:** The process of obtaining an endpoint is equivalent to that of domain name resolution, which does not affect your business.

2.  You can call the SpeedTest interface of the business SDK to test the delay of multiple transmission links over which clients can access origin servers. Then, you can compare the test results. For example, you can use `https://yxd.example.com:17281/speedtest` to complete a test. The result is shown as follows:

    ```
    {
     "baiduPingDelay": "533",
     "domainName": "https://yxd.example.com:51567",
     "domainNameDelays": [{
       "delay": 1990,
       "url": "https://yxd.example.com:51567"
     }, {
       "delay": 2174,
       "url": "https://yxd.example.com:37869"
     }, {
       "delay": 2369,
       "url": "https://yxd.example.com:38465"
     }, {
       "delay": 3196,
       "url": "https://yxd.example.com:42877"
     }],
     {
       "delay":23196,
       "url": "https://gf.example.com"
     }],
     "ipAddress": "113.210.179.96",
     "netWorkType": "4G",
     "operator": "",
     "phoneModel": "VKY-L29",
     "systemVersion": "9"
    }
    ```

    You can upload the result to Log Service for comparison and analysis.

3.  From the result, use the endpoint \(https://yxd.example.com:51567\) with the shortest delay \(1990\) to access your business.

You can also perform other tests based on your business requirements. For example,

-   enable or disable a transmission link,
-   specify a link for a group of gamers, and
-   allow gamers to select transmission links.
-   You can cache these test results to accelerate the startup of applications.

