2019年4月4日  马特雷布尔

# 使用Java 12和JHipster 6更好，更快，更轻松的Java

最近在Java生态系统中发生了很多事情。现在主要的Java版本每六个月发布一次，可能很难跟上。就个人而言，我使用Spring Boot开发了大多数Java应用程序。因此，在去年10月Spring Boot 2.1发布之前，我被困在Java 8上。

Spring Boot 2.1增加了Java 11支持，性能改进，新的度量实现，新的执行器端点以及Spring Security的一流OIDC支持。

在成为Java开发人员之前，我是一名Web开发人员。我已经开发了20年的Java，所以这说了很多！如果您是Java开发人员，那么您可能会对JavaScript世界的变化速度感到困惑。如果您认为Java很难跟上，请尝试跟上JavaScript及其生态系统！好消息是有一个很好的方法来跟上两者：只使用[JHipster](https://www.jhipster.tech/)。

JHipster 6使用Spring Boot 2.1作为其后端API，这意味着您可以使用Java 11+开发JHipster应用程序！

如果您更愿意观看视频，[我会创建此博客文章的截屏视频](https://youtu.be/Ktnvqoouulg)。



## [JHipster如何使Java和JavaScript开发更容易？](https://developer.okta.com/blog/2019/04/04/java-11-java-12-jhipster-oidc#how-does-jhipster-make-java-and-javascript-development-easier)

JHipster是我在这个星球上最喜欢的开源项目之一。它是一个用于生成，开发和部署Spring Boot + Angular应用程序的开发平台。此外，它还支持React，Vue以及使用Netflix OSS，Elastic Stack和Docker创建微服务架构。

Java开发人员倾向于不喜欢JavaScript，但很多人都喜欢使用TypeScript。你猜怎么着？JHipster在其所有UI框架实现中使用TypeScript！

JHipster通过为您配置所有内容并使用其底层框架的最新版本，使Java和JavaScript开发更容易。想要升级您的应用以使用最新版本？简单地跑`jhipster upgrade`。

## [安装Java 11或Java 12](https://developer.okta.com/blog/2019/04/04/java-11-java-12-jhipster-oidc#install-java-11-or-java-12)

在您担心安装JHipster之前，请安装Java 11.我建议使用[SDKMAN！](https://sdkman.io/)为了这个任务。如果您没有安装它，它只是一个命令：

```shell
curl -s "https://get.sdkman.io" | bash
```

安装完成后，您可以看到所有可用的Java 11版本`sdk list java`。

安装OpenJDK版本：

```shell
sdk install java 11.0.2-open
```

如果您更愿意使用亚马逊的Corretto，只需使用`11.0.2-amzn`该名称即可。如果你想尽可能的时髦，你甚至可以使用Java 12！

```shell
sdk install java 12.0.0-open
```

|      | 如果您使用`12.0.0-zulu`名称，也可以使用Azul的Zulu 。 |
| ---- | ---------------------------------------------------- |
|      |                                                      |

## [安装JHipster 6](https://developer.okta.com/blog/2019/04/04/java-11-java-12-jhipster-oidc#install-jhipster-6)

JHipster 6于[2019年5月3日发布](https://www.jhipster.tech/2019/05/02/jhipster-release-6.0.0.html)。您可以使用以下命令安装它。

```shell
npm install -g generator-jhipster@6.0.0
```

|      | 该`npm`命令是[Node.js的](https://nodejs.org/)一部分。您需要让Node 10.x安装JHipster并运行有用的命令。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

## [创建一个在Java 11+上运行的Spring Boot + Angular App](https://developer.okta.com/blog/2019/04/04/java-11-java-12-jhipster-oidc#create-a-spring-boot-angular-app-that-runs-on-java-11)

开始使用JHipster的最基本方法是创建一个新目录，cd进入它并运行`jhipster`。系统会提示您回答有关您要创建的应用的一些问题。问题范围从您的应用程序名称到您要使用的身份验证类型，再到SQL与NoSQL。

JHipster还有一种域语言（称为JLipster域语言的JDL），您可以使用它来定义您的应用程序。

```plaintext
application {
  config {}
}
```

如果您使用上述代码创建应用程序，它将使用默认值：

- baseName的： `jhipster`
- 申请类型： `monolith`
- 数据库类型： `sql`
- 等等

|      | 您可以在[JHipster的JDL文档中](https://www.jhipster.tech/jdl/#available-application-options)看到所有默认值。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

要使用JHipster生成启用OAuth 2.0的应用程序，请`app.jh`在新项目目录中创建一个文件（例如`~/hipapp`）：

```plaintext
application {
  config {
    baseName hipapp
    authenticationType oauth2
  }
}
```

打开终端窗口并导航到与此文件相同的目录。运行以下命令以生成带有Angular UI的Spring Boot API。

|      | 确保你不在你的主目录中！您的项目将在与...相同的目录中生成`app.jh`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

```shell
jhipster import-jdl app.jh
```

这将创建大量文件并使用安装依赖项`npm install`。运行此命令时，终端应类似于以下内容：

![Results of import-jdl](https://developer.okta.com/assets-jekyll/blog/java-12-jhipster-6/import-jdl-7db81bd6575856b5ad3769531b8d5f358d67dae3a1c975aefd2fea6aeb8c99bc.png)

如果你想看看这个命令在行动中看起来是什么，你可以观看下面的录音。

<iframe src="https://asciinema.org/a/244194/embed?" id="asciicast-iframe-244194" name="asciicast-iframe-244194" scrolling="no" allowfullscreen="true" style="box-sizing: border-box; max-width: 100%; overflow: hidden; margin: 0px; border: 0px; display: inline-block; width: 804px; float: none; visibility: visible; height: 434px;"></iframe>

由于您指定`oauth2`了身份验证类型，因此将为Keycloak安装Docker Compose配置。

[Keycloak](https://www.keycloak.org/)是Apache许可的开源身份和访问管理解决方案。除了[`src/main/docker/keycloak.yml`](https://github.com/jhipster/generator-jhipster/blob/master/generators/server/templates/src/main/docker/keycloak.yml.ejs)为Docker Compose 创建文件外，JHipster还会生成一个[`src/main/docker/config/realm-config`](https://github.com/jhipster/generator-jhipster/tree/master/generators/server/templates/src/main/docker/config/realm-config)包含文件的目录，该目录将Keycloak配置为与JHipster开箱即用。

## [运行您的JHipster应用程序并使用Keycloak登录](https://developer.okta.com/blog/2019/04/04/java-11-java-12-jhipster-oidc#run-your-jhipster-app-and-log-in-with-keycloak)

必须运行Keycloak才能使您的JHipster应用程序成功启动。这是因为Spring Security 5.1的[一流OIDC支持](https://developer.okta.com/blog/2019/03/05/spring-boot-migration)在JHipster 6中得到了充分利用。

此OIDC支持包括发现，这意味着Spring Security会与`/.well-known/openid-configuration`端点进行通信以进行自我配置。我自己完成[了迁移](https://github.com/jhipster/generator-jhipster/pull/9416)并删除了比我添加的代码更多的代码！

使用Docker Compose启动Keycloak：

```shell
docker-compose -f src/main/docker/keycloak.yml up -d
```

|      | 如果您没有安装Docker Compose，请参阅[这些说明](https://docs.docker.com/compose/install/)以了解如何安装它。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

然后使用Maven启动您的应用程序：

```shell
./mvnw
```

当您的应用程序启动并运行时，请`http://localhost:8080`在您喜欢的浏览器中打开并单击**登录**。您将被重定向到Keycloak，您可以在其中输入`admin/admin`登录。

![欢迎，Java时髦](https://developer.okta.com/assets-jekyll/blog/java-12-jhipster-6/welcome-java-hipster-f98915cd9e41341e9bea62953d579dd0cdbe25dbdba7a5d1a55037f2ec310427.png)

漂亮，嗯？您刚刚创建了一个使用最新发布的Angular版本的现代单页面应用程序（SPA）！不仅如此，它还使用最安全的OAuth 2.0形式 - [授权代码流](https://developer.okta.com/blog/2018/04/10/oauth-authorization-code-grant-type)。

|      | 如果您对OAuth 2.0和OpenID Connect（OIDC）如何协同工作感到困惑，请参阅[什么是OAuth？](https://developer.okta.com/blog/2017/06/21/what-the-heck-is-oauth)简而言之，OIDC是OAuth 2.0之上的一个薄层，可以添加身份。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

## [使用Okta：我们永远在线！](https://developer.okta.com/blog/2019/04/04/java-11-java-12-jhipster-oidc#use-okta-were-always-on)

Keycloak是一个非常出色的项目，非常适合开发和测试。但是，如果您在生产中使用它，您将负责维护它，将其更新到最新版本，并确保它全天候运行。出于这些原因，我建议在生产中使用Okta。毕竟，我们一直*都在！*😃

### [创建一个OpenID Connect Web应用程序](https://developer.okta.com/blog/2019/04/04/java-11-java-12-jhipster-oidc#create-an-openid-connect-web-application)

首先，注册一个[永久免费的Okta开发者帐户](https://developer.okta.com/signup/)。

登录Okta后，注册您的客户端应用程序。

- 在顶部菜单中，单击“ **应用程序”**
- 单击**添加应用程序**
- 选择**Web**，然后单击**下一步**
- 输入`Awesome OIDC`的**名称**（这个数值并不重要，可以随意去改变它）
- 将Login重定向URI更改为 `http://localhost:8080/login/oauth2/code/oidc`
- 单击**“完成”**，然后单击**“** **编辑”**并添加`http://localhost:8080`为“注销”重定向URI
- 单击**保存**

完成后，您的设置应类似于下面的屏幕截图。

![OIDC应用程序设置](https://developer.okta.com/assets-jekyll/blog/java-12-jhipster-6/app-settings-36cee2d55fa2872188a92e5c2c3c8d9d20ad2082d86b8fe6c7433c89567c3145.png)

`okta.env`在项目的根目录中创建一个文件，并将`{..}`值替换为Okta应用程序中的值：

```shell
export SPRING_SECURITY_OAUTH2_CLIENT_PROVIDER_OIDC_ISSUER_URI="https://{yourOktaDomain}/oauth2/default"
export SPRING_SECURITY_OAUTH2_CLIENT_REGISTRATION_OIDC_CLIENT_ID="{clientId}"
export SPRING_SECURITY_OAUTH2_CLIENT_REGISTRATION_OIDC_CLIENT_SECRET="{clientSecret}"
```

|      | 添加`*.env`到您的`.gitignore`文件，以便此文件不会在您的源代码管理系统中结束。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

### [创建组并将其添加为ID令牌的声明](https://developer.okta.com/blog/2019/04/04/java-11-java-12-jhipster-oidc#create-groups-and-add-them-as-claims-to-the-id-token)

默认情况下，JHipster配置为使用两种类型的用户：管理员和用户。Keycloak自动配置了用户和组，但您需要为Okta组织进行一次性配置。

创建一个`ROLE_ADMIN`和`ROLE_USER`组（**用户** > **组** > **添加组**）并向其添加用户。您可以使用您注册的帐户，也可以创建新用户（“ **用户”** >“ **添加人员”**）。导航到**API** > **授权服务器**，然后单击`default`服务器。单击**声明**选项卡和**添加声明**。将其命名`groups`，并将其包含在ID令牌中。将值类型`Groups`设置为并将过滤器设置为正则表达式`.*`。点击**创建**。

![添加声明](https://developer.okta.com/assets-jekyll/blog/java-12-jhipster-6/add-claim-915f54ead19da14f9117ac982daa317f6782c0f8d9fe9fcb5de68f7beb2d6f90.png)

使用以下命令启动应用程序：

```shell
source okta.env
./mvnw
```

导航到`http://localhost:8080`并使用您的Okta凭据登录。

![由Okta认证](https://developer.okta.com/assets-jekyll/blog/java-12-jhipster-6/authenticated-by-okta-c114234313c83a96ea1415c77dd665c626e49ac88487ab51513b9524176f6b9d.png)

Spring Boot和Spring Security如何轻松切换OIDC提供商，这不是很酷吗？！

## [与JHipster的CRUD](https://developer.okta.com/blog/2019/04/04/java-11-java-12-jhipster-oidc#crud-with-jhipster)

在这篇文章中，我几乎没有抓住JHipster所能提供的服务。例如，您可以使用JDL为实体（带有测试！）创建CRUD功能。例如，`blog.jh`使用下面的代码创建一个文件。

```plaintext
entity Blog {
  name String required minlength(3),
  handle String required minlength(2)
}

entity BlogEntry {
  title String required,
  content TextBlob required,
  date Instant required
}

entity Tag {
  name String required minlength(2)
}

relationship ManyToOne {
  Blog{user(login)} to User,
  BlogEntry{blog(name)} to Blog
}

relationship ManyToMany {
  BlogEntry{tag(name)} to Tag{entry}
}

paginate BlogEntry, Tag with infinite-scroll
```

然后`jhipster import-jdl blog.jh`在你的项目中运行。该[JDL-样品](https://github.com/jhipster/jdl-samples) GitHub的仓库有更多的例子。

## [使用JHipster做更多事情](https://developer.okta.com/blog/2019/04/04/java-11-java-12-jhipster-oidc#do-more-with-jhipster)

我要感谢Spring Security团队的[Joe Grandja](https://twitter.com/joe_grandja)和[Rob Winch](https://twitter.com/rob_winch)。没有他们的帮助，JHipster的迁移使用Spring Security 5.1是不可能的。你们*摇滚!!*

我没有为这篇文章创建一个GitHub存储库，因为生成了所有代码。你可以[在GitHub上](https://github.com/jhipster/generator-jhipster)找到[JHipster](https://github.com/jhipster/generator-jhipster)的源代码。

如果您对如何将JHipster的测试升级到Spring Security 5.1感兴趣，请参阅[通过Java Hipster的Up升级Spring Security OAuth和JUnit测试](https://developer.okta.com/blog/2019/04/15/testing-spring-security-oauth-with-junit)。如果您想了解如何使用JHipster开发微服务，请参阅[使用Spring Cloud Config和JHipster的Java](https://developer.okta.com/blog/2019/05/23/java-microservices-spring-cloud-config)微服务。

感谢JHipster及其所有出色的[贡献者](https://github.com/jhipster/generator-jhipster/graphs/contributors)。你们都在闲暇时间做了大量的工作，非常感谢。

还没准备好向JHipster 6和Java 11+迈进吗？我写了一些使用JHipster 5和Java 8的教程。

- [使用React，Spring Boot和JHipster构建照片库PWA](https://developer.okta.com/blog/2018/06/25/react-spring-boot-photo-gallery-pwa)
- [使用OAuth 2.0和JHipster开发微服务架构](https://developer.okta.com/blog/2018/03/01/develop-microservices-jhipster-oauth)
- [使用Ionic为JHipster创建具有OIDC身份验证的移动应用程序](https://developer.okta.com/blog/2018/01/30/jhipster-ionic-with-oidc-authentication)
- [使用React Native和Spring Boot构建移动应用程序](https://developer.okta.com/blog/2018/10/10/react-native-spring-boot-mobile-app)

我还为InfoQ的[JHipster](https://www.infoq.com/minibooks/jhipster-mini-book-5)写了一本[免费的迷你书](https://www.infoq.com/minibooks/jhipster-mini-book-5)。

如果您想了解有关Spring Security 5.1及其OIDC支持的更多信息，我们也有一些支持：

- [使用Spring Security的OAuth 2.0快速指南](https://developer.okta.com/blog/2019/03/12/oauth2-spring-security-guide)
- [将您的Spring Boot应用程序迁移到最新和最好的Spring Security和OAuth 2.0](https://developer.okta.com/blog/2019/03/05/spring-boot-migration)
- [Spring Boot 2.1：出色的OIDC，OAuth 2.0和Reactive API支持](https://developer.okta.com/blog/2018/11/26/spring-boot-2-dot-1-oidc-oauth2-reactive-apis)

在[@oktadev](https://twitter.com/oktadev)上关注我们，以便及时了解Java和领先的JavaScript框架。

**更新日志：**

- 2019年5月10日：更新后使用JHipster 6.0 GA版本并嵌入截屏视频。可以在[okta.github.io＃2869中](https://github.com/okta/okta.github.io/pull/2869)查看对此帖子的更改。



# 安装与配置

## 安装和配置nodejs

### 下载nodejs

https://nodejs.org/en/download/

### 安装nodejs到D:\nodejs

```
cd d:\nodejs
mkdir  node_global
mkdir node_cache
```

### 配置环境变量:

```
NODE_PATH=d:\nodejs\node_global
PATH=%NODE_PATH%; d:\nodejs;……
```

### 配置NPM环境变量：

```has
npm config set prefix "D:\nodejs\node_global"
npm config set cache "D:\nodejs\node_cache"
npm config set registry https://registry.npm.taobao.org
```

注意：这三个命令设置的信息会保存在在C:\Users\用户名\.npmrc文件中，你也可以直接修改这个文件

### 检查版本

```
npm -v
node -v
```

## 安装Jhipster

```
npm install -g generator-jhipster
```

（可选）

```
npm install -g rimraf     #我的环境中需要安装，否则在执行jhipster过程中报rimraf不是有效的命令错误
```

# 使用Jhipster

```shell
mkdir d:\work
cd work
jhipster
```

运行：

```
#分别在两个终端执行如下命令：
mvnw
npm start
```

401 not authorized

https://stackoverflow.com/questions/43453672/wso2-is-how-to-allow-anonymous-request-to-oidc-well-known-openid-configuration

http://dinukshaish.blogspot.com/2017/02/oauth-20-dynamic-client-registration.html



http://hasanthipurnima.blogspot.com/2017/03/oidc-discovery-endpoint.html

确保这个地址可以匿名访问：

https://is.cd.mtn:9443/oauth2/token/.well-known/openid-configuration



sp的回调URL：

regexp=(http://192.168.200.24:8080/login/oauth2/code/oidc|http://192.168.200.24:8080)



Post logout URI does not match with registered callback URI



https://is.cd.mtn:9443/authenticationendpoint/oauth2_error.do?oauthErrorCode=access_denied&oauthErrorMsg=Post+logout+URI+does+not+match+with+registered+callback+URI.&sp=oidctest&tenantDomain=carbon.super&sp=oidctest&tenantDomain=carbon.super



# 参考：

[jhipster - 标签 - 羽客 - 博客园](https://www.cnblogs.com/yorkwu/tag/jhipster/)

[JHipster 中文网](https://www.jhipster-cn.tech/)

[使用Java 12和JHipster 6更好，更快，更轻松的Java](https://developer.okta.com/blog/2019/04/04/java-11-java-12-jhipster-oidc)

[使用JHipster开发微服务和移动应用程序](https://www.pluralsight.com/courses/play-by-play-developing-microservices-mobile-apps-jhipster)