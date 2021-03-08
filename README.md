# 全局函数

> `flutter` 结果会通过全局函数调用的方式传递给前端，如直接调用`window.bridge.handleCallBack(data: CallBackData)`<br/>所以web端需封装个全局类，项目初始化需要挂载到`window` 不然`app`会报错



## 发送指令函数(*web端调用客户端*)
> `RC.postMessage`(由web端自由封装，最终发送指令需要调`window.RC.postMessage()`) 参数说明

| 属性   | 类型                 | 默认值 | 说明                                                         |
| ------ | -------------------- | ------ | ------------------------------------------------------------ |
| data   | {[key: string]: any} |        | 这个参数websdk需要把相关的除<br />`method`、`key`等必要参数之外的所有数据<br />都封装到data里。如router.push的`url`参数 |
| key    | string               |        | 双端通信绑定的唯一key，可以用uuid                            |
| method | stirng               |        | 具体操作的指令                                               |

> 注意``window.RC.postMessage()`` 需要发送串行化后的字符串，JSON.stringify后的参数

## 回调函数(*客户端调用web端*)

> `handleCallBack(data)` 参数说明

| 属性    | 类型   | 默认值             | 说明                                   |
| ------- | ------ | ------------------ | -------------------------------------- |
| success | bool   | false              | 是否调用成功                           |
| code    | int    | 1                  | 0 - 成功；1 - 失败；具体失败code皆 > 0 |
| msg     | string | "网络繁忙，请重试"    | 信息描述                           |
| args    | any [] | []                 | 返回的参数携带信息                     |
| method  | string | "error_method"     | web端传过来的具体操作指令              |
| key     | string | "error_key"        | web端传过来的具体操作指令绑定的key     |


---
---

# 路由	-	`router`

## `push`	
> 保留当前页面，跳转到应用内的某个页面, 使用 `router.back` 可以返回到原页面。
#### 参数 Object [routeData](#routeData)

## `back`
> 关闭当前页面，返回上一页面或多级页面。可通过 `router.getCurrentPages` 获取当前的页面栈，决定需要返回几层。
#### 参数 Object
| 属性  | 类型   | 默认值 | 必填 | 说明                                                    |
| ----- | ------ | ------ | ---- | ------------------------------------------------------- |
| delta | number | 1      | 否   | 返回的页面数，如果 delta 大于现有页面数，则返回到首页。 |

## `redirect`
> 关闭当前页面，跳转到应用内的某个页面

#### 参数 Object [routeData](#routeData)

## `switch`
> 跳转到 tabBar 页面，并关闭其他所有非 tabBar 页面
#### 参数 Object object
| 属性  | 类型   | 默认值 | 必填 | 说明                                                    |
| ----- | ------ | ------ | ---- | ------------------------------------------------------- |
| index | number | 0 | 否 | 要跳转到的tabBar索引 |

## `getCurrentPages`

> 获取当前页面栈。数组中第一个元素tab页后第一次跳的页面，最后一个元素为当前页面。
#### 参数 Array([routeData](#routeData))

### routeData
>路由中作为参数传递的`url`, 会遵循以下逻辑跳转
>1. 如果 以`http://`或者`https://`开头, 认为是外部链接跳转.
>2. 如果 以`/`开头, 认为是系统内的web页面跳转,补足baseUrl进行跳转.
>3. 如果 不以`/`开头, 会先根据url判断是否是已注册的原生页面,是的话进行原生跳转, 否则在前边补充`/`走第2步.
  所以, 确定不是跳往原生页面和外链时, 最好都以`/`开头.

| 属性 | 类型   | 默认值 | 必填 | 说明                                                         |
| ---- | ------ | :----- | ---- | :----------------------------------------------------------- |
| url  | string | -- | 是   | 需要跳转的应用内网络页面的路径 (代码包路径), 路径后可以带参数。<br />参数与路径之间使用 `?` 分隔，参数键与参数值用 `=` 相连，不同参数用 `&` 分隔；<br />如 'path?key=value&key2=value2' |
| title  | string | -- | 否   | 顶部状态栏标题 |
| bgColor  | string | -- | 否   | 顶部状态栏背景色(比如: '#F78182') |
| transition  | string | fadeIn | 否   | 路由过渡动画,可选值(fadeIn, cupertino) |
| hasTopBar  | boolean | true | 否   | 是否保留顶部状态栏 |
| isBack  | boolean | true | 否   | 有无返回按钮 |

---
---

# 支付 - `pay`
> 调用微信支付
#### 返回参数 (Object)
| 属性 | 类型  | 说明 |
| ---- | ------ | ---- |
| success  | boolean | 返回当前支付状态(成功:true, 失败: false) |
| msg  | boolean | 提示性文字 |


---
---

# 分享 - `share`
> 调用微信分享
#### 参数 (Object)
| 属性 | 类型 | 默认值 | 必填 | 说明 |
| --- | --- | ----- | --- | --- |
| type | String | 'text' | 否 | 分享类型: 文本(`text`) 小程序(`miniProgram`) 图片(`image`) 音乐(`music`) 视频(`video`) 网页(`web`) 文件(`file`) |
| shareScene | String | 'session' | 否 | 分享场景: 会话(`session`) 朋友圈(`timeline`) 收藏(`favorite`) |
| source | String | - | 类型为`text`, `image`, `file`时必填 | 分享源: [适应分享类型为: 文本(文本内容) 图片(图片地址) 文件(文件地址)] |
| thumbnail | String | - | 否 | 缩略图: 缩略图url [适应分享类型为: 小程序 图片 音乐 视频 网页 文件] |
| title | String | - | 否 | 标题: [适应分享类型为: 文本 小程序 图片 音乐 视频 网页 文件] |
| description | String | - | 否 | 描述: [适应分享类型为: 文本 小程序 图片 音乐 视频 网页 文件] |
| messageExt | String | - | 否 | 额外信息: 微信跳回时会带上 [适应分享类型为: 文本 小程序 图片 音乐 视频 网页 文件] |
| mediaTagName | String | - | 否 | 媒体标签名: [适应分享类型为: 文本 小程序 图片 音乐 视频 网页 文件] |
| messageAction | String | - | 否 | 消息行为?(unknown): [适应分享类型为: 小程序 图片 音乐 视频 网页 文件] |
| compressThumbnail | bool | - | 否 | 是否压缩缩略图: [适应分享类型为: 文本 小程序 图片 音乐 视频 网页 文件] |
| webPage | String | - | 类型为`web`时必填 | 页面地址: [适应分享类型为: 小程序(兼容低版本的网页链接) 网页] |
| shareMiniProgramType | String | - | 否 | 分享小程序类型: 正式版(release) 测试版(test) 预览版(preview)[适应分享类型为: 小程序] |
| userName | String | - | 否 | 小程序的用户名: [适应分享类型为: 小程序] |
| path | String | - | 否 | 小程序的页面路径: [适应分享类型为: 小程序] |
| hdImagePath | String | - | 否 | 小程序新版本的预览图二进制数据，6.5.9及以上版本微信客户端支持: [适应分享类型为: 小程序] |
| withShareTicket | bool | - | 否 | 是否使用带shareTicket的分享: [适应分享类型为: 小程序] |
| musicUrl | String | - | 否 | 音频网页的URL地址: [适应分享类型为: 音乐] |
| musicDataUrl | String | - | 否 | 音频数据的URL地址: [适应分享类型为: 音乐] |
| musicLowBandUrl | String | - | 否 | 音频网页的URL地址(供低带宽时使用): [适应分享类型为: 音乐] |
| musicLowBandDataUrl | String | - | 否 | 音频数据的URL地址(供低带宽时使用): [适应分享类型为: 音乐] |
| videoUrl | String | - | 否 | 视频链接: [适应分享类型为: 视频] |
| videoLowBandUrl | String | - | 否 | 视频链接(供低带宽时使用): [适应分享类型为: 视频] |

#### 返回参数 (Object)
| 属性 | 类型  | 说明 |
| ---- | ------ | ---- |
| success  | boolean | 返回分享状态(分享成功后用户未返回应用时, share方法无回调) |
| msg  | boolean | 提示性文字 |


---
---

# 其它信息

## `getToken`
> 获取当前用户token, 未登录则返回''
#### 返回参数 (String)

## `getUser`
> 获取当前用户数据
#### 返回参数 (Object)
| 属性 | 类型  | 说明 |
| ---- | ------ | ---- |
| token  | string | 用户唯一标识 |
| userCode  | string | 用户编码 |

## `getDevice`
> 获取当前设备相关数据
#### 返回参数 (Object)
| 属性 | 类型  | 说明 |
| ---- | ------ | ---- |
| width  | number | 屏幕宽度 |
| height  | number | 屏幕高度 |
| statusBarHeight  | number | 顶部状态栏高度 |
| bottomBarHeight  | number | 底部状态栏高度 |
| devicePixelRatio  | number | 设备与CSS像素的分辨率比率 |
| textScaleFactor  | number | 文字缩放倍率 |

---
