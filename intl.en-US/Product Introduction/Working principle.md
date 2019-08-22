# Working principle {#concept_1563660 .concept}

An elastic security network is the key to GameShield, which defends against DDoS attacks through edge devices.

## Overview {#section_1rn_x0h_a2f .section}

GameShield offers an elastic security network that can only be accessed by using SDK and prevents DDoS attacks and HTTP flood attacks. A client can access the elastic security network of GameShield through a local proxy server. This allows a gamer \(Token\) to access the port \(Dport\) with the origin IP address \(Dip\) through a node group \(GroupName\).

SDK code sample: `YunCeng.getProxyTcpByDomain(Token, GroupName, Dip, Dport)`

|Parameter|Description|
|---------|-----------|
|Token|The ID of a gamer. It is used to identify the malicious gamers or hackers who initiate DDoS attacks. Default value: Default.|
|GroupName|The node group ID of a game business. Example: access.v812vCOE21.ftnormal01al.com. In the GameShield console, after you add a game and a business, you must configure node groups. For each node group, you need to determine the number of nodes based on the number of simultaneous gamers. You can specify multiple node groups for each game.|
|Dip|The IP address of an origin server. You must configure it in GameShield.|
|Dport|The port of the server. You do not need to configure it in GameShield. You can pass it to GameShield based on your business requirements.|

## Endpoints for different protocols {#section_sc0_h2x_whj .section}

You can use a client SDK to deploy a local proxy server on the client so that the proxy server can map any server-side IP addresses and ports to local services. In this way, the proxy server forwards all related data flows between the client and the server and performs routing and data encryption. This architecture provides strong protection for your business, such as data encryption and defense against DDoS attacks and HTTP flood attacks.

The following table describes the endpoints for different protocols.

|Protocol|Endpoint used for direct access|Endpoint used for proxy-based access|
|--------|-------------------------------|------------------------------------|
|TCP|tcp://1.1.1.1:8080|tcp://127.0.0.1:8729 \(random port\)|
|HTTP|http://www.test.com|http://127.0.0.1:2892 \(random port\)|
|HTTPS|https://www.test.com|https://127.0.0.1:2892 \(Certificate verification may fail.\) -\> https://www-yxd.test.com:2892 **Note:** You can use a domain name such as www-yxd.test.com to solve the issues raised by hostname mismatch and HTTPS certificate verification failures. For more information, see [Best practice for dealing with HTTPS business](../../../../reseller.en-US/Best Practices/Best practice for dealing with HTTPS business.md#).

 |
|WebSocket|ws://1.1.1.1:88|ws://127.0.0.1:2891 \(random port\)|

