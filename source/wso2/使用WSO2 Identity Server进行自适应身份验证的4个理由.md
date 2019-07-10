[TOC]

# 使用WSO2 Identity Server进行自适应身份验证的4个理由

- 作者： Ishara Karunarathna

- 2018年10月8日

  

## 向自适应身份验证问好

传统的用户名 - 密码身份验证不再足以保护关键数据和系统。 多因素身份验证（MFA）是为给定应用程序提供更安全的用户可信度的一种方法。 尽管MFA可以提高应用程序的安全性，但它会降低应用程序的可用性方面，因为用户每次尝试登录应用程序时都必须经过几个身份验证步骤。

这是自适应身份验证发挥作用的地方，提供安全性以及增强的可用性。 自适应身份验证是MFA的一种演进形式，其中可以配置和部署身份验证步骤，以便系统根据用户的风险配置文件和行为决定在身份验证过程中评估哪些步骤。 如果风险等级高，则可以收紧认证层，而如果风险等级低，则可以忽略多个认证层。

## 自适应认证的好处

自适应身份验证通过消除用户不必要的摩擦并提高应用程序的安全性，确保安全性和可用性之间的健康平衡。 自适应身份验证评估上下文和其他相关数据，以便对用户身份做出明智的决策。

自适应身份验证策略应评估用户身份属性，地理位置，用户活动和IP地址的组合，以决定如何路由身份验证请求。 完美的自适应身份验证解决方案应该能够评估各种输入，并针对每种方案的精确身份验证级别做出实时决策。

不同的组织有不同的认证需求。 因此，自适应身份验证提供程序应该为用户提供区分解决方案和实现其自己的身份验证策略集的机会。

[WSO2 Identity Server](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.com.hk&sl=en&sp=nmt4&tl=zh-CN&u=https://wso2.com/identity-and-access-management/&xid=17259,15700023,15700186,15700190,15700248,15700253&usg=ALkJrhjrvFgKgUHZWqWQ1MM3-JGT3pueYg)结合了多因素身份验证，并提供自适应身份验证，智能和实时风险分析，优化的用户体验以及更轻松的合规性。 为了满足所有这些功能，WSO2 Identity Server提供了市场上最佳的自适应身份验证解决方案。

## 使用WSO2 Identity Server进行自适应身份验证的好处

1. ### 以更简单的方式提供复杂的身份验证策略

   WSO2身份服务器（IS）自适应身份验证植入带有丰富的基于脚本的策略语言，可帮助您克服传统UI工具强制执行的障碍。 WSO2 Identity Server的管理控制台具有强大的身份验证脚本编辑器，可轻松建立新策略。 使用类似JavaScript（JS）的语言来实现复杂的身份验证策略降低了复杂性级别。 此功能允许您根据编写的脚本定义动态验证序列。

   将自适应身份验证配置为静态和动态策略的组合是[WSO2 Identity Server](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.com.hk&sl=en&sp=nmt4&tl=zh-CN&u=https://wso2.com/identity-and-access-management/&xid=17259,15700023,15700186,15700190,15700248,15700253&usg=ALkJrhjrvFgKgUHZWqWQ1MM3-JGT3pueYg)自适应身份验证解决方案的另一个优点。 在实现自适应身份验证策略时，可能会关注静态策略（如用户角色，用户属性和用户存储）以及动态策略（如在设备使用，IP范围，地理速度上定义的用户趋势）。

2. ### 用于设计自适应身份验证序列的综合工具集

   WSO2 Identity Server中功能丰富的用户界面提供了一个全面的工具集来设计自适应身份验证序列。 有一组预定义的自适应身份验证模板，涵盖几乎所有行业[用例，](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.com.hk&sl=en&sp=nmt4&tl=zh-CN&u=https://wso2.com/library/article/2018/10/5-instances-to-use-adaptive-authentication-use-cases/&xid=17259,15700023,15700186,15700190,15700248,15700253&usg=ALkJrhiVQ08iv_HcyNOGGWGMw6jgHWTARw)如基于角色，基于用户年龄，基于租户，基于用户存储，基于IP，基于新设备，基于ACR（身份验证） [这里](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.com.hk&sl=en&sp=nmt4&tl=zh-CN&u=https://docs.wso2.com/display/IS570/Adaptive%2BAuthentication%2BScenarios&xid=17259,15700023,15700186,15700190,15700248,15700253&usg=ALkJrhiCSHJQHOxayU-irC9lyIqQsor3Cg)很好地描述[了](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.com.hk&sl=en&sp=nmt4&tl=zh-CN&u=https://docs.wso2.com/display/IS570/Adaptive%2BAuthentication%2BScenarios&xid=17259,15700023,15700186,15700190,15700248,15700253&usg=ALkJrhiCSHJQHOxayU-irC9lyIqQsor3Cg) Context Reference，以及基于风险的自适应身份验证。 因此，管理员可以在几分钟内通过略微偏离现有模板来获得生成新策略的优势。 模板的可用性为自适应身份验证策略制定提供了显着**的省时基础** 。

   验证器处理用户对应用程序的验证。 任何自适应身份验证提供程序都有一个基本要求，即拥有一组具有不同因素的丰富的身份验证。 [WSO2 Identity Server](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.com.hk&sl=en&sp=nmt4&tl=zh-CN&u=https://wso2.com/identity-and-access-management/&xid=17259,15700023,15700186,15700190,15700248,15700253&usg=ALkJrhjrvFgKgUHZWqWQ1MM3-JGT3pueYg)支持大量第三方身份验证器，如Facebook，LinkedIn和Email OTP。 可以为所需的第三方认证系统配置身份提供程序，以验证用户登录到应用程序。 可以将多个第三方身份验证系统配置为在多因素身份验证中处理单个身份验证请求。

   默认情况下， [WSO2 Identity Server](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.com.hk&sl=en&sp=nmt4&tl=zh-CN&u=https://wso2.com/identity-and-access-management/&xid=17259,15700023,15700186,15700190,15700248,15700253&usg=ALkJrhjrvFgKgUHZWqWQ1MM3-JGT3pueYg)附带基于用户名/密码的身份验证。 通过添加其他身份验证步骤可以增强身份验证的安全性。 WSO2 Identity Server允许配置多步验证，您可以在不同的步骤中定义包含不同验证器的验证链。 WSO2 Identity Server全面支持多因素身份验证，可为SMSOTP，FIDO，MEPin等提供身份验证器。

3. ### 开放，面向未来的自适应认证平台

   **WSO2 Identity Server自适应认证解决方案可以定义为开放且面向未来的自适应认证平台** 。 因此， [WSO2 Identity Server](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.com.hk&sl=en&sp=nmt4&tl=zh-CN&u=https://wso2.com/identity-and-access-management/&xid=17259,15700023,15700186,15700190,15700248,15700253&usg=ALkJrhjrvFgKgUHZWqWQ1MM3-JGT3pueYg)已成为身份提供商的领导者。 它具有开发人员友好的方法，而不是特定于供应商的解决方案。 用户不受预定义模板中可用策略的限制。

   WSO2有一个连接器存储，带有200多个免费和开放的连接器，用于流行的业务关键型服务。 这些连接器允许[WSO2 Identity Server](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.com.hk&sl=en&sp=nmt4&tl=zh-CN&u=https://wso2.com/identity-and-access-management/&xid=17259,15700023,15700186,15700190,15700248,15700253&usg=ALkJrhjrvFgKgUHZWqWQ1MM3-JGT3pueYg)从外部系统获取服务并轻松与其集成。 WSO2 Identity Server代码库在Github上公开可用，并且具有全面的文档，教程和自我引导示例。 因此，客户可以修改和重用WSO2 Identity Server中提供的组件。

4. ### 旨在快速与风险引擎和外部系统集成

   WSO2的自适应认证功能旨在以快速集成风险引擎和外部系统的方式进行设计。作为默认引擎，WSO2流处理器被配置为自适应身份验证策略的风险引擎。WSO2流处理器是一个轻量级，精简，基于流的SQL流处理平台，允许您收集事件，实时分析事件，识别模式，映射其影响，并在几毫秒内传达结果。 可以将此产品配置为风险引擎，以评估基于风险的策略的用户身份验证详细信息

## 摘要

由于自适应身份验证适应用户而非破坏性MFA的无摩擦体验，自适应身份验证已成为身份验证过程的未来绿灯。 但是，只有选择了具有自适应身份验证解决方案的最佳IDP，该优势才能为用户/管理员提供更多增值功能。

如上所述， [WSO2 Identity Server](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.com.hk&sl=en&sp=nmt4&tl=zh-CN&u=https://wso2.com/identity-and-access-management/&xid=17259,15700023,15700186,15700190,15700248,15700253&usg=ALkJrhjrvFgKgUHZWqWQ1MM3-JGT3pueYg)提供了一种优于其他竞争解决方案的自适应身份验证解决方案。 基于脚本的策略语言以简单的方式降低复杂性和为用户提供功能的超强能力，设计自适应认证功能的综合工具集，以及开源 - 未来可证明 - 可扩展平台是采用WSO2自适应的主要原因认证方案。