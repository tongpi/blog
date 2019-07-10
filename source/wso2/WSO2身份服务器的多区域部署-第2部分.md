[TOC]

# WSO2身份服务器的多地域部署 - 第2部分

- 作者： Johann Nallathamby

- 2018年5月1日

  

在本文中，我们将继续讨论本系列第一[部分中](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.com.hk&sl=en&sp=nmt4&tl=zh-CN&u=https://wso2.com/library/articles/2018/04/multi-region-deployment-for-wso2-identity-server-part-1/&xid=17259,15700023,15700186,15700190,15700248,15700253&usg=ALkJrhgWugwAUYQAQ6SpkdrXi5N8LNPdXA) WSO2身份服务器（IS）的多地域部署用例。 如果您尚未阅读第一部分，我们建议您在继续阅读本文之前先阅读它。

我们在上一篇文章中为WSO2 IS确定了3种类型的多地域部署用例：

1. 跨所有地域复制身份和配置数据
2. 仅跨地域复制配置数据，但按地域分区身份数据
3. 根据所属的地域对所有数据进行分区

在第一部分中，我们深入研究了用例1。在本文中，我们将探讨用例2和3.与前一篇文章一样，我们打算不使用任何数据库复制技术。 这两个用例将确保多地域高可用性并将WSO2 IS的数据限制在它所属的地域，尽管我们必须在实现最短响应时间方面做出妥协。

## 用例2的多地域部署 - 仅跨地域复制配置数据，但按地域分区身份数据

在讨论此用例之前，需要定义**常驻身份提供者的**术语。 该术语对应于用户、以及用户身份所在的身份提供者的引用以及负责断言用户声明的身份。

此用例的部署体系结构和配置将与[上一篇文章](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.com.hk&sl=en&sp=nmt4&tl=zh-CN&u=https://wso2.com/library/articles/2018/04/multi-region-deployment-for-wso2-identity-server-part-1/&xid=17259,15700023,15700186,15700190,15700248,15700253&usg=ALkJrhgWugwAUYQAQ6SpkdrXi5N8LNPdXA)中的用例1大致相同。 但是，有一些差异需要强调。

在此部署中，由于不会跨地域复制身份数据，因此任何身份相关的操作（如身份验证，密码更新，帐户更新等）仅限于相应用户的常驻身份提供商，而与应用相关的操作（如单点登录和身份联合、应用权利等）在所有地域身份提供者和/或授权服务器都可能发生。

与用例1相反，一个地域不能充当另一个地域的灾难恢复（DR）站点，因为那时跨地域划分身份数据的目标是不可实现的。 因此，每个地域必须拥有自己的专用DR站点。

如果用户的地域授权服务器/身份提供者和该用户的常驻身份提供者不同，则从应用到该特定用户的该地域授权服务器的所有单点登录和联合请求将必须与相应的用户联合。用户的常驻身份提供程序，使用身份联合协议对用户进行身份验证。

与用例1中的身份同步模式一样，我们可以识别以下两种拓扑，以在身份提供者之间建立身份联合：

### 全连接网格拓扑

[![img](https://wso2.com/files/field/image/Multi%20Region%20Deployment%20for%20WSO2%20Identity%20Server%20-%20Part%202.png)](https://wso2.com/files/field/image/Multi%20Region%20Deployment%20for%20WSO2%20Identity%20Server%20-%20Part%202.png)

在此拓扑中，每个地域身份提供者/授权服务器将维护用户及其地域的完整列表，以便与相应的常驻身份提供者联合。

### 中心辐射拓扑

[![img](https://wso2.com/files/field/image/Multi%20Region%20Deployment%20for%20WSO2%20Identity%20Server%20-%20Part%202%20%281%29.png)](https://wso2.com/files/field/image/Multi%20Region%20Deployment%20for%20WSO2%20Identity%20Server%20-%20Part%202%20%281%29.png)

此拓扑引入了一个身份联邦中心（Identity Federation Hub），负责管理用户及其地域，并与相应的常驻身份提供商联合。 例如，可以以***查找表***的形式管理用户及其地域。 为了屏蔽用户名，我们可以散列并存储它们。

从地域身份提供者或联盟中心联合到常驻身份提供者时，我们需要使用指定的地域的DNS URL。 但是，写入用户代理以跟踪用户活动的cookie需要使用全局域编写，否则这些cookie不能用于跟踪跨地域的用户会话。

下图显示了此用例的单点登录和联合的序列图如何显示。 我们将使用OpenID Connect作为示例来说明应用与地域身份提供程序之间的流程，因为它是单点登录和联合流程中最复杂的协议。 但是，该用例也适用于任何其他单点登录和联合协议，例如SAML2 SSO，WS-Federation Passive Requestor Profile和CAS。 我们将仅查看地域身份提供者/授权服务器不是相应用户的常驻身份提供者的情况的序列图。对于其他用例，序列图看起来类似于常规单点登录和联合流程。

[![img](https://wso2.com/files/field/image/Picture3_6.png)](https://wso2.com/files/field/image/Picture3_6.png)

​                              图1：网状拓扑下具有身份联邦的OpenID 连接流的序列图

[![img](https://wso2.com/files/field/image/Picture4_4.png)](https://wso2.com/files/field/image/Picture4_4.png)

​                              图2：中心辐射拓扑下使用身份联邦的OpenID 连接流的序列图

在本系列的[第1部分](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.com.hk&sl=en&sp=nmt4&tl=zh-CN&u=https://wso2.com/library/articles/2018/04/multi-region-deployment-for-wso2-identity-server-part-1/&xid=17259,15700023,15700186,15700190,15700248,15700253&usg=ALkJrhgWugwAUYQAQ6SpkdrXi5N8LNPdXA)中，我们研究了用例1的OAuth2的一个特殊问题。同样的问题也适用于用例2。 与用例1一样，OAuth2授权请求和OAuth2令牌请求有可能由两个不同的地域提供服务，这就产生了这个问题。 换句话说，用户的地域授权服务器和应用的地域授权服务器不相同。 移动应用不会出现此问题，因为用户和应用实例在任何给定时间都在同一地域，但对web应用，如果应用实例在与物理用户不同的地域中运行，这个问题就会发生。 因此，第1部分中讨论的相同解决方案也适用于此。

我们不会像在用例1中看到的那样查看所有4个解决方案的序列图，但是会选择并将同一列表中的解决方案3应用于此情况下的相同问题，并结合使用中心辐射拓扑，并查看发生这种情况时序列图是什么样的。 您应该能够轻松地推导出我们讨论的其他3个解决方案的序列图，以及应用于网状拓扑的所有4个解决方案的序列图。

[![img](https://wso2.com/files/field/image/Picture5_3.png)](https://wso2.com/files/field/image/Picture5_3.png)

图3：OpenID 连接流的序列图，其中物理用户和应用实例来自不同地域，并与用于身份联合的中心辐射拓扑结合

## 用例3的多地域部署 - 按所属地域划分所有数据

在讨论此用例之前，需要定义**常驻授权服务器**术语。 该术语分别对应于应用或服务提供者，并且指代应用或服务提供者的逻辑表示所在的授权服务器，并且负责对所请求的访问进行认证和授权应用或服务提供者。

此用例的部署体系结构和配置将与[用例1中的图1](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.com.hk&sl=en&sp=nmt4&tl=zh-CN&u=https://wso2.com/library/articles/2018/04/multi-region-deployment-for-wso2-identity-server-part-1/&xid=17259,15700023,15700186,15700190,15700248,15700253&usg=ALkJrhgWugwAUYQAQ6SpkdrXi5N8LNPdXA)大致相同。 但是，有一些差异需要强调。

在此部署中，由于身份数据和配置数据都不是跨地域复制的，因此任何身份相关的操作（如身份验证，密码更新，帐户更新等）仅限于相应用户的常驻身份提供者，而应用相关操作（如单点登录、联合，应用权利等）仅限于相应应用的常驻授权服务器。

出于与用例2中相同的原因，每个地域还必须具有其自己的专用DR站点。

可以手动或通过数据库的多主复制来完成地域内的配置数据同步。 鉴于数据库支持跨地域的多主技术，它也可以在这种情况下工作，因为地域内的节点数通常限制为2。

在用户的地域授权服务器和用户尝试访问的应用的常驻授权服务器不同的情况下，从该应用到该特定用户的地域授权服务器的所有单点登录和联合请求必须重定向到相应应用的常驻授权服务器以验证应用。

为了识别常驻授权服务器，我们将使用自包含客户端标识符模式。 自包含客户端标识符可以是SAML2单点登录中的服务提供者实体ID，OAuth2/OpenID 连接中的client_id，依此类推。

当从用户的地域授权服务器重定向到相应应用的常驻授权服务器时，我们需要使用地域特定的DNS URL。

我们将仅查看地域身份提供者/授权服务器不是相应用户的常驻身份提供者的情况的序列图。

下图显示了此用例的单点登录和联合的序列图如何显示。 我们将使用OpenID Connect作为示例来说明在这种情况下应用和地域授权服务器之间的流程。 但是，该解决方案也可以扩展到任何其他单点登录和联合协议。 我们将仅查看用户的地域身份提供者/授权服务器和用户尝试访问的特定应用的常驻授权服务器与网状拓扑结合的情况的序列图。 同样，您应该能够使用中心辐射拓扑推导出解决方案的序列图。

[![img](https://wso2.com/files/field/image/Picture6_6.png)](https://wso2.com/files/field/image/Picture6_6.png)

图4：OpenID 连接流程的序列图，其中用户的地域授权服务器和应用的常驻授权服务器不同，并与用于身份联合的hub-and-spoke拓扑结合

我们在用例1和2中看到的OAuth2的特殊问题也可能发生在用例3l中。 为了发生此问题，授权调用和令牌调用必须由两个不同的授权服务器提供服务。 在用例3中，始终从常驻授权服务器发出授权代码。 如果常驻授权服务器和应用实例的地域授权服务器不同，则可能会出现此问题。 虽然在Web应用中很少见，但如果有一个多地域Web应用在某些地域中没有授权服务器，则可能会出现这种情况。 此问题在移动应用中更为常见，其中移动设备可以跨地域与其用户一起漫游。

要解决此问题，应用必须使用指定的地域的DNS URL进行令牌调用。 一种解决方案是通过配置参数向应用提供常驻授权服务器的令牌端点URL。 这种方式有一些优点，例如：

- WSO2 IS不需要扩展
- 始终只能通过网络呼叫令牌端点以获取访问令牌

除上述内容外，在用例1中讨论的包含自包含授权码的解决方案也适用于此用例。 但是，在这种情况下，常驻授权服务器相对于应用是固定的，并且不随每个请求而改变。 在这种情况下，使用自包含授权代码可被视为过度设计。 因此，我们将只使用自包含的client_ids来编码发布client_id的常驻授权服务器的域，而不是使用自包含的授权代码。 用例1中讨论的每个解决方案的优缺点对于此用例也将保持不变。 例如，自包含的client_id可以是以下格式的JSON字符串：

```json
{"random":"abcd…1234","oauth2_authz_server":"https://oauth2.wso2.eu/"}
```

下图演示了此情况发生时与全连接拓扑结合使用时序列图的外观。 您应该能够轻松地获得具有中心辐射拓扑的解决方案的序列图。***=============这句话似乎有错误  译者注===============***

[![img](https://wso2.com/files/field/image/Picture7_0.png)](https://wso2.com/files/field/image/Picture7_0.png)

图5：OpenID 连接流程的序列图，其中应用的地域授权服务器不是常驻授权服务器，与用于身份联合的中心辐射拓扑结合

WSO2身份服务器中还有其他Restful API，它们使用OAuth2访问令牌（例如OAuth2 Token Introspection端点和OpenID Connect UserInfo端点）进行保护，这些端点由OAuth2客户端和OAuth2资源服务器访问。 为了使这些OAuth2客户端和OAuth2资源服务器能够访问OAuth2授权服务器的正确地域，我们必须告知他们负责颁发令牌的地域。 为了传达这些信息，我们必须使用自包含的访问令牌。 自包含访问令牌将对发出访问令牌的常驻授权服务器的域进行编码。 例如，自包含访问令牌可以是以下格式的JSON字符串：

```json
{"random":"abcd…1234","oauth2_authz_server":"https://oauth2.wso2.eu/"}
```

## 摘要



本文详细介绍了我们在[本系列文章的第1部分中](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.com.hk&sl=en&sp=nmt4&tl=zh-CN&u=https://wso2.com/library/articles/2018/04/multi-region-deployment-for-wso2-identity-server-part-1/&xid=17259,15700023,15700186,15700190,15700248,15700253&usg=ALkJrhgWugwAUYQAQ6SpkdrXi5N8LNPdXA)提到的WSO2 IS的其余两个多地域部署用例。 

与第一部分类似，我们研究了如何在不依赖数据库复制技术的情况下为拥有全局IT管理的组织支持这些用例。