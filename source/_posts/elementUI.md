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
所以要改为`list-type="text"`，这样只能显示文件名，不会显示成卡片样式，而且样式需要自己改动
或者自己写一个视频展示样式

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

### 需要传入数组的组件，需要提前声明变量为数组，否则调接口返回填充还是不显示，并且不报错

### element-plus的el-scroller监听滑动到底部
```html
<script setup>
const scrollEl = ref();
const scroll = (e) => {
  //scrollEl.value.wrapRef.scrollHeight：当前内容高度
  //scrollEl.value.wrapRef.clientHeight：当前内容可视高度（外部scroll标签设置的高度）
  //e.scrollTop 滚动条距顶部距离
  if (
    scrollEl.value.wrapRef.scrollHeight - scrollEl.value.wrapRef.clientHeight ==
    e.scrollTop
  ) {
    console.log("触底");
  }
};
</script>
<el-scrollbar ref="scrollEl" @scroll="scroll">
  ...
</el-scrollbar>
```

### element同时上传两个文件时，on-success只执行一次
前提：如果on-success里面如果给file-list进行了赋值，此时这个回调方法只执行一次
解决：
```js
// 上传成功回调
onSuccess(response, file, fileList){
  //等待所有文件都上传完成，这里注意fileList是所有的文件（包含已上传的）
  if(fileList.every(it => it.status == 'success')) { 
    fileList.map(item => {
    //只push新上传的 （带有response）
    item.response && this.fileList.push({name:item.response.data.originalName,url:item.response.data.link});
    })
    setTimeout(()=>{
      //提交父组件改变的文件列表
      this.$emit("input", this.fileList)
      })
    }
}
```
