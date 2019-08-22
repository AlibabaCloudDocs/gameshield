# Best practice for obtaining the real IP address of a client {#concept_1443500 .concept}

This topic describes how to obtain the real IP address of a client after GameShield is activated for your business.

## Background {#section_wd8_scd_jy1 .section}

GameShield adopts full network address translation \(NAT\). After receiving a request from a client, GameShield replaces the IP address of the client with the IP address of GameShield. This topic provides you a solution in the scenario where you want to obtain the real IP address of the client.

## Principle {#section_j2c_0ut_j0x .section}

GameShield transfers the IP address of a client through the Option field of TCP and thus provides a module called TCP Option Adapter \(TOA\). The TOA module is only applicable to GameShield. To obtain the real IP address of the client, you need to integrate the TOA module with your origin servers. You can integrate the TOA module in the kernel, applications, or code. Select the easiest method based on your business requirements.

## Integration methods {#section_vyc_n2e_1jq .section}

|Scenario|Supported architecture|Unsupported architecture|
|--------|----------------------|------------------------|
|Obtain the real IP address of a client when the client uses TCP for transmission| -   GameShield -\> Alibaba Cloud servers or servers that are not provided by Alibaba Cloud
-   GameShield -\> Alibaba Cloud Layer-4 Server Load Balancer \(SLB\) -\> Alibaba Cloud servers

 |GameShield -\> Layer-4 server-side load balancers that are not provided by Alibaba Cloud -\> servers|
|Obtain the real IP address of a client when the client uses HTTP/HTTPS for transmission| -   GameShield -\> Alibaba Cloud servers or servers that are not provided by Alibaba Cloud
-   GameShield -\> Alibaba Cloud Layer-4 SLB -\> Alibaba Cloud servers

 | -   GameShield -\> Alibaba Cloud Layer-7 SLB \(including WAF/Anti-DDoS Pro\) -\> Alibaba Cloud servers
-   GameShield -\> Layer-4 or Layer-7 server-side load balancers that are not provided by Alibaba Cloud -\> servers

 |

**Note:** GameShield is based on Layer-4 server-side load balancers. It does not manage HTTPS certificates, and cannot read data from HTTPS data streams. The real IP address of a client is not obtained from the X-Forwarded-For \(XFF\) HTTP header field. Instead, it is obtained by using the TOA module of the server.

|Module|Linux|Windows|
|------|-----|-------|
|TOA module in the kernel \(Code modification is not required.\)|Partially supported|Not supported|
|Hook-TOA module in applications \(Code modification is not required.\)|Supported|Partially supported|
|TOA module in code \(Code modification is required.\)|Supported|Supported|

Description

-   Linux

    If your origin servers are running CentOS and GameShield supports your kernel version, we recommend that you install the TOA module in the kernel.

    If your kernel version is not supported, integrate the Hook-TOA module in applications.

    If the Hook-TOA module in applications is not applicable, modify code to integrate the TOA module.

-   Windows

    Some Windows applications support integration with the Hook-TOA module. We recommend that you integrate the Hook-TOA module in the applications.

    If you cannot integrate the Hook-TOA module, modify code to integrate the TOA module.


## Integrate the TOA module in the kernel {#section_oc1_vqf_92q .section}

**Note:** We recommend that you use CentOS 7.2 because it is the most stable OS. If your origin servers are not running CentOS 7.2 and switching the OS does not affect your business, we recommend that you switch to CentOS 7.2. This helps you gain the real client IP address in an easier way.

To install the TOA module in a Linux kernel, perform the following steps:

1.  Verify that the kernel version is supported and the kernel has all required modules. GameShield supports the Linux kernel version of 2.6.32-642.13.1. You need to check whether the TOA module supports your kernel version.

    If you need to install a required kernel module, run the corresponding command.

    ``` {#codeblock_25g_cuy_79l}
    modprobe nf_conntrack_ipv4
    modprobe nf_defrag_ipv4
    modprobe xt_state
    modprobe nf_conntrack
    modprobe iptable_filter
    modprobe ip_tables
    ```

2.  Run the vi /etc/sysctl.conf command to edit the sysctl.conf file and include the following options in the file.

    ``` {#codeblock_sa4_qbv_4dd}
    vi /etc/sysctl.conf
    net.ipv4.tcp_fin_timeout = 10
    net.ipv4.tcp_tw_recycle= 1
    net.ipv4.tcp_tw_reuse = 1
    net.ipv4.tcp_keepalive_time = 15
    net.ipv4.tcp_max_tw_buckets = 1048576
    net.nf_conntrack_max=655360
    ```

    **Note:** The last option in this configuration text does not exist. You must add this option.

3.  Run the following command to allow the changes to take affect.

    ``` {#codeblock_m1q_r3j_ice}
    sysctl -p
    ```

4.  Run the following command to install the module.

    ``` {#codeblock_om8_gbh_gdm}
     insmod   XXXX.ko (Replace XXXX with a module name.)
    ```

    You can run the following commands to perform other operations on the module.

    -   Check whether the module is installed.

        ``` {#codeblock_oqx_08s_69c}
        lsmod | grep toa
        ```

    -   Delete the module.

        ``` {#codeblock_cxe_0em_6m9}
         rmmod ali_flex_toa
        ```

    -   View the module information.

        ``` {#codeblock_zgp_fc2_by4}
        modinfo  ali_flex_toa.ko
        ```

    -   View the operating status of the module.

        ``` {#codeblock_wbx_vto_5vn}
        cat /proc/aliflex/toa
        ```


## Integrate the Hook-TOA module in applications {#section_guz_rzb_fg8 .section}

Perform the following steps to integrate the Hook-TOA module:

1.  Run the install.sh command to install services related to toa-server.
2.  Include the preload.so parameter in the command to start the server. If the server name is nginx, run the following command to start the server.

    ``` {#codeblock_nqo_sti_xvz}
    LD_PRELOAD=./preload.so ./nginx
    ```

    **Note:** You must find the entry point of your program and include the parameter in the preceding command to start the service.


## Integrate the TOA module in code {#section_bfl_3w7_u6m .section}

For more information, see the instructions in the TOA archive. You can also consult the GameShield service team for more details.

