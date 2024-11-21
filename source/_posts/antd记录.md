---
title: antd记录
date: 2022-11-25 16:01:48
categories: React
tags: React
---

### 如果table分页最后一页只有一条数据，删除这条会导致分页和数据展示不一致
原因：如果需求要求记录当前table页，删除之后没有判断当前页是否还有数据，所以判断如果当前页只有一条&&当前页大于1，则重新请求时页数-1
```js
onDeleteItem: id => {
    dispatch({
        type: 'nodeManage/delete',
        payload: { id },
    }).then(() => {
        this.handleRefresh({
            page:
                list.length === 1 && pagination.current > 1
                ? pagination.current - 1
                : pagination.current,
            })
    })
}
```
<!--more-->

### 接口返回的数据填充`Form`，有时会无法填充
原因：接口返回比页面渲染慢，导致页面渲染完成还没有拿到数据
解决：保证接口返回数据再渲染页面
```js
//如果用了状态管理
state: {
    loadSuccess: false,
},
effects: {
    *query({ payload = {} }, { call, put }) {
        const data = yield call(getTaskDetail, payload);
        if (data.success) {
            yield put({
                type: 'save',
                payload: {
                    loadSuccess: true
                },
            })
        }
    },
},

//在jsx文件中：
render(){
    return(
        {this.props.loadSuccess && <div></div>}
    )
}
```
> 应该有更好的解决方案，这种如果加载时间长会白屏很长时间，等有好的解决思路再补充。。。

> 找到了，就是官方方法，在form标签上添加`fields={fields}`属性，缺点是其中的`fields`值不是对象，而是包含`name`和`value`属性的对象数组，所以如果回显时，需要把对应的值遍历填充相应`value`

### 获取路由中的id
dva.js store文件中无法使用react方法；
```js
//安装path-to-regexp插件
const { pathToRegexp } = require("path-to-regexp");
subscriptions: {
    setup({ dispatch, history }) {
        history.listen((location) => {
            const match = pathToRegexp('/marketing/node/:id').exec(location.pathname);
            if (match) {
                dispatch({ type: 'query', payload: { id: match[1] } });
            }
        })
    },
},
```

### Form表单中有上传组件`Upload`，无法在Form.Item的name属性绑定对应值，需要特殊处理下
一般是回显时使用`defaultFileList`属性绑定值，新增时可以用自带的`onChange`方法

### Form表单中的Form.Item组件`name`属性为数组时，可以做嵌套对象使用
```js
// 获取相应值
form.name.secondname
```

### react路由跳转进入，链接参数为数字，刷新后，链接参数变字符串
```js
const { query } = this.props.location;
const { cityId } = query;
typeof(cityId) //'string'
//antd中Form表单initialValues默认值填充校验数据类型，所以要和你Form表单中value类型相同，才能回显
```

### tree组件实现筛选只保留目标项（而不是官方那种搜索项高亮+展开）
> `checkStrictly`是核心，可以使`checkedKeys`即选中的数组与`treeData`即树列表之间的绑定关系分离开，从
> 而做到筛选时暂时性的改变`treeData`数据不会影响`checkedKeys`
```html
<!-- 这里最重要的是需要有一个常量一个变量来存储原始数据和筛选后的数据`normListCopy`为筛选后的数据 -->
<Input placeholder="搜索" onChange={this.onChange}/>
<Tree
    height={278}
    checkable
    checkStrictly
    onCheck={this.onCheck}
    checkedKeys={checkedKeys}
    fieldNames={{ title: "title", key: "id" }}
    treeData={normListCopy}
></Tree>
```

### dispatch({...}).then(()=>{}) 报错 is not a function

这种情况都是`dispatch`中的方法没找到，确认下方法是否声明，或者需要在调用的方法中`return`一个值；

### 用umi搭建的项目，新增环境变量
因为用了umi框架，所以不考虑用`cross-env`控制变量；
umi官方推荐一个`UMI_ENV`变量，在项目根目录新建`config.${UMI_ENV}.ts`文件，添加代码如下：
```js
export default {
  define: {
    // 添加这个自定义的环境变量
    "process.env.UMI_ENV": process.env.UMI_ENV,
  },
}
```
这样，在任意页面代码中`process.env.UMI_ENV`就是当前环境变量值，这个和`vue-cli`中的`.env.{mode}`文件异曲同工，都是在环境配置文件输出环境变量