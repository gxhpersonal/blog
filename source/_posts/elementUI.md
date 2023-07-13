---
title: element记录
date: 2022-11-25 16:01:48
categories: vue
tags: vue
---

### input中加slot标签，表单验证无法回显
默认宽度可能会覆盖回显值，设置足够宽度

### Form表单验证通过，却没反应
```js
//如果表单验证rules对象中有自定义验证，必须在验证函数最后返回回调函数
let validatePass1 = (rule, value, callback) => {
let reg = /(?!^0*(\.0{1,2})?$)^\d{1,13}(\.\d{1,2})?$/;
if (value && !reg.test(value)) {
    callback(new Error('仅支持输入大于0的数字'));
} else {
    //这里必须返回
    callback()
}
};
rules: {
    market_price: [{ validator: validatePass1, trigger: "change" }],
}
```

### Form表单如果是动态遍历添加表单项，且遍历项是字符串不是对象
```html
<el-form-item v-for="(item, index) in form.commodity_label" :prop="'commodity_label.' + index" :key="index">
  <el-input class="item" v-model="form.commodity_label[index]" maxlength="6" placeholder="请输入"></el-input>
  <el-button @click.prevent="removeMark(index)">删除</el-button>
</el-form-item>
```


### `el-upload`组件`list-type="picture-card"`属性如果是上传视频无法正常显示，不如antd，狗头

### 列表筛选条件添加到链接，以实现跳转详情返回保存筛选条件
```js
// 选中筛选条件，点击搜索时
handleSearch() {
  const { path } = this.$router.currentRoute;
  this.form = {}；//筛选参数
  this.$router.replace({ path, query: this.form }, () => { })
},
//返回列表页
mounted() {
  const { query } = this.$router.currentRoute;
  this.form = JSON.parse(JSON.stringify(query));//回显筛选项值，用深拷贝是为了form和query彻底隔离开，否则会影响form赋值
},
```
