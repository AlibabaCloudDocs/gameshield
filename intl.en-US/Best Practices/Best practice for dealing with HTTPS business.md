# Best practice for dealing with HTTPS business {#concept_1443498 .concept}

This topic describes how to use GameShield to deal with HTTPS business.

## Background {#section_ska_a7c_pb4 .section}

It requires several complex steps to deal with HTTPS business by using GameShield. You need to take some steps to tackle HTTPS compatibility issues, such as certificate verification, cookie insertion, and Server Name Indication \(SNI\).

This topic provides you a solution in the scenario where you want to use GameShield to deal with HTTPS business.

## Solution {#section_45r_iof_5bh .section}

To solve the compatibility issue of certificate verification, GameShield introduces a domain name \(www-yxd.test.com\) that resolves to 127.0.0.1. In comparison to dealing with TCP business, you need to use domain names and take a stitching step for domain name resolution.

Step 1. https://www.test.com

Step 2. https://127.0.0.1:28291 \(Certificate verification may fail.\)

Step 3. https://www-yxd.test.com:28291 \(GameShield introduces a domain name that resolves to 127.0.0.1. You must configure the server to listen to the domain name.\)

The procedure for TCP business is also provided for comparison.

Step 1. tcp://1.1.1.1:8001

Step 2. tcp://127.0.0.1:21781

To use this solution, you must configure the server to listen to the domain name that is introduced \(www-yxd.test.com\).

However, this solution has the following problems:

GameShield is designed to eliminate the need for DNS and avoid business unavailability caused by DNS hijacking. However, domain name resolution is introduced in this solution, which increases the risk of hijacking. Test results show that DNS servers of some Internet service providers \(ISPs\) do not respond to the resolution from a domain name to 127.0.0.1, thereby affecting business.

## Use a custom DNS server {#section_wwx_9u0_u80 .section}

To solve the potential problem of the domain name that resolves to 127.0.0.1, you can check whether your network protocols support resolution by custom DNS servers. If yes, we recommend that you use a custom server to resolve the domain name \(www-yxd.test.com\) instead of a DNS server provided by an ISP. In this way, you can avoid DNS spoofing and hijacking.

For example, with the DNS interface in the OkHttp library, you can resolve the domain name used by GameShield through a custom DNS server.

In comparison to resolving the domain name to 127.0.0.1 by using a DNS server of an ISP, resolution by a custom DNS server is implemented based on a DNS interface. In this way, it helps prevent DNS hijacking. Resolution by a custom server is easy to implement and requires minimum code modification. It is well suited for many scenarios, such as HTTPS certificate verification, cookie insertion, and SNI.

This practice also applies to the scenario where the Retrofit and OkHttp libraries are used. After you have configured OkHttpClient, use it as the argument for `Retrofit.Builder::client(OkHttpClient)`.

As for other libraries, you can search for solutions on the Alibaba Cloud website.

