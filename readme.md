
## 基于uni-easyinput的自动提示组件
> **组件名：ui-autocomplete**

> **关联组件： `uni-easyinput`、`uni-icons`**

> **代码块： `uiAutocomplete`**

输入自动匹配组件，实现了“节流操作”以及“最低匹配长度控制”，可自定义节流时间以及匹配开始长度，一般用于新页面中使用，组件实现简单


注意：本组件基于uni-easyinput、uni-icons实现输入以及清楚输入记录效果，使用前请先确保这两个组件已下载，否则无法执行

### 基本用法

**父页面示例**
```html
<view @click="linkTo('autocomplete')">
	<text class="result-text" :class="{'color-gray': !(initData.name || initData)}">{{ initData.name || initData || '请选择' }}</text>
</view>
```

```javascript
export default {
 data() {
	return {
	 // 输入框初始值
	 initData: '',
	}
  },
  onLoad() {
	// 页面通讯-监听设置---此处很重要，否则无法接收到选择结果
  	uni.$on('autoCompleteResult', this.getResult)
  },
  methods:{
    // 获取返回的数据
    getResult(data){
    	this.initData = data
    },
    linkTo(example){
    	const keyWords = this.initData.name || this.initData
    	uni.navigateTo({
    		url: '/pages/examples/' + example + '?key='+ keyWords
    	})
    }
  }
}
```

**子页面示例**

```html
<ui-autocomplete :propsData="onloadData" :initData="initData"></ui-autocomplete>
```

```javascript
export default {
 data() {
  return {
	  // 存储输入框初始值(来自于被跳转页面的路由传参)
	  initData: '',
	 }
  },
  onLoad(options) {
  	this.initData = options.key
  },
  methods:{
    onloadData(key){
    	return this.getAutoData(key)
    },
    getAutoData(key){
    	/* 异步获取 */
    	return new Promise((resolve, reject) => {
    		let data = [
    			{'id':123,'name':'深圳迈瑞股份有限公司'},
    			{'id':223,'name':'深圳腾讯网络科技股份有限公司'},
    			{'id':323,'name':'深圳华为有限公司'}
    		]
    		resolve(data)
    	})
    	/* 同步获取 */
    	/* return [
    			{'id':123,'name':'深圳迈瑞股份有限公司'},
    			{'id':223,'name':'深圳腾讯网络科技股份有限公司'},
    			{'id':323,'name':'深圳华为有限公司'}
    		] */
    }
  }
}
```
**注意：在父页面设置autoCompleteResult侦听是必须的，用来接收最终结果**


## API

#### autocomplete Props 

|属性名|类型|默认值|是否必传|说明|
|:-:|:-:|:-:|:-:|:-:|
|propsData|Function|[]|是|返回一个一维数组的函数，数组每一项必须有id、name|
|initData|String|''|否|输入框初始值|
|placeHolderText|String|'请输入...'|否|未输入时的提示语|
|autoFocus|Boolean|false|否|是否自动获取焦点|
|focusMatch|Boolean|true|否|是否在获焦时匹配|
|searchSize|Number|0|否|性能参数：自动补充开始长度（即从输入第几个字开始匹配）|
|wait|Number|400|否|性能参数：节流间隔时间（毫秒）|

#### autocomplete Events

|事件称名|说明|返回值|备注|
|:-:|:-:|:-:|:-:|
|autoCompleteResult|页面通讯事件|e={id: Number OR String, name: String}| 必须在父页面设置此侦听，返回值为最终选择结果 |

### 更多用法[请查看](https://github.com/jie100/ui-plugins.git)-/pages/index/index.vue