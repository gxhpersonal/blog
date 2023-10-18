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