---
title: react记录
date: 2019-11-28 11:17:22
tags: React
categories: React
top: 100
---

### 打包后路径不对解决
执行`npm run build`后发现打包文件路径错误，在package.json文件最外层添加`"homepage": "./"`

### 安装依赖（sass）
```npm
$ npm install node-sass --save
```

### 打包不同环境代码包
1.安装`cross-env`插件：
```npm
npm install cross-env -dev
```

2.`package.json`文件中：
```json
"scripts": {
  "start": "cross-env REACT_APP_SECRET_ENV='development' node scripts/start.js",
  "build": "cross-env REACT_APP_SECRET_ENV='production' node scripts/build.js",
  "build:staging": "cross-env REACT_APP_SECRET_ENV='staging' node scripts/build.js",
  "test": "node scripts/test.js"
},
```

3.在任意js中不同环境就可以获取到不同的`REACT_APP_SECRET_ENV`值;

### 路由懒加载
`router.js`文件中：
```js
import React, { Suspense, lazy } from "react";
import {BrowserRouter as Router, Switch, Route} from "react-router-dom";
const Home = lazy(() => import('../component/home/Index'));
const BasicRoute = () => (
  <Router>
  // fallback用来页面未加载显示loading等元素提升用户体验，可引入组件
    <Suspense fallback={<div></div>}>
      <Switch>
        // exact用来路由名称精准匹配
        <Route exact path="/" component={Home} />
      </Switch>
    </Suspense>
  </Router >
);
export default BasicRoute;
```

### css添加浏览器前缀
修改`package.json`文件中的
```json
"browserslist": {
  // 正式环境
  "production": [
    "> 1%",
    "last 2 versions",
    "not ie <= 8",
    "iOS >= 6",
    "Android > 4.1",
    "Firefox > 20"
  ],
  // 开发环境
  "development": [
    "last 1 chrome version",
    "last 1 firefox version",
    "last 1 safari version"
  ]
}
```

### 全局方法
例如引用axios到全局
在`index.js`中：
```js
import Axios from 'axios';//直接引用  
//or
import Axios from "/axios/index.js";//如果封装了axios，还可以引入封装后的文件
React.$axios = Axios;
```


### react集成预渲染方案，解决首屏白屏问题+seo问题
1.安装单页应用预渲染插件：
```npm
npm install prerender-spa-plugin --save-dev
```

2.安装自定义配置插件（此工具可以自定义配置webpack插件）

1) 安装插件
```npm
npm install react-app-rewired --save-dev
```
> [插件官网](https://github.com/timarney/react-app-rewired/blob/HEAD/README_zh.md)
> 此工具可以在不 'eject' 也不创建额外 react-scripts 的情况下修改 create-react-app 内置的 webpack 配置，然后你将拥有 create-react-app 的一切特性，且可以根据你的需要去配置 webpack 的 plugins, loaders 等。
> 使用了 react-app-rewired 之后，等于你得到了项目的配置权，但这表示你的项目将无法得到 CRA 提供的配置“保证”，希望你知道自己要做什么。

2) 在根目录中创建一个 config-overrides.js 文件
```js
+-- your-project
|   +-- config-overrides.js  //新增
|   +-- node_modules
|   +-- package.json
|   +-- public
|   +-- README.md
|   +-- src
```
文件中添加`prerender-spa-plugin`插件的配置项：
```js
/* config-overrides.js */
const PrerenderSPAPlugin = require('prerender-spa-plugin');
const path = require('path');

module.exports = (config, env) => {
  if (env === 'production') {
    config.plugins = config.plugins.concat([
      new PrerenderSPAPlugin({
        routes: ['/'],  //路由地址，要生成静态文件的路由添加在数组中
        staticDir: path.join(__dirname, 'build'),  //staticDir是使用哪个文件夹作为模板目录默认build
      }),
    ]);
  }

  return config;
};
```

3) 替换 package.json 中 scripts 执行部分

```json
  /* package.json */

  "scripts": {
-   "start": "react-scripts start",
+   "start": "react-app-rewired start",
-   "build": "react-scripts build",
+   "build": "react-app-rewired build",
-   "test": "react-scripts test --env=jsdom",
+   "test": "react-app-rewired test --env=jsdom",
    "eject": "react-scripts eject"
}
```
* 注意：不用替换 eject 部分。工具中没有针对 eject 的配置替换，执行了 eject 命令会让工具失去作用（能找到这个插件我也相信你知道 eject 是干嘛的）。
* react-scripts 是 create-react-app 的一个核心包，一些脚本和工具的默认配置都集成在里面，而 react-scripts eject 命令执行后会将封装在 create-react-app 中的配置全部反编译到当前项目，这样用户就能完全取得 webpack 文件的控制权。所以，eject 命令存在的意义就是更改 webpack 配置存在


3.执行`npm run build`打包后打开`build/index.html`文件会发现所有dom都已经加入了，bingo！


### react集成TDK（2020.5.15大佬插件竟然1万3千多赞，惊呆）
[传送门官方地址](https://github.com/nfl/react-helmet)
非常简单
1.安装`react-helmet`插件：
```npm
npm install react-helmet --save-dev
```
打包构建用，不在生产环境使用，所以加到`devDependencies`配置项下就可以

2.引入插件，模板中加入`Helmet`标签即可：
```js
import React from "react";
import {Helmet} from "react-helmet";

class Application extends React.Component {
  render () {
    return (
        <div className="application">
            <Helmet>
                <meta charSet="utf-8" />
                <title>title</title>
                <meta name="keywords" content="keywords" />
                <meta name="description" content="description" />
            </Helmet>
            ...
        </div>
    );
  }
};
```
* 注意：嵌套或后继组件将覆盖重复的更改：
```js
<Parent>
    <Helmet>
        <title>My Title</title>
        <meta name="description" content="Helmet application" />
    </Helmet>

    <Child>
        <Helmet>
            <title>Nested Title</title>
            <meta name="description" content="Nested component" />
        </Helmet>
    </Child>
</Parent>
```

### 父子组件互相传值/调用方法

##### 类组件：
父组件中：
```jsx
{/* 优惠券领取弹窗 */}
<CouponPopup vname={this.props.vname} couponItem={this.state.couponItem} event={this} onRef={(ref) => { this.child = ref }} />
```

子组件中：
```jsx
export default class CouponPopup extends React.Component{
  constructor() {
    super();
    this.state = {};
  }
  componentDidMount() {
    //子组件实例传递给父组件
    this.props.onRef(this)
  }
  toParent = () => {
    // 调用父组件方法
    this.props.event.closePopup()
  }
  reserve(){
    console.log("这是提供给父组件调用的方法")
  }
}
```
##### 函数组件
父组件：
```js
import React from 'react';
export default () => {
  ref = React.useRef(null);
  //调用子组件方法
  const getChild = () => {
    ref.current.getInfo(v.activityInfo);
  }
  return (
    <>
      <div onClick={getChild}></div>
      <ChildComponents onRef={ref}  getUser={getUser} />
    </>
  )
}
```
子组件：
```js
import React, { useImperativeHandle } from 'react';
export default (props) => {
  const { getUser } = props;
  const getInfo = () => {

  }
  //子组件方法暴露给父组件
  useImperativeHandle(props.onRef, () => {
    return { getInfo }
  })
  return (
    // 子组件调用父组件方法
    <div onClick={getUser}></div>
  )
}
```


### 路由跳转传参方式（state、query、params），目前我做的是history模式路由，而且以后也不会考虑hash模式
1.`state`参数：
> `state`方式传参刷新页面参数不会丢失，并且是隐式传参，链接中不会显示参数，`state`传参的方式只支持`Browserrouter`路由，不支持`hashrouter`，就是我们常说的`history`模式，而不支持`hash`模式
获取参数：`this.props.location.state`
2.`query`参数：
> `query`方式传参刷新页面参数丢失
3.`params`参数：
> `params`方式传参刷新页面参数丢失
* 暂时并没有发现在history模式下query和params两种方式的不同，看网上说在hash模式下会有区别，然后还有文档说params传参可以是下面这种：
```jsx
import { Link } from "react-router-dom";
state={
    id:88,
    name:'Jack',
}
// 路由页面：
<Route path='/demo/:id/:name' component={Demo}></Route>  
// 路由跳转并传递参数：
    // 链接方式：
    <Link to={{pathname:`/demo/${this.state.id}/${this.state.name}`}}></Link>

    // js方式：
    this.props.history.push({pathname:`/demo/${this.state.id}/${this.state.name}`})
// 获取参数：
this.props.match.params     //结果 {id: "88", name: "Jack"}
```
这种感觉像vue的子路由，还有待进一步研究

### Hook
1.
```js
import {useState} from "react";
const [state,setState] = useState({top:0,bottom:0,left:0,right:0})
```

### 使用条件渲染运算符`&&`注意
```js
render(){
  let a = 0;
  return(
    <div>
      {a && <div>`&&`后面的元素会被跳过，但会返回false表达式</div>}
    </div>
  )
}
```
上面代码返回值`<div>0</div>`，因为&&表达式前面的条件是0，而不是布尔值，0会作为值返回；

### react中css作用域
react充分发扬自由编程，没有自己语法的特点，没有自己的css作用域，所以需要用到webpack中的`css-module`，一般是默认的，无需配置。