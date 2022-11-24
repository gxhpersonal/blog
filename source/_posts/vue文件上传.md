---
title: vue文件上传
date: 2019-03-26 13:56:08
tags: vue
categories: vue
---

### 文件上传（支持PDF格式文件上传）以文件流的形式上传到接口
```html
<input type="file" id="CarDamageFile" class="g-core-image-upload-form" value="" accept="image/gif,image/jpeg,image/jpg,image/png,application/pdf" data-type="back-page" name="Pictures" multiple="multiple" @change="imagechanged($event,Udata[7])" />
```
```js
//文件上传
wrongTip(data){
  console.log(data)
},
imagechanged(e, data) {
//参数e为当前上传的表单，data为要处理的数据
    var that = this,
      _data = that.Udata,
      fileList = e.target.files;
    data.count = 0;
    if (fileList.length > 5) {
      that.wrongTip("每项最多上传5张");
      return;
    }
    if (fileList) {
      for (var i = 0; i < fileList.length; i++) {
		//遍历上传的文件，一个一个处理
        that.readFile(fileList.length, fileList[i], data);
      }
    }
  },
  readFile(num, res, Sdata) {
//num为上传文件数量，res为上传文件数据，Sdata为要处理数据
    var that = this,
      _data = that.Udata,
      Ename = Sdata.Ename,
      render = new FileReader();
    if (res.type != "application/pdf" && res.size / 1024 > 10240) {
      that.wrongTip(
        "材料仅限pdf. jpg .gif .png格式，单个图片大小不超过10M，单个PDF大小不超过4.5M，每项最多上传5张。"
      );
      return;
    } else if (res.type == "application/pdf" && res.size / 1024 > 4608) {
      that.wrongTip(
        "材料仅限pdf. jpg .gif .png格式，单个图片大小不超过10M，单个PDF大小不超过4.5M，每项最多上传5张。"
      );
      return;
    } else {
      render.addEventListener(
        "load",
        function() {
          var src = this.result;
          var formData = new FormData();
          var index = this.index;
          var _fileName = res.name;
          formData.append("fileUrl", res);
          formData.append("fileName", _fileName);
          Sdata.loading = true;
          Basic.ajaxReturnPromise({
            type: "POST",
            apiUrl: "/Comment/UploadImg",
            customLoading: true,
            param: formData,
            emulateJSON: false
          }).then(
            response => {
              if (response.IsSuccess) {
                ++Sdata.count;
                var data = response.data;
                if (Sdata.imgSrc.length + data.length < 6) {
                  Sdata.curImg = parseInt(Sdata.curImg + 1);
                  // Sdata.imgSrc.shift();
                  for (var i in data) {
                    if (data[i].BaseUrl.toLowerCase().indexOf(".pdf") != -1) {
                      Sdata.imgSrc.unshift({
                        picUrl: "https://h5-fe.huizuche.com/claim/PDF.png",
                        url: data[i].BaseUrl,
                        name: _fileName
                      });
                    } else {
                      Sdata.imgSrc.unshift({
                        picUrl:
                          data[i].BaseUrl +
                          "?x-oss-process=image/resize,m_fill,h_78,w_78",
                        url: data[i].BaseUrl,
                        name: _fileName
                      });
                    }
                  }
                  that.$store.dispatch(Sdata.store, {
                    [Ename]: Sdata.imgSrc
                  });
                } else {
                  that.wrongTip("每项最多上传5张");
                }
              }
              if (Sdata.count == num) {
                Sdata.loading = false;
              }
            },
            err => {
              that.wrongTip("文件上传失败，请重新上传");
            }
          );
        },
        false
      );
      render.readAsDataURL(res);
    }
  }
```
