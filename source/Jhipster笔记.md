## `Jhipster`前后端分离

`jhipster --skip-client` 会只创建后段应用 (这就类似一个典型的微服务应用)`

`jhipster --skip-server` 将会创建前端应用

### 目录结构

后端目录结构：参考 [Maven 标准目录结构说明](https://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html)

前端目录结构，有两个目录你需要了解：

> `src/main/webapp` 是存放客户端应用代码的地方` 
>
> `target/www` 是客户端应用打包的地方

如果你的团队分为前端和后端，你有两种做法：

> 两个团队可以在一个项目上工作。由于目录是分开的，所以团队之间不会有太多冲突。为了使事情更干净，两个团队可以在不同的分支上工作。
>
> 前端代码可以存在一个单独的` Git` 项目里，然后以 `Git `子模块的形式导入后端项目。这样做需要删掉客户端打包脚本，这非常简单。

### HTTP 请求路由及缓存

> 一旦前端和后端分离，问题将是如何处理HTTP请求：
>
> 所有的API调用，将使用`/api` 前缀。如果使用Angular，可以在webpack.common.js配置文件中设置SERVER_API_URL常量。如："http://api.jhipster.tech:8081/"作为后端api服务的前缀。要注意CORS配置
>
> 调用`/`服务静态资产（从前端），浏览器不应缓存静态资产
>
> 对`/app`（包含客户端应用程序）和`/content`（包含静态内容，如images和css）的调用应缓存在产品中，因为这些资产是被hash的。

### 使用浏览器同步 `BrowserSync`

在开发模式下，hipster使用`browsersync`对前端应用程序进行热加载。`Browsersync`有一个代理，将请求从`/api`路由到后端服务（默认情况下，http://127.0.0.1:8080）

### 使用 CORS(Cross-origin request sharing)

JHipster 提供 out-of-the-box a CORS 配置:

> CORS 可以用 `jhipster.cors` 属性设置, 这里 有定义描述：[the JHipster common application properties](https://www.jhipster-cn.tech/common-application-properties/) 
>
> 对单体应用和网关应用，开发模式下缺省启用了CORS。默认情况下，它对微服务是禁用的，因为您应该通过网关访问它们。
>
> 出于安全原因，在生产环境中默认是关闭的

### 使用 `NGinx`

​      分离前端和后端代码的另一个解决方案是使用代理服务器。这在生产中很常见，一些团队在开发中也使用了这种技术。此配置将根据您的特定用例而更改，因此生成器无法自动执行此配置，下面是一个工作配置：

创建一个`Docker compose`文件`src/main/docker/nginx.yml`:

```yaml
version: '2'
services:
  nginx:
    image: nginx:1.13-alpine
    volumes:
    - ./../../../target/www:/usr/share/nginx/html
    - ./nginx/site.conf:/etc/nginx/conf.d/default.conf
    ports:
    - "8000:80"
```

这个`docker`映像将配置一个`nginx`服务器，从`target/www`读取静态资产：这是默认情况下,hipster生成j的前端应用代码的目录。在生产环境中，您可能有一个特定的文件夹。

同时它还读取`./nginx/site.conf`文件：这是`nginx`规范配置文件。下面是一个该文件的范例：

```nginx
server {
    listen 80;
    index index.html;
    server_name localhost;
    error_log  /var/log/nginx/error.log;

    location / {
        root /usr/share/nginx/html;
    }
    location /api {
        proxy_pass http://api.jhipster.tech:8081/api;
    }
    location /management {
        proxy_pass http://api.jhipster.tech:8081/management;
    }
    location /v2 {
       proxy_pass http://api.jhipster.tech:8081/v2;
    }
    location /swagger-ui {
        proxy_pass http://api.jhipster.tech:8081/swagger-ui;
    }
    location /swagger-resources {
        proxy_pass http://api.jhipster.tech:8081/swagger-resources;
    }
}
```

这种配置意味着:

> `Nginx`运行在80端口；
>
> 它从`/usr/share/nginx/html`目录读取静态资源；
>
> 它将作为从`/api`到 `http://api.jhipster.tech:8081/api`的代理

根据您的具体需要，此配置需要进行一些调整，但对于大多数应用程序来说，这应该是一个足够好的起点。

## 异常处理

`Jhipster`对错误处理有一流的支持：它提供错误页面和自定义机制来处理服务器端的业务和技术错误。

### 错误页面

`Jhipster`生成一个单页应用程序（SPA），但它仍然需要为不（或无法）访问该应用程序的用户自定义错误页。

### 动态错误页面

`jhipster`提供了一个通用的错误页面，它是一个`thymeleaf`模板，该页面位于：`src/main/resources/templates/error.html`。

### 静态404错误页面

`jhipster`提供了一个特定的静态404错误页面，位于`src/main/webapp/404.html`。默认情况下，`jhipster`不使用此页面：这里是用于在`jhipster`之前使用代理的项目（`apache/nginx/etc`），这样即使`jhipster`应用程序不可用，代理也可以显示404错误页面。

`注意`：它需要在前端代理上进行专门的配置。

### API错误

为了处理Spring MVC REST错误，`Jhipster`使用了 [Zalando’s Problem Spring Web library](https://github.com/zalando/problem-spring-web)库，以便提供丰富的、基于JSON的错误消息。

为了帮助最终用户，对于每个已知问题，此库将提供一个指向特定错误页的链接，该链接将提供更多详细信息。这些链接在`ErrorConstants`类中配置，并默认指向此网站。在您的应用程序中，您应该定制这些链接，并将它们指向您自己的API文档。

以下是可用的错误链接：

> - [Problem with message](https://www.jhipster-cn.tech/problem/problem-with-message)
> - [Constraint violation](https://www.jhipster-cn.tech/problem/constraint-violation)
> - [Problem with a parameterized message](https://www.jhipster-cn.tech/problem/parameterized)
> - [Entity not found](https://www.jhipster-cn.tech/problem/entity-not-found)
> - [Invalid password](https://www.jhipster-cn.tech/problem/invalid-password)
> - [E-mail already used](https://www.jhipster-cn.tech/problem/email-already-used)
> - [Login already used](https://www.jhipster-cn.tech/problem/login-already-used)
> - [E-mail not found](https://www.jhipster-cn.tech/problem/email-not-found)

## 使用Angular

### 工具

`Angular`使用的是`Typescript`而不是`Javascript`，因此需要一些特定的工具来有效地使用它。我们针对`Angular2+`应用程序的开发工作流程如下，如果您愿意，请使用`npm`而不是`yarn`。

> 生成应用程序时，将创建文件，并在生成结束时触发`npm install`任务。
>
> 一旦`npm install`任务完成，它调用`package.json`中的`postInstall`脚本，此步骤触发`webpack:build`任务。
>
> 现在，您应该选择基于构建工具（`maven`或`gradle`）生成并编译到目标或构建文件夹中的`www`文件夹中的所有文件。
>
> 现在就可以运行`./mvnw`或`./gradlew`来启动应用程序服务器，它应该在`localhost:8080`上可用，这也为根据上述步骤编译的客户端代码提供服务。
>
> 现在就可以在一个新的终端上运行`npm start`或`yarn start`，用`browsersync`启动`webpack dev`服务器。这将负责编译您的`typescript`代码，并自动重新加载您的浏览器。

要在浏览器中处理代码，我们建议使用 [Angular Augury](https://augury.angular.io/)，这样您就可以可视化路由并轻松调试代码。

### 前端项目的目录结构:

```
webapp
├── app                               - Your application
│   ├── account                       - User account management UI
│   ├── admin                         - Administration UI
│   ├── blocks                        - Common building blocks like configuration and interceptors
│   ├── entities                      - Generated entities (more information below)
│   ├── home                          - Home page
│   ├── layouts                       - Common page layouts like navigation bar and error pages
│   ├── shared                        - Common services like authentication and internationalization
│   ├── app.main.ts                   - Main application class
│   ├── app.module.ts                 - Application modules configuration
│   ├── app.route.ts                  - Main application router
├── content                           - Static content
│   ├── css                           - CSS stylesheets
│   ├── images                        - Images
├── i18n                              - Translation files
├── scss                              - Sass style sheet files will be here if you choose the option
├── swagger-ui                        - Swagger UI front-end
├── 404.html                          - 404 page
├── favicon.ico                       - Fav icon
├── index.html                        - Index page
├── robots.txt                        - Configuration for bots and Web crawlers
```

如果生成了实体，则结构中多了如下一些内容：

```
webapp
├── app
│   ├── entities
│       ├── foo                                    - CRUD front-end for the Foo entity
│           ├── foo.component.html                 - HTML view for the list page
│           ├── foo.component.ts                   - Controller for the list page
│           ├── foo.model.ts                       - Model representing the Foo entity
│           ├── foo.module.ts                      - Angular module for the Foo entity
│           ├── foo.route.ts                       - Angular Router configuration
│           ├── foo.service.ts                     - Service which access the Foo REST resource
│           ├── foo-delete-dialog.component.html   - HTML view for deleting a Foo
│           ├── foo-delete-dialog.component.ts     - Controller for deleting a Foo
│           ├── foo-detail.component.html          - HTML view for displaying a Foo
│           ├── foo-detail.component.ts            - Controller or displaying a Foo
│           ├── foo-dialog.component.html          - HTML view for editing a Foo
│           ├── foo-dialog.component.ts            - Controller for editing a Foo
│           ├── foo-popup.service.ts               - Service for handling the create/update dialog pop-up
│           ├── index.ts                           - Barrel for exporting everything
├── i18n                                           - Translation files
│   ├── en                                         - English translations
│   │   ├── foo.json                               - English translation of Foo name, fields, ...
│   ├── zh                                         - 中文 translations
│   │   ├── foo.json                               - 中文 translation of Foo name, fields, ...
```

请注意，默认语言翻译将基于您在应用程序生成期间选择的内容。此处显示的“en”和“fr”仅供演示

### 访问控制

`Jhipster`使用 [the Angular router](https://angular.io/docs/ts/latest/guide/router.html)来组织客户端应用程序的不同部分。

对于每种状态，所需权限列在该状态的数据中，当权限列表为空时，表示可以匿名访问该状态。

权限也在类`AuthoritiesCostants.java`的服务器端定义，并且在逻辑上，客户机和服务器端权限应该相同。

在下面的示例中，“sessions”状态设计为仅由具有角色“ROLE_USER”的已验证用户访问：

```java
export const sessionsRoute: Route = {
    path: 'sessions',
    component: SessionsComponent,
    data: {
        authorities: ['ROLE_USER'],
        pageTitle: 'global.menu.account.sessions'
    },
    canActivate: [UserRouteAccessService]
};
```

一旦在路由器中定义了这些权限，它们可以通过`jhiHasAnyAuthority`指令在其基于参数类型2个变体中使用：

- 对于单个字符串，该指令仅在用户具有所需权限时显示HTML组件
- 对于字符串数组，如果用户具有列出的权限之一，则该指令将显示HTML组件

例如：

以下文本将仅显示给具有“ROLE_ADMIN”角色的用户：

```html
html<h1 *jhiHasAnyAuthority="'ROLE_ADMIN'">Hello, admin user</h1>
```

以下文本将仅显示给具有“ROLE_ADMIN”或“ROLE_USER”角色之一的用户：

```html
<h1 *jhiHasAnyAuthority="['ROLE_ADMIN', 'ROLE_USER']">Hello, dear user</h1>
```

请注意，这些指令只在客户端显示或隐藏HTML组件，您还需要在服务器端保护您的代码！

### `Ng Jhipster`库

`ng jhipster`库包含`Angular2+`应用程序使用的实用功能和通用组件。它们包括:

- 验证指令
- 国际化组件
- 常用的管道，如大写、排序和字截断
- base64、日期和分页处理服务
- 通知系统（见下文）

### 通知系统

`Jhipster`使用一个定制的通知系统将事件从服务器端发送到客户端，并具有`I18N`功能的`JhiAlertComponent`和`JhiAlertErrorComponent`组件，这些组件可以在生成的应用程序中使用。

默认情况下，当从HTTP响应捕获错误时，`Jhipster`将显示错误通知。

success、info、warning和error的默认超时为5秒，可以配置为：

```javascript
this.alerts.push(
    this.alertService.addAlert(
        {
            type: 'danger',
            msg: 'you should not have pressed this button!',
            timeout: 5000,
            toast: false,
            scoped: true
        },
        this.alerts
    )
);
```

## 使用`React`（带`Redux`）

如果您对我们的应用程序结构、文件名、打印脚本约定有任何疑问，请先阅读本指南…

对于`react`路由，我们遵循破折号命名约定，这样URL就干净一致了。当您生成一个实体时，路由名称、路由`URL`和`REST API`端点`URL`将根据此约定生成，并且在需要时，实体名称也将自动为复数。

### 项目结构：

```
webapp
├── app                             - Your application
│   ├── config                      - General configuration (redux store, middleware, etc.)
│   ├── entities                    - Generated entities
│   ├── modules                     - Main components directory
│   │   ├── account                 - Account related components
│   │   ├── administration          - Administration related components
│   │   ├── home                    - Application homepage
│   │   └── login                   - Login related components
│   ├── shared                      - Shared elements such as your header, footer, reducers, models and util classes
│   ├── app.scss                    - Your global application stylesheet if you choose the Sass option
│   ├── app.css                     - Your global application stylesheet
│   ├── app.tsx                     - The application main class
│   ├── index.tsx                   - Index script
│   ├── routes.tsx                  - Application main routes
│   └── typings.d.ts                -
├── i18n                            - Translation files
├── static                          - Contains your static files such as images and fonts
├── swagger-ui                      - Swagger UI front-end
├── 404.html                        - 404 page
├── favicon.ico                     - Fav icon
├── index.html                      - Index page
├── manifest.webapp                 - Application manifest
└── robots.txt                      - Configuration for bots and Web crawlers
```

如果生成了实体，则结构中多了如下一些内容：

```
webapp
├── app                                        
│   └── entities
│       ├── foo                           - CRUD front-end for the Foo entity
│       │   ├── foo-delete-dialog.tsx     - Delete dialog component
│       │   ├── foo-detail.tsx            - Detail page component
│       │   ├── foo-dialog.tsx            - Creation dialog component
│       │   ├── foo.reducer.ts            - Foo entity reducer
│       │   ├── foo.tsx                   - Entity main component
│       │   └── index.tsx                 - Entity main routes
│       └── index.tsx                     - Entities routes    
└── i18n                                  - Translation files
     ├── en                               - English translations
     │   ├── foo.json                     - English translation of Foo name, fields, ...
     └── zh                               - 中文 translations
         └── foo.json                     - 中文 translation of Foo name, fields, ...
```

### `Redux`

[Redux](https://redux.js.org/) 是一个可预测的`JavaScript`状态容器。它与`react`一起用于管理`react`组件的状态。

基本上，`Redux`提供了一个对象存储，用于存储应用程序的整个状态。要访问此存储并因此更新状态组件，唯一的方法是派发出描述请求更新的事实的**actions**，然后**reducers**将定义如何响应这些操作更新状态。

下面是一个reducers的示例代码：

```typescript
export const ACTION_TYPES = {
  FETCH_FOOS: 'foo/FETCH_FOOS',
};

const initialState = {
  loading: false,
  foos: [],
  updateSuccess: false,
  updateFailure: false
};

// Reducer
export default (state = initialState, action) => {
  switch (action.type) {
    case REQUEST(ACTION_TYPES.FETCH_FOOS):
      return {
        ...state,
        updateSuccess: false,
        updateFailure: false,
        loading: true
      };
    case FAILURE(ACTION_TYPES.FETCH_FOOS):
      return {
        ...state,
        loading: false,
        updateSuccess: false,
        updateFailure: true
      };
    case SUCCESS(ACTION_TYPES.FETCH_FOOS):
      return {
        ...state,
        loading: false,
        updateSuccess: true,
        updateFailure: false,
        foos: action.payload.data
      };
    default:
      return state;
  }
};
```

为了访问存储并更新当前应用程序状态，如前所述，您需要将action派发到存储区。action是简单的`javascript`对象，必须具有**type**，它描述了action将要执行的操作，通常它们还具有一个**payload**，该**payload**与要传递到存储区的数据相对应。

下面是一个访问存储的action：

```javascript
const apiUrl = SERVER_API_URL + '/api/foos';

// Action
export const getFoos = () => ({
  type: ACTION_TYPES.FETCH_FOOS,
  payload: axios.get(apiUrl)
});
```

上面描述的action表明我们希望通过发送GET请求来检索所有foo对象。操作类型将匹配注意，导出关键字用于在必要时（例如，每次更新组件时）使连接的组件能够使用该操作。

### 访问控制

`Jhipster`使用 [React router](https://github.com/ReactTraining/react-router)来组织应用程序的不同部分。

当涉及到需要身份验证的路由时，将使用生成的`PrivateRoute`组件。此组件只会阻止任何未经身份验证的用户访问路由。

下面是一个`PrivateRoute`的例子：

```typescript
const Routes = () => (
  <div className="view-routes">
    <Route exact path="/" component={Home} />
    <Route path="/login" component={Login} />
    <PrivateRoute path="/account" component={Account} />
  </div>
);
```

如您所见，未经身份验证的用户可以访问`/`和`/login`，但访问`/account`则需要首先登录。

请注意，`Privateroute`使用`authentication.isAuthenticated `存储值来确定用户是否经过了身份验证。

### 通知系统

`Jhipster`使用了 [react-toastify](https://github.com/fkhadra/react-toastify)作为通知系统。

默认情况下，每当创建/更新/删除实体时，`Jhipster`将显示成功通知；当从响应中捕获错误时，将显示错误通知。

## 定制Bootstrap4

自定义`Jhipster`应用程序外观的最简单方法是重写CSS样式文件`src/main/webapp/content/css/global.css`，或者如果您选择了SASS选项，就需要重写`src/main/webapp/content/scss/global.scss`文件

使用sass比简单的css更容易、更简洁、更强大，因为引导程序也是用sass编写的。

如果您想在自己的SCSS文件中使用Bootstrap [partials](http://sass-lang.com/guide) ，那么可以像下面在SCSS文件的开头导入它。例如，要使用圆角混合：

```scss
@import "node_modules/bootstrap/scss/variables";
@import "node_modules/bootstrap/scss/mixins/border-radius";
```

确保只导入partials而不导入主SASS文件，否则将生成可能导致问题的重复CSS。

要更改默认Bootstrap设置（如颜色、边框半径等），请添加或更改partial文件(`src/main/webapp/content/scss/_bootstrap-variable.scss`)中属性的值。

`Bootstrap  _variables.scss`中定义的所有值都可以在此处被重写。

## 使用TLS和HTTP/2

### Spring Boot中使用TLS和HTTP/2

为了使用提供的自签名证书运行`jhipster`，并且启用了`tls`和`http/2`，您只需要使用此`tls`配置文件：

```
./mvnw -Pdev,tls
或
./gradlew -Ptls
```

该应用程序将在https://localhost:8080/上提供。

由于证书是自签名的，浏览器将发出警告，您需要忽略它（或导入它）才能访问应用程序。

### 在Angular 或 React上使用TLS和HTTP/2

使用`npm run start-tls`而不是`npm start`来启动

## 插件和蓝图

有100个插件或蓝图，包括[Vuejs](https://www.jhipster-cn.tech/modules/marketplace/)、[Leaflet](https://www.jhipster-cn.tech/modules/marketplace/) 、[Docker](https://www.jhipster-cn.tech/modules/marketplace/)、、[Leafletmap](https://www.jhipster-cn.tech/modules/marketplace/)、[Database Backup](https://www.jhipster-cn.tech/modules/marketplace/)、[Swagger Api First](https://www.jhipster-cn.tech/modules/marketplace/)、[@xiges/siddhi](https://www.jhipster-cn.tech/modules/marketplace/)、[Social Login Api](https://www.jhipster-cn.tech/modules/marketplace/) 等

# JDL 语法

JDL 是 JHipster 特质的 domain 语言，提供了能在一个文件（或多个）中来描述整个应用、部署、实体对象及其关系的能力，并且只需要很简单的、友好的语法。

> 可以使用我们的在线工具 [JDL-Studio](https://start.jhipster.tech/jdl-studio/) 或者插件 [JHipster IDE](https://www.jhipster.tech/jhipster-ide/) ，支持多个平台： [Eclipse](https://marketplace.eclipse.org/content/jhipster-ide), [VS Code](https://marketplace.visualstudio.com/items?itemName=jhipster-ide.jdl) 以及 [Atom](https://atom.io/packages/ide-jhipster) 来创建 JDL 及其 UML 视图。你还可以创建、导出、或分享你的模型。
>
> 一旦你创建了项目 (用 `jhipster import-jdl` 或者 `jhipster` 命令工具创建), 接下来你可以使用命令 `jhipster import-jdl your-jdl-file.jdl` 来从 JDL 文件生成实体对象 (确保在` JHipster `项目目录下执行)。
>
> 你还能用 [JHipster UML](https://www.jhipster-cn.tech/jhipster-uml/) 来创建实体对象或引用并导出为 JDL 文件，执行 `jhipster-uml your-xmi-file.xmi --to-jdl` (确保在 JHipster 项目目录下执行)。要了解更多关于如何安装和使用 JHipster UML, 访问 [JHipster UML documentation](https://www.jhipster-cn.tech/jhipster-uml/).
>
> 这可以作为 [entity sub-generator](https://www.jhipster-cn.tech/creating-an-entity/) 工具的替代，并且是推荐这样做的。 核心思想是使用图形工具比使用传统的 Yeoman 问答来 [管理关联关系](https://www.jhipster-cn.tech/managing-relationships/) 要简单的多。
>
> JDL 项目在 [GitHub 地址](https://github.com/jhipster/jhipster-core/), 它和` JHipster `一样是开源的并且遵循 Apache 2.0 开源协议

##  Oracle 的 “人力资源” 样例程序的JDL：

```java
//
// This is the entity model for for the Oracle Humar Resources sample
//

entity Region {
	regionName String
}

entity Country {
	countryName String
}

// an ignored comment
/** not an ignored comment */
entity Location {
	streetAddress String,
	postalCode String,
	city String,
	stateProvince String
}

entity Department {
	departmentName String required
}

/**
 * Task entity.
 * @author The JHipster team.
 */
entity Task {
	title String,
	description String
}

/**
 * The Employee entity.
 */
entity Employee {
	/**
	* The firstname attribute.
	*/
	firstName String,
	lastName String,
	email String,
	phoneNumber String,
	hireDate Instant,
	salary Long,
	commissionPct Long
}

entity Job {
	jobTitle String,
	minSalary Long,
	maxSalary Long
}

entity JobHistory {
	startDate Instant,
	endDate Instant,
	language Language
}

enum Language {
    FRENCH, ENGLISH, SPANISH
}

relationship OneToOne {
	Country{region} to Region
}

relationship OneToOne {
	Location{country} to Country
}

relationship OneToOne {
	Department{location} to Location
}

relationship ManyToMany {
	Job{task(title)} to Task{job}
}

// defining multiple OneToMany relationships with comments
relationship OneToMany {
	Employee{job} to Job,
	/**
	* A relationship
	*/
	Department{employee} to
	/**
	* Another side of the same relationship
	*/
	Employee
}

relationship ManyToOne {
	Employee{manager} to Employee
}

// defining multiple oneToOne relationships
relationship OneToOne {
	JobHistory{job} to Job,
	JobHistory{department} to Department,
	JobHistory{employee} to Employee
}

// Set pagination options
paginate JobHistory, Employee with infinite-scroll
paginate Job with pagination

// Use Data Transfert Objects (DTO)
// dto * with mapstruct

// Set service options to all except few
service all with serviceImpl except Employee, Job

// Set an angular suffix
// angularSuffix * with mySuffix
```

## 如何使用

### 你可以使用 JDL 文件来生成实体对象：

- 创建一个后缀为 ‘.jh’ 或 ‘.jdl’ 的文件，
- 声明应用、部署、实体对象和关联关系，或者用 [JDL-Studio](https://start.jhipster.tech/jdl-studio/) 或者 [JHipster IDE](https://www.jhipster.tech/jhipster-ide/) 开发下载该文件，
- 如果你创建了实体对象，就在应用的根目录中，执行： `jhipster import-jdl my_file.jdl`
- 如果你创建的是应用，就在一个目录里执行： `jhipster import-jdl my_file.jdl`

如果你不想创建实体对象类，而只是需要 JSON 文件，你可以在导入时加上参数：`--json-only`，这样就只会在 `.jhipster` 目录里创建 json 文件，而不会创建那些实体对象了。

```shell
jhipster import-jdl ./my-jdl-file.jdl --json-only
```

默认情况下，`import-jdl` 命令只会生成发生了改变的实体对象, 如果你需要所有的实体对象都重新生成一遍， 可以加上参数：`--force`。 请注意这会覆盖你在这些对象上做的所有变更。

```
jhipster import-jdl ./my-jdl-file.jdl --force
```

如果你希望在项目内使用，你可以执行:

- NPM: `npm install jhipster-core --save`
- Yarn: `yarn add jhipster-core`

来将之安装在本地，并保存在你的 `package.json` 文件里。

### 声明实体对象

实体对象的声明如下格式：

```
entity <entity name> {
  <field name> <type> [<validation>*]
}
```

- `<entity name>` 是实体对象的名称，
- `<field name>` 实体对象的属性，
- `<type>` JHipster 支持的属性类型，
- 可选的 `<validation>` 类型的校验规则。

所有的属性类型和校验规则在 [这里](https://www.jhipster-cn.tech/jdl/#annexes)， 如果校验规则需要一个值，还要加上 `(<value>)` 在校验规则的右边。

一些 JDL 的例子：

```
entity A
entity B
entity C
entity D {
  name String required
  address String required maxlength(100)
  age Integer required min(18)
}
```

正则表达式的写法稍微特别些 (v1.3.6 版本开始支持):

```
entity A {
  myString String required minlength(1) maxlength(42) pattern(/[A-Z]+/)
}
```

如果你在使用 v4.9.X 之前的版本，你得这样使用正则表达式：`pattern('[A-Z]+')`.

JDL 设计的易用可读，如果你的实体是空的（没有属性），你可以这样声明：`entity A` 或 `entity A {}`。

请注意 JHipster 会增加一个默认的 `id` 字段，所以你可以不用管它。

### 声明关联关系

关联关系的声明如下格式：

```
relationship (OneToMany | ManyToOne | OneToOne | ManyToMany) {
  <from entity>[{<relationship name>[(<display field>)]}] to <to entity>[{<relationship name>[(<display field>)]}]
}
```

- `(OneToMany | ManyToOne| OneToOne | ManyToMany)` 是关系的类型，
- `<from entity>` 是关系所有者的名称、来源，
- `<to entity>` 是关系所指向的目标，
- `<relationship name>` 是关系在另一边对象内属性的名称，
- `<display field>` 是关系在选择列表中显示的属性字段 (默认是: `id`),
- `required` 是指属性是否是必填的。

这是个例子：

```
entity Book
entity Author

relationship ManyToOne {
  Book to Author
}
```

让我们来做的更复杂点。 一本书，有一个必填的作者，一个作者，有多本书。

```
entity Book
entity Author {
  name String required
}

relationship OneToMany {
  Author{book} to Book{writer(name) required}
}
```

在这个 `Book` 例子中，它有一个 **必填** 字段 `writer` 关联到 `Author` 的 `name` 属性。

当然，在真实场景中，你会需要些一堆的关系并且不断重复写上面那三行代码感觉有点傻。 所以，你可以这样合并了声明：

```
entity A
entity B
entity C
entity D

relationship OneToOne {
  A{b} to B{a}
  B{c} to C
}
relationship ManyToMany {
  A{d} to D{a}
  C{d} to D{c}
}
```

这些关系一般使用 `id` 字段作为关联属性，同时它也是在前端编辑这个关系时默认显示的字段。如果你希望显示其他的字段，可以这样设置： （译注：这里应该指显示和关联都用这个字段，需要唯一；如果只需要显示，还得手动修改 global 里的 other 字段重新生成）

```
entity A {
  name String required
}
entity B


relationship OneToOne {
  A{b} to B{a(name)}
}
```

这样的话 JHipster 会产生一组 REST 资源到前端并包含 `id` 和 `name` 字段，这样 name 属性就可以正常的显示给用户了。

### 枚举

在 JDL 里声明枚举类型的语法：

- 声明枚举：

  ```
    enum Language {
      FRENCH, ENGLISH, SPANISH
    }
  ```

- 在实体对象中，使用枚举作为字段的属性：

  ```
    entity Book {
      title String required,
      description String,
      language Language
    }
  ```

### Blob 类型 (byte[])

JHipster 提供了用户选择图像类型或者二进制类型。JDL 同样支持： 需要在编辑器中创建一个客制化类型 (参考数据类型章节)，用以下规约来命名：

- `AnyBlob` 或 `Blob` 类型来创建适合任意二进制类型的字段；
- `ImageBlob` 类型来创建用于指代图片的字段；
- `TextBlob` 类型创建 CLOB (长文本)字段。

你可以创建任意多类型。

### 声明的可选项

JHipster 中，你可以定义额外的选项来描述类似翻页或 DTO 等功能。 你可以这样：

```
entity A {
  name String required
}
entity B
entity C

dto A, B with mapstruct

paginate A with infinite-scroll
paginate B with pagination
paginate C with pager  // pager is only available in AngularJS

service A with serviceClass
service C with serviceImpl
```

关键字 `dto`, `paginate`, `service` 还有 `with` 来支持这些特性。 如果定义了错误的选项，JDL 有一个红色消息的通知并将之忽略在 JHipster 的 JSON 文件之外。

#### Service 选项

不定义 Service 将会让创建的资源类直接调用 repository 接口。 这个是默认选项，参考下面的 A 对象。 定义 Service 并且配置 serviceClass (参考 B 对象) 将会让资源类调用 service 类，service 类再调用 repository 接口。 定义 Service 并配置 serviceImpl (参考 C) 将会让资源类调用一个 service 接口。 该接口会有一个实现类，实现类再调用 repository 接口。

如果你不清楚使用哪种，就不要定义 service，这是最简单的 CRUD 实现方式。 如果你的业务逻辑非常复杂且调用多个 repository，实现一个 serviceClass 是理想方式。 Jhipster 不喜欢不必要的接口方式，不过如果你喜欢你还是可以实现他们。

```
entity A
entity B
entity C

// no service for A
service B with serviceClass
service C with serviceImpl
```

JDL 还支持批量选项设置。可以这么做：

```
entity A
entity B
...
entity Z

dto * with mapstruct
service all with serviceImpl
paginate C with pagination
```

请注意 `*` 和 `all` 是等价的。 最近的版本还支持排除 (在给所有实体对象设置选项时非常有用):

```
entity A
entity B
...
entity Z

dto * with mapstruct except A
service all with serviceImpl except A, B, C
paginate C with pagination
```

你还可以告诉 JHipster，你不需要创建客户端代码、或者服务端代码。 甚至你需要给 Angular 相关的文件增加前缀。（译注：为什么要给 Angular 代码增加前缀？译者试过设置这个前缀为空，结果引起一堆命名冲突，会有关键字什么的） [Filtering](https://www.jhipster.tech/entities-filtering/) options can be activated on a per entity basis: filter `<entity name>` or for all entities: filter `*`. 在 JDL 中，这样定义：

```
entity A
entity B
entity C

skipClient A
skipServer B
angularSuffix * with mySuperEntities
filter C
```

最后，可以定义表名（默认使用实体对象的名称）：

```
entity A // A 就是表名
entity B (the_best_entity) // 在这里，the_best_entity 是表名
```

### 微服务相关选项

从 JHipster v3 开始，可以创建微服务了。 你可以定义一些选项来生成实体对象：微服务名称，和搜索引擎。

这样定义微服务名称 (就是 JHipster 应用的名称):

```
entity A
entity B
entity C

microservice * with mysuperjhipsterapp except C
microservice C with myotherjhipsterapp
search * with elasticsearch except C
```

第一个选项告诉 JHipster 你希望微服务来处理你的实体对象， 第二个选项定义你的实体对象是否会被搜索到。

### 注解

从 JHipster v5 版本开始支持使用注解。注解和 Java 中的使用方式类似，annotations work the same way so that 你可以给你的实体对象添加注解了。

假设有个这样的 JDL：

```
entity A
entity B
entity C

dto C with mapstruct
paginate * with pager except C
search A with elasticsearch
```

和以下使用注解方式等价：

```
@paginate(pager)
@search(elasticsearch)
entity A

@paginate(pager)
entity B

@dto(mapstruct)
entity C
```

看上去会比移除的代码添加更多，但是这的确对项目有帮助，尤其是使用微服务时使用多个 JDL 文件时。

### 声明部署

从版本 v3.6.0 开始，可以在 JDL 中声明部署了 (兼容 JHipster v5.7 以上).

*To import one or several deployments, you need not be in a JHipster application folder.*

The most basic declaration is done as follows:

```
deployment {
  deploymentType docker-compose
  appsFolders [foo, bar]
  dockerRepositoryName "yourDockerLoginName"
}
```

A JHipster deployment has a config with default values for all other properties and using the previous syntax will ensure your deployment will use the default values (as if you didn’t make any specific choice). The resulting deployment will have:

- deploymentType: `docker-compose`
- appsFolders: `foo, bar`
- dockerRepositoryName: `yourDockerLoginName`
- serviceDiscoveryType: `eureka`
- gatewayType: `zuul`
- directoryPath: `../`
- etc.

Now, if you want some custom options:

```
deployment {
  deploymentType kubernetes
  appsFolders [store, invoice, notification, product]
  dockerRepositoryName "yourDockerLoginName"
  serviceDiscoveryType no
  istio autoInjection
  istioRoute true
  kubernetesServiceType Ingress
  kubernetesNamespace jhipster
  ingressDomain "jhipster.192.168.99.100.nip.io"
}
```

Those options are only a sample of what’s available in the JDL. The complete list of options is available in the annexes, [here](https://www.jhipster-cn.tech/jdl/#annexes).

If you want more than one deployment, here’s how you do it:

```
// will be created under 'docker-compose' folder
deployment {
  deploymentType docker-compose
  appsFolders [foo, bar]
  dockerRepositoryName "yourDockerLoginName"
}

// will be created under 'kubernetes' folder
deployment {
  deploymentType kubernetes
  appsFolders [foo, bar]
  dockerRepositoryName "yourDockerLoginName"
}
```

You can have one deployment per deploymentType. The applications defined in `appsFolders` should be in the same folder where you are creating deployments or in the folder defined in `directoryPath`. For example for above you need to have a folder structure like this

```
.
├── yourJdlFile.jdl
├── foo
├── bar
├── kubernetes // will created by the JDL
└── docker-compose // will created by the JDL
```

## 注释 & Javadoc

在 JDL 文件中还可以增加 Javadoc 和注释。 和在 Java 中写法一致，Just like in Java, 下面的例子展示了如何添加 Javadoc 类型注释：

```
/**
 * Class comments.
 * @author The JHipster team.
 */
entity MyEntity { // another form of comment
  /** A required attribute */
  myField String required
  mySecondField String // another form of comment
}

/**
 * Second entity.
 */
entity MySecondEntity {}

relationship OneToMany {
  /** This is possible too! */
  MyEntity{mySecondEntity}
  to
  /**
   * And this too!
   */
  MySecondEntity{myEntity}
}
```

这些注释随后会添加为 Javadoc 注释。

JDL 还会这样处理注释：

```
// 被忽略的注释
/** 不被忽略的注释 */
```

所以，任何以 `//` 开头的内容都是 JDL 内部注释，不会产生 Javadoc。

请注意 JDL Studio 中以 `#` 开头的指令在解析时会被忽略掉。 （译注：# entity A？不就等于 // 么？）

还有一种注释形式：

```
entity A {
  name String /** My super field */
  count Integer /** My other super field */
}
```

A 类型的 name 属性会注释为 `My super field`, B 是 `My other super field`. 备注不是必要的，但是认真写注释才是明智之举。 如果你希望混合使用逗号和注释，小心！

```
entity A {
  name String, /** My comment */
  count Integer
}
```

（译注：刚发现上面的例子，不要逗号也可以） A 的 name 属性不会有注释，count 有。

## 关联关系详解

这里解释如果用 JDL 创建关联关系。

### 一对一（One-to-One）

双向关系，Car 有一个 Driver, Driver 有一个 Car.

```
entity Driver
entity Car
relationship OneToOne {
  Car{driver} to Driver{car}
}
```

单向关系，Citizen 有一个 Passport, 但是 Passport 关联不到他的所有者。

```
entity Citizen
entity Passport
relationship OneToOne {
  Citizen to Passport
}
```

### 一对多（One-to-Many）

双向关系，Owner 有一个、或没有、或多个 Car 对象，Car 关联它的所有者：

```
entity Owner
entity Car
relationship OneToMany {
  Owner{car} to Car{owner}
}
```

单向关系，JHipster 还不支持，可以这样标识： （关于为什么不支持，参考另一个 关联关系 章节）

```
entity Owner
entity Car
relationship OneToMany {
  Owner to Car
}
```

### 多对一（Many-to-One）

和上面一对多（One-to-Many）相反的关系一样的写法。 （译注：应该是 one-to-many，many-to-one 都可以写的意思） 单向关系，只有 Car 关联它的所有者：

```
entity Owner
entity Car
relationship ManyToOne {
  Car to Owner
}
```

### 多对多（Many-to-Many）

最后，Car 和 Driver 的多对多关系，可以互相访问：

```
entity Driver
entity Car
relationship ManyToMany {
  Car{driver} to Driver{car}
}
```

请注意：关系的管理方（owning side）需要写在定义的左边！

## JDL支持的常量

从 JHipster Core v1.2.7 版本开始，JDL 支持数字型常量。 例子：

```
DEFAULT_MIN_LENGTH = 1
DEFAULT_MAX_LENGTH = 42
DEFAULT_MIN_BYTES = 20
DEFAULT_MAX_BYTES = 40
DEFAULT_MIN = 0
DEFAULT_MAX = 41

entity A {
  name String minlength(DEFAULT_MIN_LENGTH) maxlength(DEFAULT_MAX_LENGTH)
  content TextBlob required
  count Integer min(DEFAULT_MIN) max(DEFAULT_MAX)
}
```