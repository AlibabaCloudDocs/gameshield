# SDK接入重点说明 {#concept_x4q_ms2_xdb .concept}

## 一、获取游戏盾节点IP和端口 {#section_pq5_ns2_xdb .section}

客户端需要尽可能早的调用SDK的初始化接口，建议放到所有业务逻辑最前端。

在访问原服务器前的流程变化如下：

-   原：客户端获取服务端IP和端口——\>连接服务端
-   新：客户端调用SDK获取游戏盾IP和端口——-\>连接游戏盾IP和端口——\>服务端

必须完整使用游戏盾返回的IP和端口，游戏盾返回的IP和端口实例如下：

-   正常模式： 116.211.171.31 10009
-   链路加密模式： 127.0.0.1 56382 （[免疫CC攻击的模式](../../../../../intl.zh-CN/用户指南/隧道加密模式.md#)）

正常模式返回的是公网IP和固定端口（同转发规则配置），链路加密模式返回的是本地local地址和随机端口。

## 二、客户端对掉线重连逻辑的优化 {#section_sq5_ns2_xdb .section}

客户端在心跳超时、连接超时通常需要立即进入掉线重连逻辑，对于超时时间的判断我们建议是3-5秒（建议参考一局游戏出牌的时间），重连次数超过3-5次或者60秒时在提示客户连接失败，即可能弱化掉线重连的用户感知。

**客户端和服务端通信的逻辑必须是客户端先发起数据请求后服务端再响应，否则会被游戏盾游戏安全网关集群作为空连接攻击拦截**

## 三、服务端的优化和改造 {#section_tq5_ns2_xdb .section}

必须对客户在线的状态做判断，设定老化时间（通常是15-30秒）。

必须对客户的重新登录做处理，新登录用户踢出旧登录状态用户（掉线重连可能会比老化时间快）。

服务端获取客户端真实IP，请参考[客户真实IP获取](../../../../../intl.zh-CN/用户指南/客户真实IP获取.md#)

## 四、IPV6支持 {#section_uq5_ns2_xdb .section}

游戏盾支持IPV6环境，在IOS审核时，游戏盾会自动判断网络环境启用IPV6的调度和转发环境。

## 五、游戏盾服务不可用时的降级处理 {#section_vq5_ns2_xdb .section}

客户端需要做游戏盾SDK调用返回Code为非0时做降级处理，通常做法是写死游戏盾的高防IP节点作为兜底逻辑，将高防IP+端口作为游戏盾服务不可用的兜底服务使用。

是否启用游戏盾也需要在服务端设置开关来控制台数据流的走向。

## 六、用户分组和游戏盾节点分组的分配建议 {#section_wq5_ns2_xdb .section}

我们建议您的服务端也需要对您的客户做分层处理，不同的用户对应不同的游戏盾分组，通常逻辑如下：

-   新玩家，注册时间小于10天
-   老玩家，注册时间大于10天，游戏时间=<10小时或者游戏局数小于30局
-   VIP玩家，游戏时间\>10小时，游戏局数大于30局，消费金额大于100元

以上逻辑仅供参考，您可以根据您的业务情况做修改，用户分组的逻辑建议定期更新。

**建议您的游戏也能能够做到将制定用户分配到指定游戏盾分组上，具备发现恶意用户后自行手工调度的逻辑。**

## 七、客户端正式上线前需要做的检查 {#section_yq5_ns2_xdb .section}

如下步骤必须检查以保障游戏盾接入后的防护效果。

1.  游戏盾启用和停用的开关测试。
2.  游戏盾分组内节点被黑洞后的调度逻辑和调度时间测试（联系游戏盾团队模拟攻击）。
3.  游戏盾SDK获取节点失败的调度逻辑测试（联系游戏盾团队模拟攻击）。
4.  游戏盾用户手工调度功能测试。（非强制、可通过游戏盾分组智能调度实现）
5.  游戏盾SDK集成后的兼容性测试（将APK、IOS包提供给游戏盾团队进行稳定性、兼容性测试）。

