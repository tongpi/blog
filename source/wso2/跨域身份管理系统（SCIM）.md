[TOC]



# 跨域身份管理系统（SCIM）

- 作者： Vindula Jayawardana

- 2017年10月23日

  

## 介绍

在过去十年中，尽管存在负面的问题和影响，但全球正在转向基于云的运营环境。 虽然从内部部署到云的转换过程相当缓慢，但任何此类动机都需要谨慎进行。 身份配置是一个需要考虑的重要方面，因为数据安全漏洞可能会造成严重损害。 [跨域身份管理系统](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.com.hk&sl=en&sp=nmt4&tl=zh-CN&u=http://www.simplecloud.info/&xid=17259,15700023,15700186,15700190,15700248,15700253&usg=ALkJrhjh47wTVPGkB0K9PnlpOENZPQL2ZA)（SCIM）作为一种新兴的开放标准发挥作用，使基于云的应用程序和服务中的身份配置更容易，更便宜，更快速。 [WSO2 Identity Server](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.com.hk&sl=en&sp=nmt4&tl=zh-CN&u=https://wso2.com/identity-and-access-management&xid=17259,15700023,15700186,15700190,15700248,15700253&usg=ALkJrhh2bLjur_sBTV8DaUrHIrPpopwPwQ)支持SCIM 1.1和SCIM 2.0以进行身份配置。

### 身份配置

简单来说， [身份供应](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.com.hk&sl=en&sp=nmt4&tl=zh-CN&u=https://docs.wso2.com/identity-server/Key%2BConcepts&xid=17259,15700023,15700186,15700190,15700248,15700253&usg=ALkJrhhNJFQCMirUDfKO3K6EcwlA2X6xiw#KeyConcepts-Identityprovisioning)是响应于自动化或交互式业务流程，在一个或多个系统或应用程序中创建，维护和停用用户帐户。

在为采用SCIM时，基于云的环境中的传统身份配置类似于下图所示，其中包括从企业云订阅者（ECS）到云服务提供商（CSP）的多个冗余集成工作。

[![img](https://wso2.com/files/field/image/scim-article-1.png)](https://wso2.com/files/field/image/scim-article-1.png)

​									**图1**   不采用SCIM连接云服务的架构

尽管这个过程是有效的，但是增加复杂的和成本高昂的多个连接器的维护工作可能是一场噩梦。 减轻这种情况的简单解决方案是一个简单的开放协议，每个人都同意强调需要一个系统来进行身份提供的跨域身份管理。

[![img](https://wso2.com/files/field/image/scim-article-2.png)](https://wso2.com/files/field/image/scim-article-2.png)

​                                                                                 **图2  采用SCIM连接云服务的架构**

根据[Request for Comments](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.com.hk&sl=en&sp=nmt4&tl=zh-CN&u=https://tools.ietf.org/html/rfc7642&xid=17259,15700023,15700186,15700190,15700248,15700253&usg=ALkJrhi-TOO9ryXSLwLZafkqShZs1gnKLg) （RFC）7642， [Internet工程任务组](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.com.hk&sl=en&sp=nmt4&tl=zh-CN&u=https://www.ietf.org/&xid=17259,15700023,15700186,15700190,15700248,15700253&usg=ALkJrhgBbFKuV0gBRIj_4ejnLbuFdB30fA) （IETF）将SCIM的需求解释为：SCIM规范旨在以标准化方式管理基于云的应用程序和服务中的用户身份，以实现互操作性，安全性和可扩展性。

## SCIM版本

第一个版本SCIM 1.0于2011年发布，随后是2012年7月发布的名为1.1版的下一个版本。当前标准SCIM 2.0于2015年9月作为IETF RFC发布。尽管SCIM最初设计为基于云的使用，事实证明，在本地移动身份的通用语言也可能是一个非常有用的场景，后者为SCIM创建了一个主要的适用平台。

## SCIM规范

SCIM规范有两个主要组件：

- 核心模式提供了用于表示用户和组的抽象模式和扩展模型。 该模式的标准序列化采用JSON格式提供。
- SCIM协议规范定义了一个[REST API，](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.com.hk&sl=en&sp=nmt4&tl=zh-CN&u=https://docs.wso2.com/display/ISCONNECTORS/Configuring%2BSCIM%2B2.0%2BProvisioning%2BConnector&xid=17259,15700023,15700186,15700190,15700248,15700253&usg=ALkJrhjP8gTqk0eXPD233Dmuj_r7CTMd_Q)用于通过JSON交换身份资源。

## SCIM机制

SCIM由通用用户和组方案以及扩展模型的概念提供支持。 它不仅仅促进了这样的方案，而且也使用标准协议交换这些方案的有约束力的合约，这使得SCIM成为开放的连接器。 下图说明了SCIM 2.0的对象模型。 如下所示，Resource是共同特性，所有SCIM对象都是从它派生的。 资源具有id，externalId和meta作为属性， [RFC7643](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.com.hk&sl=en&sp=nmt4&tl=zh-CN&u=https://tools.ietf.org/html/rfc7643&xid=17259,15700023,15700186,15700190,15700248,15700253&usg=ALkJrhhI7aMOSx7RhSG8MbK_79FD4BHXSA)定义了扩展公共属性的User，Group和EnterpriseUser。

[![img](https://wso2.com/files/field/image/scim-article-3.png)](https://wso2.com/files/field/image/scim-article-3.png)

​                                                                          **图3 SCIM 2.0的对象模型**

为了继续资源操作，SCIM提供了丰富但简单的REST API，其集合操作功能去打足以执行大批量更新。 以下是HTTP方法及其SCIM用法的高级概述。

[![img](https://wso2.com/files/field/image/scim-article-4.png)](https://wso2.com/files/field/image/scim-article-4.png)

​                                                                            **图4 SCIM 的REST API** 

在执行上述操作时，SCIM根据资源类型的域定义端点。 /Users，/Groups，/Me和/Bulk是用于资源操作目的的此类已定义端点。 为了简化互操作性，SCIM提供了三个端点/Serviceproviderconfig，/Resourcetype和/Schema，以发现支持的功能和特定的属性详细信息。

[![img](https://wso2.com/files/field/image/scim-article-5.png)](https://wso2.com/files/field/image/scim-article-5.png)

​                                                          **图5 SCIM的端点**

为了更好地理解SCIM协议的工作原理，下图说明了通过HTTP POST方法到/Users端点的简单用户创建操作。

[![img](https://wso2.com/files/field/image/scim-article-6.png)](https://wso2.com/files/field/image/scim-article-6.png)

​                                                              **图6 通过HTTP POST方法到/Users端点的简单用户创建操作请求**

响应于该请求，服务器使用HTTP状态代码201来声明成功创建用户，并返回所创建资源的表示。

[![img](https://wso2.com/files/field/image/scim-article-7.png)](https://wso2.com/files/field/image/scim-article-7.png)

​                                                      **图7 通过HTTP POST方法到/Users端点的简单用户创建操作响应**



## 用例

掌握在现实世界中使用SCIM对初学者来说可能有些挑战。 在本节中，我们将说明SCIM的五个主要用例的高级概述。 在每种情况下，SCIM都用作开放式连接器，作为两个功能分离的各方之间的相互协议。

为了更好地理解，每个用例都与真实场景相关联，然后对其进行简要说明。

### 1.身份的迁移

公司ABC有一个应用程序LetsChat，它使用其员工的身份信息（例如标识符、属性）。 该身份信息存储在由免费云服务提供商FreeCloud控制的云中。 然而，ABC公司已决定将该身份信息迁移至不同的云服务提供商SmartCloud，以获得更好的安全性和服务。 除此之外，ABC公司还购买了另一个依赖于身份信息的SecureMail应用程序。 
通过对所有相关方使用SCIM，ABC公司可以轻松地将身份信息迁移到SmartCloud，而无需更改身份信息的表示，SecureMail可以直接使用身份信息，而不需要一个显式的连接器。 这确实节省了时间和成本，最重要的是消除了变化的痛点。

[![img](https://wso2.com/files/field/image/scim-article-8.png)](https://wso2.com/files/field/image/scim-article-8.png)

​                                                                      **图8 切换到不同云服务商之间的身份迁移**



### 2.单点登录（SSO）服务

张咪在她最喜欢的社交媒体应用程序PeopleBook中拥有一个帐户，该应用程序托管在云服务提供商SmartCloud中。 SmartCloud已将其用户身份与另一个云服务提供商OurCloud联合起来。 张咪提出要求使用在OurCloud中托管的名为ManageMe的应用程序。 ManageMe依赖于SmartCloud提供的身份信息来验证张咪。 因此，张咪从OurCloud上运行的ManageMe接收所请求的服务，而无需明确地对该应用程序进行身份验证。

[![img](https://wso2.com/files/field/image/scim-article-9.png)](https://wso2.com/files/field/image/scim-article-9.png)

​                                                                   **图9  SSO**

SCIM模式和协议提供了建立开放标准的可行性，该标准用于在SmartCloud和OurCloud之间以及应用程序和相关云服务之间交换用户身份。 简而言之，它创建了一个具有互操作性和可扩展架构的平台，并减少了所有相关方的时间和成本。

### 3.为感兴趣的社区（COI）提供用户帐户

HumanHR是一个为感兴趣的社区（Orange Inc）提供人力资源（HR）服务的组织。Orange Inc在世界各地设有办事处，其信息系统由在私有云和公共云上运行的一组应用程序以及传统IT系统组成。所有当地的Orange Inc办事处都负责收集员工的个人信息（即用户身份和属性）。 另一方面，HumanHR在公共云和私有云上提供人力资源服务作为软件即服务（SaaS）。 因此，HumanHR正在处理所有Orange Inc办事处的身份信息供应和分发。 此外，HumanHR允许他们自己管理个人信息，如果他们有资格这样做的话。 （例如，用户自己更新地址或电话号码）。

[![img](https://wso2.com/files/field/image/scim-article-10.png)](https://wso2.com/files/field/image/scim-article-10.png)

​                                                                                            **图10 **

此方案仅强调需要[用于身份配置](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.com.hk&sl=en&sp=nmt4&tl=zh-CN&u=https://docs.wso2.com/display/ISCONNECTORS/SCIM%2B2.0%2BProvisioning%2BConnector&xid=17259,15700023,15700186,15700190,15700248,15700253&usg=ALkJrhj-U3N3N84SeMmbwskqLWb9Wg4NrA)和分发[的开放连接器](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.com.hk&sl=en&sp=nmt4&tl=zh-CN&u=https://docs.wso2.com/display/ISCONNECTORS/SCIM%2B2.0%2BProvisioning%2BConnector&xid=17259,15700023,15700186,15700190,15700248,15700253&usg=ALkJrhj-U3N3N84SeMmbwskqLWb9Wg4NrA) 。 想象一下，如果每个Orange Inc本地办事处都使用自己的架构和协议进行身份交换，则需要花费很多时间。 通过基于SCIM的机制，所有个人账户都可以通过HumanHR在眨眼之间提供的服务全局通过Orange Inc系统的任何授权用户或应用程序使用。

### 4.将属性转移到依赖方的网站

老张在目录服务DServe中拥有一个帐户。 然后老张访问了一个信任方的Loople网站。 该网站要求获取被访问用户的某些属性才能正常继续使用。 在老张第一次访问该站点时，他选择了所需共享的属性，并授权通过任何授权协议（例如OAuth，SAML）将属性数据从目录服务DServe传输到Loople网站。

[![img](https://wso2.com/files/field/image/scim-article-11.png)](https://wso2.com/files/field/image/scim-article-11.png)

​                                                                     **图11 用户属性转移**

SCIM模式和协议在这里也很方便，因为需要在DServer和Loople之间建立一个交换属性的共同协议。 在这种情况下使用SCIM只会为服务提供商提供安全，可互操作的可行性。

### 5.更改通知

老苏在目录服务WEServe中拥有一个帐户，她已授权从WEServe到依赖方网站example.com的属性转移。 老苏的属性稍后在目录服务WEServe中更改（例如老苏更改其姓名或手机号码）。 但是，example.com可能具有这些属性的缓存，如果它知道对其缓存副本的这些更改，则可能会导致其中的状态更改。 但是，变更的规模可能非常大，并非所有变更都会引起依赖方的兴趣。 因此，目录服务WEServe希望通知example.com存在潜在兴趣的变化，使得example.com可以在适当的时间随后联系目录服务WEServe并仅检索感兴趣的变化子集。

[![img](https://wso2.com/files/field/image/scim-article-12.png)](https://wso2.com/files/field/image/scim-article-12.png)

​                                                                                 **图12 更改通知**

即使使用变更通知系统，SCIM也不必做太多，SCIM的可互操作性和可扩展性提供了实现所述机制的可能性。

## WSO2身份服务器和SCIM

[WSO2 身份服务器](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.com.hk&sl=en&sp=nmt4&tl=zh-CN&u=https://wso2.com/identity-and-access-management&xid=17259,15700023,15700186,15700190,15700248,15700253&usg=ALkJrhh2bLjur_sBTV8DaUrHIrPpopwPwQ)有效地承担了跨企业应用程序、服务和API的身份管理相关的复杂任务。 它利用了最广泛使用的标准SCIM的优势，并提供了一种与平台无关的方法，允许企业架构师在其数字业务的现有资产上实现统一的安全层。

WSO2身份服务器支持SCIM 1.1和[SCIM 2.0以进行身份配置](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.com.hk&sl=en&sp=nmt4&tl=zh-CN&u=https://docs.wso2.com/display/ISCONNECTORS/SCIM%2B2.0%2BProvisioning%2BConnector&xid=17259,15700023,15700186,15700190,15700248,15700253&usg=ALkJrhj-U3N3N84SeMmbwskqLWb9Wg4NrA) 。 除此之外，WSO2在Apache 2.0许可下提供了一个名为[WSO2 Charon的](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.com.hk&sl=en&sp=nmt4&tl=zh-CN&u=https://github.com/wso2/charon&xid=17259,15700023,15700186,15700190,15700248,15700253&usg=ALkJrhgC9NrVrVL5SNwZglBMIei_DD4zgA)开源SCIM实现。 任何想要为其应用程序添加基于SCIM的供应支持的人都可以使用[WSO2 Charon](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.com.hk&sl=en&sp=nmt4&tl=zh-CN&u=https://docs.wso2.com/identity-server/Provisioning%2BArchitecture&xid=17259,15700023,15700186,15700190,15700248,15700253&usg=ALkJrhj6fKjLnjTup81OGIWhY6FiBNENMA#ProvisioningArchitecture-SCIMimplementationusingWSO2Charon) 。 WSO2 Charon与WSO2 身份服务集成在一起，以提供基于SCIM的身份配置。

以下是WSO2 Identity Server的SCIM服务提供程序体系结构的高级概述。

[![img](https://wso2.com/files/field/image/scim-article-13.png)](https://wso2.com/files/field/image/scim-article-13.png)

​                                                       **图13 WSO2 Identity Server的SCIM服务提供程序体系结构 **



## 结论

SCIM规范旨在以标准化方式管理基于云的应用程序和服务中的用户身份，以实现互操作性，安全性和可伸缩性。 规范套件旨在利用现有模式和部署的经验，特别强调开发和集成的简单性，同时应用现有的身份验证，授权和隐私模型。 SCIM规范的目的是通过提供公共用户模式和扩展模型来降低用户管理操作的成本和复杂性，以及绑定文档以提供使用标准协议交换该模式的模式。 从本质上讲，使其快速，便宜，并且易于将用户移入，移出和移动到云中。



## 参考

- [1] SCIM定义，概述，概念和要求 - [https://tools.ietf.org/html/rfc7642](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.com.hk&sl=en&sp=nmt4&tl=zh-CN&u=https://tools.ietf.org/html/rfc7642&xid=17259,15700023,15700186,15700190,15700248,15700253&usg=ALkJrhi-TOO9ryXSLwLZafkqShZs1gnKLg)
- [2] SCIM协议 - [https://tools.ietf.org/html/rfc7644](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.com.hk&sl=en&sp=nmt4&tl=zh-CN&u=https://tools.ietf.org/html/rfc7644&xid=17259,15700023,15700186,15700190,15700248,15700253&usg=ALkJrhjyVHOb5Du20161HzKKFSOm_A2mQw)
- [3] SCIM核心架构 - [https://tools.ietf.org/html/rfc7643](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.com.hk&sl=en&sp=nmt4&tl=zh-CN&u=https://tools.ietf.org/html/rfc7643&xid=17259,15700023,15700186,15700190,15700248,15700253&usg=ALkJrhhI7aMOSx7RhSG8MbK_79FD4BHXSA)
- [4] [https://medium.com/@vindulajayawardana/scim-make-it-fast-cheap-and-easy-b2bd56492c15](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.com.hk&sl=en&sp=nmt4&tl=zh-CN&u=https://medium.com/%40vindulajayawardana/scim-make-it-fast-cheap-and-easy-b2bd56492c15&xid=17259,15700023,15700186,15700190,15700248,15700253&usg=ALkJrhg552akxwIrDBFd4Jszf4xI8yawhQ)
- [5] [https://medium.com/@vindulajayawardana/5-things-that-will-not-be-a-nightmare-anymore-if-you-support-scim-9353d73836a7](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.com.hk&sl=en&sp=nmt4&tl=zh-CN&u=https://medium.com/%40vindulajayawardana/5-things-that-will-not-be-a-nightmare-anymore-if-you-support-scim-9353d73836a7&xid=17259,15700023,15700186,15700190,15700248,15700253&usg=ALkJrhi5UqePPsy8pJJNoFdC8TkR1jT8Ig)
- [6] [https://wso2.com/identity-and-access-management](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.com.hk&sl=en&sp=nmt4&tl=zh-CN&u=https://wso2.com/identity-and-access-management&xid=17259,15700023,15700186,15700190,15700248,15700253&usg=ALkJrhh2bLjur_sBTV8DaUrHIrPpopwPwQ)