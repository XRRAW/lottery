土行孙埋点SDK
=======

## 版本说明

npm最新的包是1.1.0版本，

因为包还在完善的过程中 建议每次新下载包都使用最新版本

## 安装
```
* npm install txs-sdk -S
* yarn add txs-sdk
```
## 使用
在main.js/main.ts中引用, APP应用为`import { AppTracker} from 'txs-sdk`，其余为`import { Tracker} from 'txs-sdk`，然后执行[init](#初始化)

## 初始化
先new一个引入的类, App为Tracker，其余为AppTracker，[options](#Props)详见下方，然后全局挂载在vue实例上
Vue2中：
```js
const $tracker = new Tracker(options);
Vue.prototype.$tracker = $tracker;
```
Vue3中：
```js
const $tracker = new Tracker(options);
app.config.globalProperties.$tracker = $tracker;
```

## App中开启埋点
`仅限App中`，在App.vue的onLunch生命周期里，执行SDK中全局监听方法
Vue2中：
```js
this.$tracker.watchApp(func);
```
Vue3中：
```ts
import { getCurrentInstance } from "vue";
const { proxy }: any = getCurrentInstance();
proxy.$tracker.watchApp(func);
```
其中`func`为获取页面栈方法，比如uniapp中为`getCurrentPages`

## 设置userId
多用于登录后，对登录之前的数据进行用户绑定。
在登录完成之后，调用SDK中的设置userId方法，传入可作为用户唯一标识的参数
Vue2中：
```js
this.$tracker.setUserId(userId);
```
Vue3中：
```ts
import { getCurrentInstance } from "vue";
const { proxy }: any = getCurrentInstance();
proxy.$tracker.setUserId(userId);
```

## API

### Props

| 参数          | 说明                                                         | 类型   | 默认值                 | 平台差异
| ------------- | ------------------------------------------------------------ | :------: | ---------------------- | ---- |
| appid         | 应用Id                                                    | String | 无默认值，必填         |
| platform      | 应用平台类型                                       | String | 'web'      | App不需要传，其余可传值参考[platform](#platform)
| requestUrl    | SDK中全局请求接口前缀                                        | String | 'https://track.lyzhyun.com:18300'|
| enableHistoryTracker | 是否启用全局history跳转埋点                           | Boolean  | false              | App不支持
| enableHashTracker | 是否启用全局hash跳转埋点                                 | Boolean  | false              | App不支持
| api   | SDK里用到的接口      | Object | { sendApi: '/api/track', bindApi: '/api/track/bind'}    | [api](#api)详细看下方
| wx            | 微信小程序内嵌对象，wx对象或者uni对象都行                       | Object | 无默认值 | 微信小程序必填
| uni           | uniApp内嵌全局对象                                          | Object | 无默认值 | uniApp必填
| vueApp        | vue实例                                                    | Object | 无默认值 | 使用Js异常上报必填

### platform

| 名称 | 说明                                               |
| ---- | -------------------------------------------------- |
| wechat | 小程序 |
| web | web/H5 |

### api
* 埋点上报接口 sendApi: '/api/track' 
* 埋点绑定用户接口 bindApi: '/api/track/bind' 
* Js异常上报接口 sendBugApi: '/api/bug/report' 
* 微信小程序获取openId接口 wxOpenIdApi: 应用里的接口传入，微信小程序必传
关于Js异常上报：
`仅限web端`，api传入sendBugApi将自动开启Js异常上报，此时Sdk内部合并options使用的是深复制，所以其它用到的api也需要传入

### 方法

通过全局挂载的$tracker进行调用

| 方法名 | 说明         | 参数                              | 返回值 |
| ------ | ------------ | --------------------------------- | ------ |
| setUserId | 绑定用户Id | userId：作为用户的唯一标识 | -      |
| sendTracker | 手动上传埋点 | function(eventId: string, data: Object, eventInfo: Object)<br />
eventId: 事件名称<br />
data: 作为这次埋点的数据一并上传<br />
eventInfo：将被转为JSON字符串作为eventInfo的值上传 | -      |

## 开源协议
ISC