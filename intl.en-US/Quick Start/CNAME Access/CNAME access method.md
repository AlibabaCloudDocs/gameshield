# CNAME access method {#concept_crt_ys2_xdb .concept}

This article introduces the CNAME access method of Game Shield.

## CNAME Access Description {#section_wq1_zs2_xdb .section}

Advantages: easy and fast access, and smart resolution and automatic scheduling ability of black hole routing.

Disadvantages: slow switching, and direct exposure of the Game Shield node.

**Note:** The CNAME access is recommended only when access to the SDK is unavailable. Only with access through the SDK can you enjoy the full protection benefits of Game Shield.

## How do you configure CNAME access? {#section_xq1_zs2_xdb .section}

The name of the node group is the CNAME where the Game Shield access uses.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13516/15477778523482_en-US.png)

## What is CNAME smart scheduling logic? {#section_zq1_zs2_xdb .section}

Default line: The BGP line has a higher priority than a single line. If there is a BGP line, only the BGP line is revealed \(up to 2 IPs are revealed\). If there is no BGP line, a single line is returned \(up to 6 IPs are revealed\).

China Telecom line: A China Telecom line gets allocation priority. If there is no China Telecom line, no allocation is made \(up to 2 IPs are revealed\).

China Unicom line: A China Unicom line gets allocation priority. If there is no China Unicom line, no allocation is made \(up to 2 IPs are revealed\).

China Mobile line: A China Mobile line gets allocation priority. If there is no China Mobile line, no allocation is made \(up to 2 IPs are revealed\).

**Note:** 

-   After some nodes are routed to black holes: Automatically remove the nodes in black holes and automatically adjust DNS resolution according to the preceding logic.
-   After all nodes are routed to black holes: The resolution state is equivalent to that when all nodes survive.

## How do you verify the effectiveness of smart resolution? {#section_ar1_zs2_xdb .section}

You can use [https: // www. 17ce.com/](https://www.17ce.com/) to verify the smart resolution result of CNAME.

## Is smart resolution automatically enabled for old users? {#section_br1_zs2_xdb .section}

Only the node groups created after March 21 have the smart resolution function automatically enabled. The older groups must undergo events, such as enabling scheduling, disabling scheduling, adding nodes, deleting nodes, and black hole attacks, to trigger their own DNS resolution logic.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13516/15477778523483_en-US.png)

