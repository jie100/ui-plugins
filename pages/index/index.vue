<template>
	<view class="content">
		<text class="example" @click="linkTo('linkage')"> 二级联动组件-linkage </text>
		<text class="example"> 自动补全组件-autocomplete </text>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'Hello'
			}
		},
		onLoad() {

		},
		methods: {
			linkTo(example){
				uni.navigateTo({
					url: '/pages/examples/' + example
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
		.example{
			margin-top: 2px;
			padding: 10px;
			width: 100vw;
			background-color: $uni-bg-color-hover;
			text-indent: 10px;
		}
	}

</style>
