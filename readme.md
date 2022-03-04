
## 基于 uni-popup的双联动组件
> **组件名：uni-popup-linkage**

> **关联组件： `uni-popup`、`uni-transition`**

> **代码块： `uPopupLinkage`**

基于uni-popup的双联动tab弹框，实现了双向划、自动置顶，支持用户自定义传入数据的keyname，H5端做了去滚动条处理，
组件实现简单，查看demo请查看 /pages/examples/linkage.vue

注意：本组件基于 uni-popup、uni-transition实现弹框与滚动动画，请使用前先确保该两组件已下载，否则无法执行

### 基本用法

**示例**

```html
<ui-popup-linkage :propsData="propsData" @result="getResult"></ui-popup-linkage>
```

```javascript
export default {
 data() {
  return {
   // 源数据，必传
   propsData: [ 
        {			
           "id":78,
           "name":"管理岗",
           "sub":[
             { "id":80,  "name":"总经理" },
             {"id":81,"name":"总裁" }
		    ]
		},
        {
            "id":75,
            "name":"其他",
            "sub":[
                {"id":76,"name":"其他"}
            ]
		}
	],
    resultData: {}
  },
  methods:{
    // 获取返回的数据
    getResult(data){
        console.log('get result from component:', data)
		this.resultData = data
	}
  }
}

```

### 自定义触发器以及数据格式 

**示例**

```html
<ui-popup-linkage :propsData="propsData" :initData="initData" :objType="objType" navHeight="400" @result="getResult">
	<view class="result-box">
		<text class="placeHolder" v-if="!(initData.subItem && initData.subItem.value)">请选择</text>
		<text v-else>{{ initData.item.value + '-' + initData.subItem.value }}</text>
	</view>
</ui-popup-linkage>
```

```javascript
export default {
 data() {
  return {
	  // 初始化数据，若不需要初始化，则该参数可以不传
	  initData: {
	  	item: {
	  		code: '78',
	  		value: '管理岗'
	  	},
	  	subItem: {
	  		code: '81',
	  		value: '总裁'
	  	}
	  },
	  /* 定义数据格式，要求大类数据与子类数据中对应的keyname相同(比如都为id、name或者都为code、value)。
	  	默认值：idType为"id"，nameType为"name"，"subType"为"sub"。哪个不同修改哪个，如下：
	  	1，因为示例数据中idType为"code"，则修改"idType"的值为"code"
	  	2，因为示例数据中nameType为"value"，则修改"idType"的值为"value"
	  	3，subType的值与默认值相同，则无需修改，可以不传
	  	所以：只需要修改与默认值不同的keyname即可，相同的可以不传，若都相同，则该参数可以不传
	   */
	  objType: {
	  	idType: "code",
	  	nameType: "value"
	  },
	  // 源数据，必传
	  propsData: [ 
	  	{			
	  	   "code":78,
			   "value":"管理岗",
	  	   "sub":[
	  		 { "code":80,  "value":"总经理" },
	  		 {"code":81,"value":"总裁" }
	  		]
	  	},
	  	{
	  		"code":75,
	  		"value":"其他",
	  		"sub":[
	  			{"code":76,"value":"其他"}
	  		]
	  	}
	  ],
  },
  methods:{
    // 获取返回的数据
    getResult(data){
        console.log('get result from component:', data)
		this.initData = data
	}
  }
}
```


```style
<style lang="scss" scoped>
	.result-box{
		display: block;
		width: 100vw;
		height: 30px;
		line-height: 30px;
		text-align: center;
		background-color: #eeeeee;
		.placeHolder{
			color: gray;
		}
	}
</style>
```
### 更多用法请查看demo /pages/examples/linkage.vue