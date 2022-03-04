<template>
	<view class="content">
		<ui-popup-linkage :propsData="propsData" :objType="objType" :navHeight="300" @result="getResult">
			<!-- <view class="result-box">
				<text class="placeHolder" v-if="!(initData.subItem && initData.subItem.value)">请选择</text>
				<text v-else>{{ initData.item.value + '-' + initData.subItem.value }}</text>
			</view> -->
		</ui-popup-linkage>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 源数据，必传
				propsData: [], 
				// 初始化数据，若不需要初始化，则该参数可以不传
				initData: {
					item: {
						code: '22',
						value: '采购岗'
					},
					subItem: {
						code: '26',
						value: '采购专员'
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
					nameType: "value",
					// subType: "sub"
				},
				/* 示例数据
					1，此处的数据 idType 为"code", 因此需要修改objType的idType为"code"
					2，此处的数据 nameType 为"value", 因此需要修改objType的idType为"value"
					3，子数组名为"sub",与默认相同，则无需修改
				 */
				listData: [
					{
						"code":78,
						"value":"管理岗",
						"sub":[
							{"code":80,"pid":78,"sort":200,"value":"总经理"},
							{"code":81,"pid":78,"sort":190,"value":"总裁"},
							{"code":82,"pid":78,"sort":180,"value":"CEO"},
							{"code":79,"pid":78,"sort":170,"value":"董事长"},
							{"code":98,"pid":78,"sort":160,"value":"会长"},
							{"code":99,"pid":78,"sort":150,"value":"理事长"},
							{"code":100,"pid":78,"sort":140,"value":"秘书长"},
							{"code":83,"pid":78,"sort":130,"value":"副总经理"},
							{"code":101,"pid":78,"sort":120,"value":"副董事长"},
							{"code":102,"pid":78,"sort":110,"value":"副总裁"},
							{"code":103,"pid":78,"sort":100,"value":"副会长"},
							{"code":104,"pid":78,"sort":90,"value":"副理事长"},
							{"code":84,"pid":78,"sort":80,"value":"总裁助理"},
							{"code":105,"pid":78,"sort":70,"value":"理事"},
						],
					},
					{
						"code":85,
						"value":"销售岗",
						"sub":[
							{"code":86,"pid":85,"sort":100,"value":"销售总监"},
							{"code":87,"pid":85,"sort":90,"value":"销售经理"},
							{"code":88,"pid":85,"sort":80,"value":"大区总监"},
							{"code":89,"pid":85,"sort":70,"value":"大区经理"},
							{"code":91,"pid":85,"sort":60,"value":"销售工程师"},
							{"code":106,"pid":85,"sort":50,"value":"售前工程师"},
							{"code":107,"pid":85,"sort":40,"value":"销售主管"},
							{"code":92,"pid":85,"sort":30,"value":"销售专员"},
						],
					},
					{
						"code":22,
						"value":"采购岗",
						"sub":[
							{"code":26,"pid":22,"sort":100,"value":"采购专员"},
							{"code":23,"pid":22,"sort":100,"value":"采购总监"},
							{"code":24,"pid":22,"sort":100,"value":"采购经理"},
							{"code":25,"pid":22,"sort":100,"value":"采购主管"},
						],
					},
					{
						"code":71,
						"value":"市场岗",
						"sub":[
							{"code":73,"pid":71,"sort":100,"value":"市场经理"},
							{"code":74,"pid":71,"sort":100,"value":"市场专员"},
							{"code":95,"pid":71,"sort":0,"value":"市场总监"},
						],
					},
					{
						"code":27,
						"value":"技术岗",
						"sub":[
							{"code":28,"pid":27,"sort":100,"value":"研发总监"},
							{"code":29,"pid":27,"sort":100,"value":"研发经理"},
							{"code":30,"pid":27,"sort":100,"value":"技术总监"},
							{"code":31,"pid":27,"sort":100,"value":"总工程师"},
							{"code":32,"pid":27,"sort":100,"value":"技术主管"},
							{"code":33,"pid":27,"sort":100,"value":"研发工程师"},
							{"code":34,"pid":27,"sort":100,"value":"技术专员"},
							{"code":35,"pid":27,"sort":100,"value":"维护工程师"},
							{"code":36,"pid":27,"sort":100,"value":"售后工程师"},
							{"code":38,"pid":27,"sort":100,"value":"产品经理"},
							{"code":39,"pid":27,"sort":100,"value":"产品专员"},
							{"code":37,"pid":27,"sort":100,"value":"产品总监"},
						],
					},
					{
						"code":15,
						"value":"商务岗",
						"sub":[
							{"code":16,"pid":15,"sort":100,"value":"商务总监"},
							{"code":17,"pid":15,"sort":100,"value":"商务经理"},
							{"code":18,"pid":15,"sort":100,"value":"商务主管"},
							{"code":19,"pid":15,"sort":100,"value":"商务专员"},
							{"code":20,"pid":15,"sort":100,"value":"渠道经理"},
							{"code":21,"pid":15,"sort":100,"value":"渠道专员"},
							{"code":93,"pid":15,"sort":0,"value":"业务经理"},
							{"code":97,"pid":15,"sort":0,"value":"商务助理"},
							{"code":94,"pid":15,"sort":0,"value":"业务专员"},
						],
					},
					{
						"code":16,
						"value":"财务岗",
						"sub":[
							{"code":160,"pid":16,"sort":100,"value":"财务总监"},
							{"code":170,"pid":16,"sort":100,"value":"财务经理"},
							{"code":180,"pid":16,"sort":100,"value":"财务主管"},
							{"code":190,"pid":16,"sort":100,"value":"财务专员"}
						],
					},
					{
						"code":75,
						"value":"其他",
						"sub":[
							{"code":76,"pid":75,"sort":100,"value":"其他"},
						],
					},
				]
			}
		},
		onLoad() {
			this.changeListData()
		},
		methods: {
			// 获取返回的数据
			getResult(data){
				console.log('get result from component:', data)
				this.initData = data
			},
			
			changeListData(){
				let timer = setTimeout(()=>{
					this.propsData = this.listData
				},2000)
			}
		}
	}
</script>

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
