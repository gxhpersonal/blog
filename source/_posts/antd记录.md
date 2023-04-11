---
title: antd记录
date: 2022-11-25 16:01:48
categories: React
tags: React
---

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

### Form表单中的Form.Item组件`name`属性为数组时，可以做嵌套对象使用

### react路由跳转进入，链接参数为数字，刷新后，链接参数变字符串
```js
const { query } = this.props.location;
const { cityId, activityId, smsSupplierType, smsPrice } = query;
//antd中Form表单initialValues默认值填充校验数据类型，所以要和你Form表单中value类型相同，才能回显
```

### tree组件实现筛选只保留目标项
```html
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