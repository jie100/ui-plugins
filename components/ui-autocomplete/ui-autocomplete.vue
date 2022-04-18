<template>
	<view>
		<view class="result-box">
			<uni-easyinput class="result-text" type="text" v-model="inputValue.name" :placeholder="placeHolderText"
				@input="loadData" @focus="focusSearch" :focus="autoFocus" @confirm="okTap" />
			<text v-if="inputValue.name" class="color-gray" @click="cancel">取消</text>
		</view>
		<view class="item-list" v-if="autoItems.length">
			<view class="item-opt" v-for="item in autoItems" :key="item.id" @tap="selectItems(item)">
				{{ item.name }}
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: "uiAutocomplete",
		props: {
			/* 必传，匹配到的数据--一维数组(每一项都需要有id和name,id没有可以为空) */
			propsData: {
				type: Object | Array,
				default: []
			},
			/* 选填，输入框初始值 */
			initData: {
				type: String,
				default: ''
			},
			/* 选填，提示语 */
			placeHolderText: {
				type: String,
				default: '请输入...'
			},
			/* 选填，是否自动获取焦点，默认false */
			autoFocus: {
				type: Boolean,
				default: false
			},
			/* 选题，是否在获焦时匹配，默认true */
			focusMatch: {
				type: Boolean,
				default: true
			},
			/* 选填，性能参数：自动补充开始长度，比如：值为2代表输入两个字才开始匹配，默认为0，即只要有输入就会开始匹配 */
			searchSize: {
				type: Number,
				default: 0
			},
			/* 选填，性能参数：节流间隔时间（毫秒），表示在一直输入的情况下，每隔多久请求一次，默认400，即400毫秒请求一次 */
			wait: {
				type: Number,
				default: 400
			}
		},
		data() {
			return {
				inputValue: {},
				autoItems: [],
				previous: 0, // 节流初始时间
			};
		},
		created() {
			this.inputValue.name = this.initData
		},
		methods: {
			/* 此处已做节流处理 */
			async loadData(data) {
				let now = Date.now();
				if (now - this.previous > this.wait) {
					if ( this.inputValue.name && this.inputValue.name.trim().length >= this.searchSize) {
						this.autoItems = await this.propsData(this.inputValue.name)
						this.previous = now;
					}
				}
			},
			cancel() {
				uni.navigateBack()
			},
			okTap(data) {
				this.inputValue.name = data?.detail?.value || data
				this.inputValue.id = ''
				uni.$emit('autoCompleteResult', this.inputValue)
				uni.navigateBack()
			},
			selectItems(item) {
				this.inputValue = item;
				// this.autoItems = []
				uni.$emit('autoCompleteResult', this.inputValue)
				uni.navigateBack()
			},
			async focusSearch(){
				if ( this.focusMatch && this.inputValue.name && this.inputValue.name.trim().length >= this.searchSize) {
					this.autoItems = await this.propsData(this.inputValue.name)
					// console.log( this.autoItems )
				}
			}
		}
	}
</script>

<style lang="scss" scoped>
	.result-box {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 10px;
		background-color: #eeeeee;
		border-bottom: 1px solid lightgray;

		.result-text {
			border-radius: 5px;
			flex-grow: 1;
		}

		.color-gray {
			color: gray;
			padding: 5px 10px;
			margin-left: 10px;
		}
	}

	.item-list {
		padding: 10px 5px;

		.item-opt {
			padding: 10px;
			border-bottom: 1px solid lightgray;
		}
	}
</style>
