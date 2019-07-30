# 获取客户端真实IP最佳实践 {#concept_1443500 .concept}

本文介绍了将业务接入游戏盾防护后，如何获取客户端的真实IP。

## 背景介绍 {#section_wd8_scd_jy1 .section}

游戏盾是FullNat代理模式，经过游戏盾的请求，其客户端的IP地址会变成游戏盾的IP地址。针对有获取客户端真实IP需求的游戏盾用户，本文提供了获取客户端IP的解决方案。

## 实现原理 {#section_j2c_0ut_j0x .section}

游戏盾通过TCP协议的Option字段来携带和传递客户端的IP信息，俗称TOA。由于游戏盾的TOA协议格式属于游戏盾专有，服务器需要集成游戏盾提供的TOA模块才能获取客户端IP信息。游戏盾提供内核级、应用级、代码集成等多种方式来集成TOA模块，您可以根据实际情况选择最简单的方式进行集成。

## 选择集成方式 {#section_vyc_n2e_1jq .section}

|场景|支持的架构|不支持的架构|
|--|-----|------|
|四层TCP协议获取客户端真实IP| -   游戏盾-\>阿里云服务器/非阿里云服务器
-   游戏盾-\>阿里云SLB-4层转发-\>阿里云服务器

 |游戏盾-\>非阿里云SLB-4层转发-\>服务器|
|七层HTTP/HTTPS协议获取客户端真实IP| -   游戏盾-\>阿里云服务器/非阿里云服务器
-   游戏盾-\>阿里云SLB-4层转发-\>阿里云服务器

 | -   游戏盾-\>阿里云SLB-7层转发\(包含WAF/高防IP的7层接入\)-\>阿里云服务器
-   游戏盾-\>非阿里云SLB-4/7层转发-\>服务器

 |

**说明：** 游戏盾是四层转发，不托管HTTPS证书，无法查看您HTTPS数据流中的数据信息。所以，七层协议获取客户端真实IP不是通过XFF字段获取的，是通过在服务端的TOA模块适配直接获取客户端真实IP。

|模块|Linux|Windows|
|--|-----|-------|
|内核TOA模块（无需修改代码）|部分支持|不支持|
|应用层Hook-TOA模块（无需修改代码）|支持|部分支持|
|应用层代码集成式模块（需要做代码集成）|支持|支持|

最佳实践说明

-   Linux

    Linux服务器环境且是Centos，内核版本在游戏盾支持的列表中，优先选择安装内核级TOA模块。

    如果内核模块不在支持列表中，则集成应用层Hook-Toa模块。

    如果无法使用应用层Hook-Toa模块，再通过修改应用内的代码来集成TOA。

-   Windows

    Windows服务提供部分程序的应用层Hook-TOA模块，优先选择集成应用层Hook-Toa模块。

    如果无法通过Hook方式集成，则通过在应用内修改代码来集成TOA。


## 集成内核级TOA模块 {#section_oc1_vqf_92q .section}

**说明：** 推荐您使用Centos 7.2系统，此版本稳定性最高。如果您对操作系统没有依赖，建议您切换成推荐系统，降低获取真实IP的复杂性。

Linux内核调节

1.  确认内核模式完整性和内核版本。内核版本兼容：2.6.32-642.13.1（具体见TOA内核名称）。

    补齐内核依赖命令

    ``` {#codeblock_25g_cuy_79l}
    modprobe nf_conntrack_ipv4
    modprobe nf_defrag_ipv4
    modprobe xt_state
    modprobe nf_conntrack
    modprobe iptable_filter
    modprobe ip_tables
    ```

2.  修改配置文件。

    ``` {#codeblock_sa4_qbv_4dd}
    vi /etc/sysctl.conf
    net.ipv4.tcp_fin_timeout = 10
    net.ipv4.tcp_tw_recycle= 1
    net.ipv4.tcp_tw_reuse = 1
    net.ipv4.tcp_keepalive_time = 15
    net.ipv4.tcp_max_tw_buckets = 1048576
    net.nf_conntrack_max=655360
    ```

    **说明：** 最后一条为新增。

3.  配置生效。

    ``` {#codeblock_m1q_r3j_ice}
    sysctl -p
    ```

4.  安装模块。

    ``` {#codeblock_om8_gbh_gdm}
     insmod   XXXX.ko     （修改成模块名称）
    ```

    参考命令如下：

    -   查看是否安装模块

        ``` {#codeblock_oqx_08s_69c}
        lsmod | grep toa
        ```

    -   删除模块

        ``` {#codeblock_cxe_0em_6m9}
         rmmod ali_flex_toa
        ```

    -   查看模块信息

        ``` {#codeblock_zgp_fc2_by4}
        modinfo  ali_flex_toa.ko
        ```

    -   查看运行状态

        ``` {#codeblock_wbx_vto_5vn}
        cat /proc/aliflex/toa
        ```


## 集成应用层Hook-TOA模块 {#section_guz_rzb_fg8 .section}

参照以下步骤，集成应用层Hook-TOA模块：

1.  执行install.sh，安装toa\_server相关服务。
2.  携带preload.so启动服务器。假设服务器名为nginx ，则启动命令如下：

    ``` {#codeblock_nqo_sti_xvz}
    LD_PRELOAD=./preload.so ./nginx
    ```

    **说明：** 您需要根据实际情况找到您程序的启动地方，增加如上命令参数进行服务的启动。


## 集成代码层TOA {#section_bfl_3w7_u6m .section}

具体请参见TOA压缩包内的指导文档进行操作。详细情况可以咨询游戏盾服务团队。

