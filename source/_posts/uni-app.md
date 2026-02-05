---
title: uni-app
date: 2023-10-17 14:52:19
tags: uni-app
categories: uni-app
---

### 小程序增加判断环境
```js
// #ifdef MP-WEIXIN
const accountInfo = uni.getAccountInfoSync();
const {
	envVersion
} = accountInfo.miniProgram; //小程序当前环境
Vue.prototype.$envVersion = envVersion;
if (envVersion == "develop") {
	// url = 'https://www.xxx.site'
}
if (envVersion == "trial") {
	url = "http://192.168.1.1" //测试环境
}
if (envVersion == "release") {
	url = 'https://www.xxx.site'
}
// #endif
```

### 条件编译快捷命令
选中要条件编译的代码块，`ctrl + alt + /`即可生成正确注释
```js
// #ifdef APP
uni.hideNavigationBarLoading();
// #endif
```

### 滚动一屏，sticky失效
需要把 sticky元素放在 一个 view 中，不能放在 template 中  

### app端选择地图导航
```js
toMapAPP(data) {
  const {
    lat,
    lng,
    shopName,
    name
  } = data;
  let url = "";
  if (plus.os.name == "Android") {
    //判断是安卓端
    plus.nativeUI.actionSheet({
        //选择菜单
        title: "选择地图应用",
        cancel: "取消",
        buttons: [{
            title: "百度地图",
          },
          {
            title: "高德地图",
          },
        ],
      },
      function(e) {
        switch (e.index) {
          //下面是拼接url,不同系统以及不同地图都有不同的拼接字段
          case 1:
            url =
              `baidumap://map/marker?location=${lat},${lng}&title=${name||shopName}&src=taoliangche&coord_type=gcj02`
            break;
          case 2:
            url =
              `androidamap://arroundpoi?sourceApplication=softname&keywords=${name||shopName}&lat=${lat}&lon=${lng}&dev=0`;
            break;
          default:
            break;
        }
        if (url != "") {
          url = encodeURI(url);
          plus.runtime.openURL(url, function(e) {
            plus.nativeUI.alert("本机未安装指定的地图应用");
          });
        }
      }
    );
  } else {
    // iOS上获取本机是否安装了百度高德地图，需要在manifest里配置
    // 在manifest.json文件app-plus->distribute->apple->urlschemewhitelist节点下添加
    //（如urlschemewhitelist:"iosamap","baidumap"）
    plus.nativeUI.actionSheet({
        title: "选择地图应用",
        cancel: "取消",
        buttons: [
          // 	{
          // 	title: "腾讯地图"
          // },
          {
            title: "百度地图",
          },
          {
            title: "高德地图",
          },
        ],
      },
      function(e) {
        switch (e.index) {
          // case 1:
          // 	url = `qqmap://map/geocoder?coord=${lat},${lng}&referer=xxx`;
          // 	break;
          case 1:
            url =
              `baidumap://map/direction?origin={{我的位置}}&destination=latlng:${lat},${lng}|name=${shopName||name}&mode=driving&coord_type=bd09ll`;
            // coord_type=bd09ll：坐标系类型，必须与传入的经纬度编码一致。
            // origin={{我的位置}}：固定写法，表示以用户当前位置为起点。
            break;
          case 2:
            url = `iosamap://path?sourceApplication=掏靓车&keywords=${shopName||name}&dlat=${lat}&dlon=${lng}&dev=0&style=0`;
              // lat/lon：目标经纬度（必须使用高德地图的 ​​GCJ-02​​ 坐标系）。
              // dev：坐标系类型，0 表示 GCJ-02，1 表示 GPS（WGS-84）。
              // style：导航方式，2 表示驾车，0 表示速度最快策略。
            break;
          default:
            break;
        }
        if (url != "") {
          url = encodeURI(url);
          plus.runtime.openURL(url, function(e) {
            plus.nativeUI.alert("本机未安装指定的地图应用");
          });
        }
      }
    );
  }
},
```

### app分享微信小程序
```js
// 分享到微信小程序
share() {
  // #ifdef APP
  uni.share({
    provider: "weixin",
    scene: "WXSceneSession",
    type: 5,
    miniProgram: {
      id: '原始小程序 id',
      path: '/pages/home/home?id=' + this.cid + '&uid=' + uni.getStorageSync('uid'),
      type: 0,
      webUrl: "www.123.com"
    },
    title: this.info.name,
    summary: "描述",
    imageUrl: "share.png",
    success: function(res) {
      console.log("success:" + JSON.stringify(res));
    },
    fail: function(err) {
      console.log("fail:" + JSON.stringify(err));
    }
  });
  // #endif
}
```
注意事项：
1. 报错：应用与小程序未在同一个开放平台下：因为直接运行到标准基座了，这个时候可以打自定义基座
2. 报错：share:fail [Share微信分享:-3]Unable to send, https://ask.dcloud.net.cn/article/287
原因一 miniProgram中的webUrl 未填写，随便填写一个地址就行了
原因二 uni.share 中的imageUrl 图片大小过大，应该小于20kb
3. 报错：不支持的分享类型
检查 uni.share 中miniProgram参数中的id要填写小程序原始id，不是小程序的appid，原始 id在小程序后台-账号设置-账号信息-原始ID


### picker mode="selector"选择器类型
1.value不是数据中的 id啥的，而是对应的数组下标

### app内选择 poi
在`manifest.json`中添加高德地图的配置
1.安卓/ios模块配置——Geolocation（定位）——高德定位添加用户名和 appkey
2.Maps(地图)——高德地图，同样添加用户名和 appkey
一个 appkey只能绑定一个 app
部分安卓手机 input设置为 disable绑定 click事件不生效

### 编译版本和手机端sdk版本不一致
需要重新安装发布自定义基座


### 蓝牙打印机
```vue
<template>
	<view>
		<nav-bar navBgColor="#fff" left-text="打印机设置"></nav-bar>
		<view class="refresh" @click="searchPrint">选择蓝牙设备<image class="icon"
				src="https://taoliangche.oss-cn-beijing.aliyuncs.com/mini-my/6.30/resh.png"></image>
		</view>
		<view class="print-loading">搜索中...</view>
		<view class="me-list">
			<view class="item" v-for="(v,i) in devices" :key="i" @click="setBindDevice(v)">
				<view class="name">{{v.name}}</view>
				<view :class="['status',v.isBind&&'connect']">{{v.isBind?'已连接':'未连接'}}
					<image class="icon" src="https://taoliangche.oss-cn-beijing.aliyuncs.com/mini-my/6.30/gth.png"></image>
				</view>
			</view>
		</view>
	</view>
</template>

<script setup>
	import {
		ref
	} from 'vue'
	import {
		onLoad,
		onHide,
		onUnload
	} from '@dcloudio/uni-app'
	const devices = ref([])
	const bindDevice = ref({})
	const platform = ref('')

	onLoad(() => {
		getSystemInfo()
		searchPrint()
	})
	onHide(() => {
		stopBluetoothDevicesDiscovery()
	})
	onUnload(() => {
		stopBluetoothDevicesDiscovery()
	})
	//得到系统信息
	const getSystemInfo = () => {
		uni.getSystemInfo({
			success(res) {
				platform.value = res.platform
			}
		})
	}
	//查询蓝牙设备
	const searchPrint = () => {
		uni.closeBluetoothAdapter({
			complete: () => {
				//初始化蓝牙
				uni.openBluetoothAdapter({
					success: () => {
						startBluetoothDevicesDiscovery();
					},
					fail: (res) => {
						if (res.errCode === 10001) {
							console.log('蓝牙未开启');
						} else {
							console.log('蓝牙初始化失败');
						}
					}
				})
			}
		});
	}
	//开始搜寻附近的蓝牙设备
	const startBluetoothDevicesDiscovery = () => {
		//停止蓝牙搜索
		stopBluetoothDevicesDiscovery();
		//开启蓝牙搜索
		uni.startBluetoothDevicesDiscovery({
			success: () => {
				//搜索，监听返回结果
				onBluetoothDeviceFound()
			},
			fail: (res) => {
				console.log(res, "搜索蓝牙失败");
			}
		});
	}
	//停止蓝牙搜索
	const stopBluetoothDevicesDiscovery = () => {
		uni.stopBluetoothDevicesDiscovery({
			success() {
				console.log('停止蓝牙搜索success')
			},
			fail(res) {
				console.log('停止蓝牙搜索fail', res)
			}
		})
	}
	//寻找到新设备的事件的回调函数
	const onBluetoothDeviceFound = () => {
		uni.onBluetoothDeviceFound((res) => {
			console.log('寻找到新设备的事件的回调函数', res);
			res.devices.forEach(device => {
				if (!device.name && !device.localName) {
					return
				}
				// 蓝牙数组
				const foundDevices = devices.value;
				const idx = findArrItem(foundDevices, device.deviceId);
				if (idx === -1) {
					foundDevices.push(device);
				} else {
					foundDevices[idx] = device;
				}
				const device1 = uni.getStorageSync("bindDevice");
				if (device1) {
					foundDevices.map((v) => {
						if (v.name == device1.name) {
							v.isBind = true;
						}
						return v
					})
				}
				devices.value = foundDevices
			})
		})
	}
	//重复搜索时，判断设备是否已经存在过滤掉
	const findArrItem = (arr, id) => {
		let index = -1;
		for (let i in arr) {
			if (arr[i].deviceId == id) {
				index = i;
			}
		}
		return index;
	}
	//绑定设备
	const setBindDevice = (item) => {
		uni.showLoading({
			title: "连接中...",
			mask: true
		})
		//创建连接，测试设备是否可以读，可写
		createBLEConnection1(item);
	}
	//低功耗蓝牙设备连接
	const createBLEConnection1 = (item, callback) => {
		uni.createBLEConnection({
			deviceId: item.deviceId,
			timeout: 15000,
			success: (res) => {
				console.log('蓝牙连接成功');
				bindDevice.value = item
				//如果蓝牙设备write没定义，说明是新设备需要执行
				if (platform.value == 'android') {
					if (item.write == undefined) {
						//检查该蓝牙设备是否有写入权限，并保存参数，以便发送数据
						setTimeout(() => {
							getBLEDeviceServices1(item.deviceId, callback);
						}, 3000)
					} else {
						callback ? callback() : ''
					}
				} else {
					getBLEDeviceServices1(item.deviceId, callback);
				}
			},
			fail: (res) => {
				uni.hideLoading()
				console.log("蓝牙连接失败:", res);
			}
		})
	}
	const getBLEDeviceServices1 = (deviceId, callback) => {
		uni.getBLEDeviceServices({
			deviceId,
			success: (res) => {
				console.log('获取蓝牙设备所有服务', res.services)
				for (let i = 0; i < res.services.length; i++) {
					if (res.services[i].isPrimary) {
						getBLEDeviceCharacteristics1(deviceId, res.services[i].uuid, callback)
						// return
					}
				}
			},
			fail: (res) => {
				uni.hideLoading()
				console.log("获取蓝牙服务失败：" + JSON.stringify(res))
			}
		})
	}

	//获取蓝牙设备某个服务中所有特征值(characteristic)
	const getBLEDeviceCharacteristics1 = (deviceId, serviceId, callback) => {
		uni.getBLEDeviceCharacteristics({
			deviceId,
			serviceId,
			success: (res) => {
				console.log('获取设备特征值', res.characteristics)
				for (let i = 0; i < res.characteristics.length; i++) {
					let item = res.characteristics[i];
					let _uuid = item.uuid;
					let bindDevices = bindDevice.value;
					// 读取低功耗蓝牙设备的特征值的二进制数据值 注意：必须设备的特征值支持 read 才可以成功调用。
					if (item.properties.read) {
						bindDevices.read = true;
					}
					//向低功耗蓝牙设备特征值中写入二进制数据。注意：必须设备的特征值支持 write 才可以成功调用。
					if (item.properties.write && !item.properties.notify) {
						bindDevices.serviceId = serviceId;
						bindDevices.characteristicId = _uuid;
						bindDevices.write = true;
						callback ? callback() : ''
					}
					//启用低功耗蓝牙设备特征值变化时的 notify 功能，使用characteristicValueChange事件
					if (item.properties.notify || item.properties.indicate) {
						uni.notifyBLECharacteristicValueChange({
							deviceId,
							serviceId,
							characteristicId: _uuid,
							state: true,
						})
					}
					//设置当前选中的蓝牙设备，包括是否可读写属性采集
					bindDevice.value = bindDevices;
					let oldDevices = devices.value;
					devices.value = oldDevices.map((v) => {
						if (v.name == bindDevices.name) {
							v.isBind = true;
						}
						return v
					})
					uni.setStorageSync("bindDevice", bindDevices);
				}
				if (uni.getStorageSync("bindDevice").characteristicId) {
					uni.showToast({
						title: "连接成功",
						icon: "success",
						complete() {
							setTimeout(() => {
								uni.navigateBack()
							}, 1500)
						}
					})
				}
			},
			fail(res) {
				console.error('获取特征值失败：', res)
			},
			complete() {
				uni.hideLoading()
			}
		})
	}
</script>

<style lang="scss">
	.refresh {
		display: flex;
		align-items: center;
		justify-content: space-between;
		width: 100%;
		height: 113rpx;
		background: #FFFFFF;
		border-radius: 0 0 20rpx 20rpx;
		font-size: 30rpx;
		color: #262626;
		padding: 0 30rpx;

		.icon {
			width: 28rpx;
			height: 28rpx;
		}
	}

	.print-loading {
		width: 100%;
		text-align: center;
		font-size: 30rpx;
		color: #262626;
		padding: 30rpx 0 0;
	}

	.me-list {
		width: 690rpx;
		background: #FFFFFF;
		border-radius: 20rpx 20rpx 20rpx 20rpx;
		margin: 30rpx auto;
		padding: 0 20rpx;

		.item {
			display: flex;
			align-items: center;
			justify-content: space-between;
			width: 100%;
			height: 112rpx;
			border-bottom: 1px solid rgba(37, 37, 37, .03);

			&:last-child {
				border-bottom: none;
			}

			.name {
				width: 500rpx;
				text-overflow: ellipsis;
				overflow: hidden;
				white-space: nowrap;
				font-size: 36rpx;
				color: #333333;
			}

			.status {
				display: flex;
				align-items: center;
				font-size: 30rpx;
				color: #808080;

				&.connect {
					color: #4D4D4D;
				}

				.icon {
					width: 27rpx;
					height: 27rpx;
					margin-left: 20rpx;
				}
			}
		}
	}
</style>
```


### ios微信小程序蓝牙打印机

```js
onLoad(options) {
  //获取系统信息
  const that = this;
  uni.getSystemInfo({
    success(res) {
      that.platform = res.platform
    }
  })
},
//低功耗蓝牙设备连接
createBLEConnection1() {
  const that = this;
  uni.createBLEConnection({
    deviceId: this.device.deviceId,
    timeout: 15000,
    success: () => {
      console.log('蓝牙连接成功');
      that.isConnect = true;
      // 微信小程序ios端每次打印都需要重新获取下设备服务id和特征id
      // #ifdef MP
      if (that.platform == 'ios') {
        that.getBLEDeviceServices1(that.device.deviceId)
      }
      // #endif
    },
    fail: (res) => {
      console.log("蓝牙连接失败:", res);
      if (res.errMsg.includes("already connect")) {
        that.isConnect = true;
      } else {
        that.isConnect = false;
      }
    }
  })
},
//获取蓝牙设备对应服务
getBLEDeviceServices1(deviceId) {
  const that = this;
  uni.getBLEDeviceServices({
    deviceId,
    success: (res) => {
      // console.log('获取蓝牙设备所有服务', res.services)
      for (let i = 0; i < res.services.length; i++) {
        if (res.services[i].isPrimary) {
          that.getBLEDeviceCharacteristics1(deviceId, res.services[i].uuid)
          // return
        }
      }
    },
    fail: (res) => {
      uni.hideLoading()
      console.log("获取蓝牙服务失败：" + JSON.stringify(res))
    }
  })
},
//获取蓝牙设备某个服务中所有特征值(characteristic)
getBLEDeviceCharacteristics1(deviceId, serviceId) {
  uni.getBLEDeviceCharacteristics({
    deviceId,
    serviceId,
    success: (res) => {
      // console.log('获取设备特征值', res.characteristics)
    },
    fail(res) {
      console.error('获取特征值失败：', res)
    },
    complete() {
      uni.hideLoading()
    }
  })
},
```