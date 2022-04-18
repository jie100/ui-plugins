<template>
	<view class="content">
		<text class="example" @click="linkTo('linkage')"> 二级联动组件-linkage </text>
		<view class="example">
			<text class=""> 自动补全组件-autocomplete </text>
			<view class="result-box"  @click="linkTo('autocomplete')">
				<text class="result-text" :class="{'color-gray': !(initData.name || initData)}">{{ initData.name || initData || '请选择' }}</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 输入框初始值
				initData: '深圳',
			}
		},
		onLoad() {
			uni.$on('autoCompleteResult', this.getResult)
		},
		methods: {
			// 获取返回的数据---autocomplete组件
			getResult(data){
				console.log('get result from component:', data)
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
</script>

<style lang="scss" scoped>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		.color-gray{
			color: gray;
		}
		.example{
			margin-top: 2px;
			padding: 10px;
			width: 100vw;
			background-color: $uni-bg-color-hover;
			text-indent: 10px;
		}
		.result-box{
			margin-top: 5px;
			display: flex;
			justify-content: space-between;
			align-items: center;
			.result-text{
				margin: 10px;
				padding: 5px 10px;
				text-indent: 0;
				border: 1px solid lightgray;
				border-radius: 5px;
				flex-grow: 1;
			}
		}
	}
</style>
