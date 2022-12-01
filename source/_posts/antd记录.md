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
```

### 如果table分页最后一页只有一条数据，删除这条会导致分页和数据展示不一致
原因：

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
                    dispatch({
                        type: 'save', payload: {
                            loadSuccess: false,
                            disabled: location?.query?.type === "1" ? true : false
                        }
                    });
                    dispatch({ type: 'query', payload: { id: match[1] } });
                }
            })
        },
    },
```

### Form表单中有上传组件`Upload`，无法在Form.Item的name属性绑定对应值，需要特殊处理下

