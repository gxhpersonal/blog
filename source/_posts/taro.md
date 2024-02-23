---
title: taro
date: 2023-10-17 14:52:19
tags: taro
categories: taro
---

### 新建项目

### 项目配置
1.开启 PreBundle 配置
> 开发环境热更新占用内存可以大大降低，热更新所需时间也将大幅减少；生产模式也可以通过提前编译依赖，大幅提升部署效率。
```js
/** config/index.js */
const config = {
  compiler: {
    type: 'webpack5',
    // 仅 webpack5 支持依赖预编译配置
    prebundle: {
      enable: true,
    },
  },
}
```

### 生产预渲染
```js
// /config/prod.js
export default{
  mini: {
    prerender: {
      match: 'pages/**', // 所有以 `pages/` 开头的页面都参与预渲染 prerender
      // include: ['pages/index/index'], // 参与预渲染 prerender
      exclude: [] // 不参与预渲染 prerender
    }
  },
}
```
预渲染会增加包体积，所以除非万不得已，否则不应该使用，而且有可以替代预渲染的方案：
```js
class SomePage extends Component {
  state = {
    mounted: false,
  }

  componentDidMount() {
    // 等待组件载入，先渲染了首屏我们再渲染其它内容，降低首次渲染的数据量
    // 当 mounted 为 true 时，CompA, B, C 的 DOM 树才会作为 data 参与小程序渲染
    // 注意我们需要在 `componentDidMount()` 这个周期做这件事（对应 Vue 的 `ready()`），更早的生命周期 `setState()` 会与首次渲染的数据一起合并更新
    // 使用 nextTick 确保本次 setState 不会和首次渲染合并更新
    Taro.nextTick(() => {
      this.setState({
        mounted: true,
      })
    })
  }

  render() {
    return (
      <View>
        <FirstScreen /> /* 假设我们知道这个组件会把用户的屏幕全部占据 */
        {this.state.mounted && (
          <React.Fragment>
            {' '}
            /* CompA, B, C 一开始并不会在首屏中显示 */
            <CompA />
            <CompB />
            <CompC />
          </React.Fragment>
        )}
      </View>
    )
  }
}
```

### 持久化缓存功能，提升二次编译速度，提升开发效率

### 同时打包多端代码，多端项目同时调试
```js
//config/index.js
const baseConfig = {
  ...
  outputRoot: `dist/${process.env.TARO_ENV}`,
}
```

### 半编译模式 —— CompileMode 通常用于节点较多的列表
```js
//config/index.js
const config = {
  mini: {
    experimental: {
      compileMode: true
    }
  }
  // ...
}
//然后只需要给 Taro 基础组件添加 compileMode 属性，该组件及其 children 将会被编译为单独的小程序模板：
function GoodsItem () {
  return (
    <View compileMode>
      ...
    </View>
  )
}
```
常见问题：
1. 编译出的模板文件会增加包体积
2. 只能优化部分语法

### 优化主包体积大小
```js
//config/index.js
//这样简单配置之后，可以避免主包没有引入的 module 被提取到commonChunks中，该功能会在打包时分析 module 和 chunk 的依赖关系，筛选出主包没有引用到的 module 把它提取到分包内
module.exports = {
  // ...
  mini: {
    // ...
    optimizeMainPackage: {
      enable: true,
    },
  },
}
```

### 添加环境字段
用taro init生成项目时，根目录下会发现有三个`.env.xxx`文件。
因为默认打包时查找的文件是`NODE_ENV`的值，所以需要把`.env.dev`和`.env.prod`文件改为`.env.development`和`.env.production`;
或者可以在`package.json`文件中修改命令`"dev:weapp": taro build --type weapp --watch --mode dev`;
可以发现命令中不加`--mode dev`时默认`mode`为`development`;
两种方案皆可。
然后就可以在`.env.xxx`文件中添加对应环境变量字段：
```C
# 配置文档参考 https://taro-docs.jd.com/docs/next/env-mode-config
TARO_APP_ID="开发环境小程序APPID"
TARO_APP_API="https://api-dev.com"
```
> 请注意，只有以`TARO_APP_`开头的变量将通过`webpack.DefinePlugin`静态地嵌入到客户端侧的代码中。这是为了避免和系统内置环境变量冲突。

### Uncaught Error: Rendered fewer hooks than expected. This may be caused by an accidental early return statement.
不要在循环，条件或嵌套函数中调用Hook。相反，始终在React函数的顶层使用Hooks。通过遵循此规则，您可以确保每次组件呈现时都以相同的顺序调用Hook。这就是允许React在多个useState和useEffect调用之间正确保留Hook状态的原因。

选择在没有判断条件的顶层使用useContext、useState、useEffect、userHistory、useTransaction等，将获取的值，作为参数传给子组件使用。

### 多个页面引用同一个自定义组件时，页面不显示
开启`optimizeMainPackage`打包优化导致的。。。暂时不知道原因。。。估计是官方bug
```js

```

### useEffect
两个参数，第一个参数为函数，函数内执行初始化代码，可以返回一个函数，用来组件销毁时执行；
第二个参数为数组，传值则 Effect 在 初始渲染后以及依赖项变更的重新渲染后 运行；
```js
useEffect(() => {
  // ...
}, [a, b]); // 如果 a 或 b 不同则会再次运行
```
传空数组，Effect 仅在 初始渲染后 运行；
```js
useEffect(() => {
  // ...
}, []); // 不会再次运行（开发环境下除外）
// 为了 帮助你发现 bug，在开发环境下，React 在运行 setup 之前会额外运行一次setup 和 cleanup。这是一个压力测试，用于验证 Effect 逻辑是否正确实现。如果这会导致可见的问题，那么你的 cleanup 函数就缺少一些逻辑。cleanup 函数应该停止或撤消 setup 函数正在执行的任何操作
```
如果完全不传递依赖数组，则 Effect 会在组件的 每次单独渲染（和重新渲染）之后 运行。
```js
useEffect(() => {
  // ...
}); // 总是再次运行
```

### alias
用于配置目录别名，从而方便书写代码引用路径。
```js
//config/index.js
module.exports = {
  // ...
  alias: {
    '@/components': path.resolve(__dirname, '..', 'src/components'),
    '@/utils': path.resolve(__dirname, '..', 'src/utils'),
  },
}
//index.jsx
import A from '@/components/A'
```
为了让编辑器（VS Code）不报错，并继续使用自动路径补全的功能（F12能定位到引用文件并打开）：
```json
//项目根目录下新建文件 jsconfig.json
{
  "compilerOptions": {
    "baseUrl": ".", //根路径
    "paths": {
      "@/components/*": ["./src/components/*"], //相对路径，如果有更深引用，需要加 /*
      "@/utils/*": ["./src/utils/*"],
    }
  }
}
```

### mobx中请求异步赋值警告
```js
import { runInAction } from 'mobx';
getAdvData = async () => {
  const data = await api.http({ url: '/xjb/city/advList' })
  //需要把赋值操作放在runInAction回调中
  runInAction(() => {
    this.banner = data.result.first_banner
    this.cipian = data.result.first_cipian
    this.jingang = data.result.first_jingang
    this.address = data.result.jiabo_adress[0].image_url
    this.yunying = data.result.first_yunying
  })
}
```

### 编译taro警告`Error: chunk common [mini-css-extract-plugin]`
这是由于引用组件顺序不同，打包时会多打包一次，就会发出警告
```js
export default {
  // ...
  mini: {
    miniCssExtractPluginOptions: {
      ignoreOrder: true,
    },
  }
  // ...
};
```

### 上滑加载page不更新问题
```js
//如果直接声明 let page = 1;在事件回调中page+=1;始终只能拿到page=2，应该是组件更新时，把page变量直接重置了；
//如果用useState钩子，不能在事件中更新值，因为不能及时拿到更新后的page，所以要提前更新page，放在接口调用方法中：
const [list, setList] = useState([]);
const [hasData, setHasData] = useState(true);
const [pages, setPages] = useState(1);
const getData = async (page = 1, refresh = true) => {
  //提前更新page
  setPages(page);
  let res = await api.http({
    url: '/api/getdata',
    data: { page }
  });
  if (refresh) {
    setList(res.data);
  } else {
    setList(list.concat(res.data));
  }
  setHasData(res.data?.length < 10 ? false : true);
}
// 上滑触底
useReachBottom(() => {
  if (hasData) {
    getData(currentTab, pages + 1, false);
  }
})
```

### ios小程序不支持直接写backdrop-filter，需要加上浏览器前缀-webkit-backdrop-filter

### ios偶发自定义tabbar点击没反应问题
经过真机调试，发现自定义tabbar在来回切换，会有一定几率点不到，因为tabbar被挤出了页面，掉在最下面，虽然还能看到，导致点不到
```js
//page show
useDidShow(() => {
  //触发一下滚动，解决ios偶发tabbar点不中bug
  Taro.pageScrollTo({
    scrollTop: 1
  })
})
```