

[TOC]

# 一、快速上手

## 1、环境

- 安装 nodejs

- 安装 nrm ,设置使用本地私服

  ```shell
  npm install -g nrm
  nrm ls
  nrm add local http://192.168.3.69:8081/repository/npm-public/
  nrm use local
  ```

- 安装 Git

- 安装VS CODE

- 安装create-react-app

  ```
  npm install -g create-react-app
  ```

- 安装`generator-jhipster`

  如果你期望使用集成度更好的脚手架工具，可安装这个（一般来说还需安装maven)

  ```
  npm install -g generator-jhipster
  npm install -g yo
  ```

## 2、推荐学习课程

[TypeScript 入门教程](https://ts.xcatliu.com/)

[视频： Lison老师的TypeScript完全解读]([https://segmentfault.com/ls/1650000018455856/l/1500000018451292)

[TypeScript Handbook（中文版）](https://zhongsp.gitbooks.io/typescript-handbook/content/)

[在线测验一下你的TS水平](https://www.tutorialsteacher.com/online-test/typescript-test)

[视频：React 教程：编写 TodoList 功能，React16.4 快速上手教程-慕课网](https://www.imooc.com/video/17570)

[超级棒的英文视频教程:React和TS](https://egghead.io/lessons/react-why-use-typescript-with-react)

<font color='red'>注意：看了英文版的视频教程，就知道自己编程有多菜了</font>

## 3、创建新项目

```shell
# 创建工作目录，建议你的不同语言的项目代码都统一放到ws文件夹中
cd /D E:\ws
# 创建react-ts目录，建议按照技术架构来存放项目
mkdir react-ts
cd react-ts
# 使用创建基于typescript的react项目
npx create-react-app my-app-by-npx --typescript
cd my-app-by-npx
# Adds the type definitions
npm install --save typescript @types/node @types/react @types/react-dom @types/jest
# 使用vs code 打开当前项目
code .
#打开集成命令行终端
Ctrl + Shift + ` 
npm start
npm test
```

## 4、安装测试依赖包

```shell
npm install -D enzyme @types/enzyme enzyme-adapter-react-16 @types/enzyme-adapter-react-16
```

## 5、安装Redux，添加state管理

```shell
npm install -S redux react-redux @types/react-redux
```

# 二、VS CODE 使用技巧

## 1、VS CODE 高手使用的快捷键

```shell
Ctrl + Shift + ` 打开集成命令行终端
ALt + z 自动折行开关
Alt + up/down 上下移动当前行
Shift + Alt up/down上下复制当前行  或上下复制选择的多行
shift + ctrl + k  删除当前行
Ctrl + Enter 在当前行下插入新的一行
Ctrl + Enter 在当前行下插入新的一行
Ctrl + Shift + V 预览Markdown文件
alt+shift+A 多行注释
shift + alt +f 代码格式化

F12 打开定义，ESC取消 
Alt + 组件上点击打开组件定义
Alt+Shift+鼠标左键，Ctrl+Alt+Down/Up  多行编辑(列编辑)：
Ctrl+Shift+L 同时选中所有匹配，可用来批量重命名
Ctrl+D 下一个匹配的也被选中，可用来批量重命名
Ctrl+Shift+K 删除当前行
Ctrl+U 返回上一次光标位置：
```

## 2、VS CODE 高手常用的插件

```shell
#让vscode资源管理器目录是哪个加图标装饰、必备
Vscode-icons
# 逆天的基于机器学习的智能代码提示
TabNine
# 打开Markdown文件时，自动打开Markdown预览窗口
Auto-Open Markdown Preview
```

## 3、`react-typescrip`代码辅助编写插件：

```shell
# 这个插件很牛，自动完成的代码符合本文第三部分的编码规范。在新的tsx或jsx文件中输入：rc或rs或rp或pt，上下箭头选择合适的代码模板，按TAB看效果
Airbnb react snippets
#在新的tsx文件中输入，tsrc或tsrcc，按TAB看效果 
Typescript React code snippets
#：自动导入组件
Auto Import - ES6, TS, JSX, TSX
# 在VS资源管理器窗口快速创建组件的右键菜单，不支持ts
React Typescript Toolbox
# 选择部分jsx代码，点击小灯泡，选择合适的重构操作
VSCode React Refactor 代码重构插件
```

# 三、React/JSX 编码规范(Airbnb)

*算是最合理的React/JSX编码规范之一了*

此编码规范主要基于目前流行的JavaScript标准，尽管某些其他约定(如async/await，静态class属性)可能在不同的项目中被引入或者被禁用。目前的状态是任何stage-3之前的规范都不包括也不推荐使用。

##  1、基本规则

- 每个文件只写一个组件.
  - 但是多个[无状态组件](https://facebook.github.io/react/docs/reusable-components.html#stateless-functions)可以放在单个文件中. eslint: [`react/no-multi-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-multi-comp.md#ignorestateless).
- 推荐使用JSX语法.
- 不要使用 `React.createElement`，除非从一个非JSX的文件中初始化你的app.

## 2、创建组件（Class vs React.createClass vs stateless）

- 如果你的组件有内部状态或者是`refs`, 推荐使用 `class extends React.Component` 而不是 `React.createClass`. eslint: [`react/prefer-es6-class`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-es6-class.md) [`react/prefer-stateless-function`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-stateless-function.md)


```jsx
// 不好
const Listing = React.createClass({
  // ...
  render() {
    return <div>{this.state.hello}</div>;
  }
});

// 好
class Listing extends React.Component {
  // ...
  render() {
    return <div>{this.state.hello}</div>;
  }
}
```

- 如果你的组件没有状态或是没有引用`refs`， 推荐使用普通函数（非箭头函数）而不是类:


```jsx
// 不好
class Listing extends React.Component {
  render() {
    return <div>{this.props.hello}</div>;
  }
}

// 不好 (relying on function name inference is discouraged)
const Listing = ({ hello }) => (
  <div>{hello}</div>
);

// 好
function Listing({ hello }) {
  return <div>{hello}</div>;
}
```

## 3、Mixins

- [不要使用 mixins](https://facebook.github.io/react/blog/2016/07/13/mixins-considered-harmful.html).

> 为什么?
>
> Mixins 会增加隐式的依赖，导致命名冲突，并且会以雪球式增加复杂度。在大多数情况下Mixins可以被更好的方法替代，如：组件化，高阶组件，工具组件等。

## 4、命名规约

- **扩展名**: React组件使用 `.jsx` 扩展名.

- **文件名**: 文件名使用PascalCase首字母大写命名. 如, `ReservationCard.jsx`.

- **引用命名**: React组件名使用PascalCase命名，实例使用驼峰式命名(首字母小写). eslint: [`react/jsx-pascal-case`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-pascal-case.md)

  ```jsx
  // 不好
  import reservationCard from './ReservationCard';
  
  // 好
  import ReservationCard from './ReservationCard';
  
  // 不好
  const ReservationItem = <ReservationCard />;
  
  // 好
  const reservationItem = <ReservationCard />;
  ```

- **组件命名**: 组件使用与当前文件名一样的名称. 比如 `ReservationCard.jsx` 应该包含名为 `ReservationCard`的组件. 但是，如果整个文件夹是一个组件，使用 `index.js`作为入口文件，文件夹名作为组件的名称:

  ```jsx
  // 不好
  import Footer from './Footer/Footer';
  
  // 不好
  import Footer from './Footer/index';
  
  // 好
  import Footer from './Footer';
  ```

- **高阶组件命名**: 对于生成一个新的组件，其中的组件名 `displayName` 应该为高阶组件名和传入组件名的组合. 例如, 高阶组件 `withFoo()`, 当传入一个 `Bar` 组件的时候， 生成的组件名 `displayName` 应该为 `withFoo(Bar)`.

  > 为什么？
  >
  > 一个组件的 `displayName` 可能会在开发者工具或者错误信息中使用到，因此有一个能清楚的表达这层关系的值能帮助我们更好的理解组件发生了什么，更好的Debug.

  ```jsx
  // 不好
  export default function withFoo(WrappedComponent) {
    return function WithFoo(props) {
      return <WrappedComponent {...props} foo />;
    }
  }
  
  // 好
  export default function withFoo(WrappedComponent) {
    function WithFoo(props) {
      return <WrappedComponent {...props} foo />;
    }
  
    const wrappedComponentName = WrappedComponent.displayName
      || WrappedComponent.name
      || 'Component';
  
    WithFoo.displayName = `withFoo(${wrappedComponentName})`;
    return WithFoo;
  }
  ```

- **属性命名**: 避免使用DOM相关的属性来用作其他的用途。

  > 为什么？
  >
  > 对于`style` 和 `className`这样的属性名，我们都会默认它们代表一些特殊的含义，如元素的样式，CSS class的名称。
  >
  > 在你的应用中使用这些属性来表示其他的含义会使你的代码更难阅读，更难维护，并且可能会引起bug。

  ```jsx
  // 不好
  <MyComponent style="fancy" />
  
  // 好
  <MyComponent variant="fancy" />
  ```

## 5、声明组件

- 不要使用 `displayName` 来命名React组件，而是使用引用来命名组件， 如 class 名称.

  ```jsx
  // 不好
  export default React.createClass({
    displayName: 'ReservationCard',
    // stuff goes here
  });
  
  // 好
  export default class ReservationCard extends React.Component {
  }
  ```

## 5、代码对齐

- 遵循以下的JSX语法缩进/格式. eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md) [`react/jsx-closing-tag-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-tag-location.md)

  ```jsx
  // 不好
  <Foo superLongParam="bar"
       anotherSuperLongParam="baz" />
  
  // 好, 有多行属性的话, 新建一行关闭标签
  <Foo
    superLongParam="bar"
    anotherSuperLongParam="baz"
  />
  
  // 若能在一行中显示, 直接写成一行
  <Foo bar="bar" />
  
  // 子元素按照常规方式缩进
  <Foo
    superLongParam="bar"
    anotherSuperLongParam="baz"
  >
    <Quux />
  </Foo>
  
  // 不好
  {showButton &&
    <Button />
  }
  
  // 不好
  {
    showButton &&
      <Button />
  }
  
  // 好
  {showButton && (
    <Button />
  )}
  
  // 好
  {showButton && <Button />}
  ```

## 4、单引号还是双引号

- 对于JSX属性值总是使用双引号(`"`), 其他均使用单引号(`'`). eslint: [`jsx-quotes`](http://eslint.org/docs/rules/jsx-quotes)

  > 为什么? HTML属性也是用双引号, 因此JSX的属性也遵循此约定.

  ```jsx
  // 不好
  <Foo bar='bar' />
  
  // 好
  <Foo bar="bar" />
  
  // 不好
  <Foo style={{ left: "20px" }} />
  
  // 好
  <Foo style={{ left: '20px' }} />
  ```

## 8、空格

- 总是在自关闭的标签前加一个空格，正常情况下也不需要换行. eslint: [`no-multi-spaces`](http://eslint.org/docs/rules/no-multi-spaces), [`react/jsx-tag-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-tag-spacing.md)

  ```jsx
  // 不好
  <Foo/>
  
  // very bad
  <Foo                 />
  
  // 不好
  <Foo
   />
  
  // 好
  <Foo />
  ```

- 不要在JSX `{}` 里两边加空格. eslint: [`react/jsx-curly-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-curly-spacing.md)

  ```jsx
  // 不好
  <Foo bar={ baz } />
  
  // 好
  <Foo bar={baz} />
  ```

## 9、属性

- JSX属性名使用驼峰式首字母小写风格.

  ```jsx
  // 不好
  <Foo
    UserName="hello"
    phone_number={12345678}
  />
  
  // 好
  <Foo
    userName="hello"
    phoneNumber={12345678}
  />
  ```

- 如果属性值为 `true`, 可以直接省略. eslint: [`react/jsx-boolean-value`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-boolean-value.md)

  ```jsx
  // 不好
  <Foo
    hidden={true}
  />
  
  // 好
  <Foo
    hidden
  />
  
  // 好
  <Foo hidden />
  ```

- <img> 标签总是添加 alt 属性。如果图片以presentation(感觉是以类似PPT方式显示?)方式显示，alt 可为空, 或者<img> 要包含role="presentation". eslint: jsx-a11y/alt-text

  ```jsx
  // 不好
  <img src="hello.jpg" />
  
  // 好
  <img src="hello.jpg" alt="Me waving hello" />
  
  // 好
  <img src="hello.jpg" alt="" />
  
  // 好
  <img src="hello.jpg" role="presentation" />
  ```

- 不要在 `alt` 值里使用如 "image", "photo", 或 "picture"包括图片含义这样的词， 中文也一样. eslint: [`jsx-a11y/img-redundant-alt`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/img-redundant-alt.md)

  > 为什么? 屏幕助读器已经把 `img` 标签标注为图片了, 所以没有必要再在 `alt` 里说明了.

  ```jsx
  // 不好
  <img src="hello.jpg" alt="Picture of me waving hello" />
  
  // 好
  <img src="hello.jpg" alt="Me waving hello" />
  ```

- 使用有效正确的 `role`属性值 [ARIA roles](https://www.w3.org/TR/wai-aria/roles#usage_intro). eslint: [`jsx-a11y/aria-role`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/aria-role.md)

  ```jsx
  // 不好 - not an ARIA role
  <div role="datepicker" />
  
  // 不好 - abstract ARIA role
  <div role="range" />
  
  // 好
  <div role="button" />
  ```

- 不要在元素上使用 `accessKey` 属性. eslint: [`jsx-a11y/no-access-key`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/no-access-key.md)

> 为什么? 屏幕助读器在键盘快捷键与键盘命令时造成的不统一性会导致阅读性更加复杂.

```jsx
// 不好
<div accessKey="h" />

// 好
<div />
```

- 避免使用数组的index来作为属性`key`的值，推荐使用唯一ID. ([为什么?](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318))

```jsx
// 不好
{todos.map((todo, index) =>
  <Todo
    {...todo}
    key={index}
  />
)}

// 好
{todos.map(todo => (
  <Todo
    {...todo}
    key={todo.id}
  />
))}
```

- 对于所有非必须的属性，总是手动去定义`defaultProps`属性.

> 为什么? 
>
> propTypes 可以作为组件的文档说明， 并且声明 defaultProps 的话意味着阅读代码的人不需要去假设一些默认值。更重要的是，显示的声明默认属性可以让你的组件跳过属性类型的检查。

```jsx
// 不好
function SFC({ foo, bar, children }) {
  return <div>{foo}{bar}{children}</div>;
}
SFC.propTypes = {
  foo: PropTypes.number.isRequired,
  bar: PropTypes.string,
  children: PropTypes.node,
};

// 好
function SFC({ foo, bar, children }) {
  return <div>{foo}{bar}{children}</div>;
}
SFC.propTypes = {
  foo: PropTypes.number.isRequired,
  bar: PropTypes.string,
  children: PropTypes.node,
};
SFC.defaultProps = {
  bar: '',
  children: null,
};   
```

- 尽可能少地使用展开运算符

> 为什么? 
>
> 除非你很想传递一些不必要的属性。对于React v15.6.1和更早的版本，你可以[给DOM传递一些无效的HTML属性](https://doc.react-china.org/blog/2017/09/08/dom-attributes-in-react-16.html)

例外情况:

- 使用了变量提升的高阶组件

```jsx
function HOC(WrappedComponent) {
  return class Proxy extends React.Component {
    Proxy.propTypes = {
      text: PropTypes.string,
      isLoading: PropTypes.bool
    };

    render() {
      return <WrappedComponent {...this.props} />
    }
  }
}  
```

- 只有在清楚明白扩展对象时才使用扩展运算符。这非常有用尤其是在使用Mocha测试组件的时候。

```jsx
export default function Foo {
  const props = {
    text: '',
    isPublished: false
  }

  return (<div {...props} />);
}    
```

 特别提醒：尽可能地筛选出不必要的属性。同时，使用[prop-types-exact](https://www.npmjs.com/package/prop-types-exact)来预防问题出现。

```jsx
// 好
render() {
  const { irrelevantProp, ...relevantProps  } = this.props;
  return <WrappedComponent {...relevantProps} />
}

// 不好
render() {
  const { irrelevantProp, ...relevantProps  } = this.props;
  return <WrappedComponent {...this.props} />
}    
```

## 10、Refs

- 总是在Refs里使用回调函数. eslint: [`react/no-string-refs`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-string-refs.md)

  ```jsx
  // 不好
  <Foo
    ref="myRef"
  />
  
  // 好
  <Foo
    ref={(ref) => { this.myRef = ref; }}
  />
  ```

## 11、圆括号

- 将多行的JSX标签写在 `()`里. eslint: [`react/jsx-wrap-multilines`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-wrap-multilines.md)

  ```jsx
  // 不好
  render() {
    return <MyComponent className="long body" foo="bar">
             <MyChild />
           </MyComponent>;
  }
  
  // 好
  render() {
    return (
      <MyComponent className="long body" foo="bar">
        <MyChild />
      </MyComponent>
    );
  }
  
  // 好, 单行可以不需要
  render() {
    const body = <div>hello</div>;
    return <MyComponent>{body}</MyComponent>;
  }
  ```

## 12、标签

- 对于没有子元素的标签来说总是用自关闭标签. eslint: [`react/self-closing-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/self-closing-comp.md)

  ```jsx
  // 不好
  <Foo className="stuff"></Foo>
  
  // 好
  <Foo className="stuff" />
  ```

- 如果组件有多行的属性， 关闭标签另起一行. eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

  ```jsx
  // 不好
  <Foo
    bar="bar"
    baz="baz" />
  
  // 好
  <Foo
    bar="bar"
    baz="baz"
  />
  ```

## 13、函数

- 使用箭头函数来获取本地变量。当需要将附加数据传递给事件处理程序时，它很方便。尽管如此，请确保它们不会严重影响性能，尤其是传递给可能是PureComponents的自定义组件时，因为它们每次都会触发可能不必要的重新发送程序。

  ```jsx
  function ItemList(props) {
    return (
      <ul>
        {props.items.map((item, index) => (
          <Item
            key={item.key}
            onClick={() => doSomethingWith(item.name, index)}
          />
        ))}
      </ul>
    );
  }
  ```

- 当在 `render()` 里使用事件处理方法时，提前在构造函数里把 `this` 绑定上去. eslint: [`react/jsx-no-bind`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-no-bind.md)

  > 为什么? 
  >
  > 在每次 `render` 过程中， 再调用 `bind` 都会新建一个新的函数，浪费资源.

  ```jsx
  // 不好
  class extends React.Component {
    onClickDiv() {
      // do stuff
    }
  
    render() {
      return <div onClick={this.onClickDiv.bind(this)} />;
    }
  }
  
  // 好
  class extends React.Component {
    constructor(props) {
      super(props);
  
      this.onClickDiv = this.onClickDiv.bind(this);
    }
  
    onClickDiv() {
      // do stuff
    }
  
    render() {
      return <div onClick={this.onClickDiv} />;
    }
  }
  ```

- 在React组件中，不要给所谓的私有函数添加 `_` 前缀，本质上它并不是私有的.

  > 为什么？
  >
  > `_` 下划线前缀在某些语言中通常被用来表示私有变量或者函数。但是不像其他的一些语言，在JS中没有原生支持所谓的私有变量，所有的变量函数都是共有的。尽管你的意图是使它私有化，在之前加上下划线并不会使这些变量私有化，并且所有的属性（包括有下划线前缀及没有前缀的）都应该被视为是共有的。了解更多详情请查看Issue [#1024](https://github.com/airbnb/javascript/issues/1024), 和 [#490](https://github.com/airbnb/javascript/issues/490) 。

  ```jsx
  // 不好
  React.createClass({
    _onClickSubmit() {
      // do stuff
    },
  
    // other stuff
  });
  
  // 好
  class extends React.Component {
    onClickSubmit() {
      // do stuff
    }
  
    // other stuff
  }
  ```

- 在 `render` 方法中总是确保 `return` 返回值. eslint: [`react/require-render-return`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/require-render-return.md)

  ```jsx
  // 不好
  render() {
    (<div />);
  }
  
  // 好
  render() {
    return (<div />);
  }
  ```

## 14、React 组件生命周期函数的顺序

- `class extends React.Component` 的生命周期函数:

> 1. 可选的 `static` 方法
> 2. `constructor` 构造函数
> 3. `getChildContext` 获取子元素内容
> 4. `componentWillMount` 组件渲染前
> 5. `componentDidMount` 组件渲染后
> 6. `componentWillReceiveProps` 组件将接受新的数据
> 7. `shouldComponentUpdate` 判断组件需不需要重新渲染
> 8. `componentWillUpdate` 上面的方法返回 `true`， 组件将重新渲染
> 9. `componentDidUpdate` 组件渲染结束
> 10. `componentWillUnmount` 组件将从DOM中清除, 做一些清理任务
> 11. *点击回调或者事件处理器* 如 `onClickSubmit()` 或 `onChangeDescription()`
> 12. *render 里的 getter 方法* 如 `getSelectReason()` 或 `getFooterContent()`
> 13. *可选的 render 方法* 如 `renderNavigation()` 或 `renderProfilePicture()`
> 14. `render` render() 方法
>

- 如何定义 `propTypes`, `defaultProps`, `contextTypes`, 等等其他属性...

  ```jsx
  import React from 'react';
  import PropTypes from 'prop-types';
  
  const propTypes = {
    id: PropTypes.number.isRequired,
    url: PropTypes.string.isRequired,
    text: PropTypes.string,
  };
  
  const defaultProps = {
    text: 'Hello World',
  };
  
  class Link extends React.Component {
    static methodsAreOk() {
      return true;
    }
  
    render() {
      return <a href={this.props.url} data-id={this.props.id}>{this.props.text}</a>;
    }
  }
  
  Link.propTypes = propTypes;
  Link.defaultProps = defaultProps;
  
  export default Link;
  ```

- `React.createClass` 的生命周期函数，与使用class稍有不同: eslint: [`react/sort-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/sort-comp.md)

> 1. `displayName` 设定组件名称
> 2. `propTypes` 设置属性的类型
> 3. `contextTypes` 设置上下文类型
> 4. `childContextTypes` 设置子元素上下文类型
> 5. `mixins` 添加一些mixins
> 6. `statics`
> 7. `defaultProps` 设置默认的属性值
> 8. `getDefaultProps` 获取默认属性值
> 9. `getInitialState` 或者初始状态
> 10. `getChildContext`
> 11. `componentWillMount`
> 12. `componentDidMount`
> 13. `componentWillReceiveProps`
> 14. `shouldComponentUpdate`
> 15. `componentWillUpdate`
> 16. `componentDidUpdate`
> 17. `componentWillUnmount`
> 18. *clickHandlers or eventHandlers* like `onClickSubmit()` or `onChangeDescription()`
> 19. *getter methods for render* like `getSelectReason()` or `getFooterContent()`
> 20. *Optional render methods* like `renderNavigation()` or `renderProfilePicture()`
> 21. `render`
>

## 15、isMounted

- 不要再使用 `isMounted`. eslint: [`react/no-is-mounted`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-is-mounted.md)

> 为什么?
>
>  [`isMounted` 反人类设计模式:()](https://facebook.github.io/react/blog/2015/12/16/ismounted-antipattern.html), 在 ES6 classes 中无法使用， 官方将在未来的版本里删除此方法.