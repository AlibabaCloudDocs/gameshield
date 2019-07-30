# HTTPS业务接入最佳实践 {#concept_1443498 .concept}

本文介绍了在游戏盾中接入HTTPS业务时需要执行的特殊处理。

## 背景介绍 {#section_ska_a7c_pb4 .section}

HTTPS业务的接入是游戏盾接入中最复杂的部分，为了兼容HTTPS协议中的证书校验、Cookie植入、SNI等问题，HTTPS的业务接入需要做一些特殊的处理。

针对有HTTPS业务接入需求的游戏盾用户，建议您参见本文提供的解决方案进行接入。

## 游戏盾解决方案 {#section_45r_iof_5bh .section}

游戏盾引入了一个固定解析到127.0.0.1的域名（www-yxd.test.com），以解决证书校验域名问题。对比TCP协议接入，该方式多了一个域名和拼接步骤，具体流程如下：

Step.1 https://www.test.com

Step.2 https://127.0.0.1:28291（无法通过证书校验）

Step.3 https://www-yxd.test.com:28291（引入一个解析到127.0.0.1的域名，需要在服务端配置此域名监听）

对比TCP业务接入流程：

Step.1 tcp://1.1.1.1:8001

Step.2 tcp://127.0.0.1:21781

为适应游戏盾解决方案，您需要在服务端配置所引入域名（www-yxd.test.com）的监听。

该方案可能存在以下问题：

游戏盾的本质是为了去DNS化，解决DNS被各种劫持导致的业务不可用问题。但是，HTTPS方案中又引入了一个DNS，这样会增加业务被劫持的风险。实际测试中也发现解析到127.0.0.1的域名比较特殊，容易被一些ISP的LocalDNS服务器作为异常域名解析而不予响应，影响正常业务的访问。

## 固化解析方案 {#section_wwx_9u0_u80 .section}

针对游戏盾解析方案可能存在的问题，您可以查看您使用的网络协议库，是否支持自定义DNS解析。可以的话，建议您将HTTPS业务使用的域名（www-yxd.test.com）的解析结果（127.0.0.1）固化到本地，不通过LocalDns去解析，从而彻底解决DNS污染和劫持问题。

例如，OkHttp库暴露了一个DNS服务接口，我们可以使游戏盾使用的域名不经过LocalDns服务，直接自定义DNS解析结果。

相比于暴露一个解析到127.0.0.1的域名的方案，该方案通过DNS接口即可实现DNS本地化，彻底解决DNS劫持问题。该方案不仅实现方式最简单、改动量最小，而且通用性也最强，能够完美适应HTTPS证书校验、Cookie、SNI等场景。

本实践对于Retrofit+OkHttp同样适用，将配置好的OkHttpClient作为`Retrofit.Builder::client(OkHttpClient)`参数传入即可。

其他网络库建议您自行查找文档，寻找对应解决方案。相关解决方案文档介绍：[HttpDns+OkHttp最佳实践](https://helpcdn.aliyun.com/document_detail/52008.html)。

