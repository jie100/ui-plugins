<template>
	<ui-autocomplete :propsData="onloadData" :initData="initData"></ui-autocomplete>
</template>

<script>
	export default {
		data() {
			return {
				// 存储输入框初始值(来自于被跳转页面的路由传参)
				initData: '',
			}
		},
		onLoad(options) {
			// console.log('初始值：', options)
			this.initData = options.key
		},
		methods: {
			onloadData(key){
				console.log('输入的关键字：', key)
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
</script>

<style lang="scss" scoped>

</style>
