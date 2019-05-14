<template>
	<view class="boxa">
		<view class="top_kbox">
			<block v-for="(item,i) in newlist" :key="i">
				<view class="ibox" @tap="alertnum(i)" :class="[i== i1?'actives-title':'']">
					<text class="uni_14">{{item}}</text>
					<image v-if="i != i1" class="ii" src="../../static/icon_arrow_b.png" mode=""></image>
					<image v-else class="ii" src="../../static/icon_arrow_b_selected.png" mode=""></image>
				</view>
			</block>
		</view>
		<view  :style="{display:show?'block':'none'}" class="list_boxs2">
			<view class="lione">
				<block v-if="i1===2">
					<view>
						<phone-search-list :phones="phones" @paramClick="paramClick" @confirm="confirm"></phone-search-list>
					</view>
				</block>
				<view v-else class="lione-items">
					<block v-for="(item,i) in listchild" :key="i">
						<view class="mli" @tap="chooseOne(i)" :class="[i== i2?'actives':'']">
							<text class="uni_14">{{item}}</text>
							<!-- <image v-if="i == i2" class="ii" src="/static/choose-Cade/choose-Cadecc.png" mode=""></image> -->
						</view>
					</block>
				</view>
			</view>
			<view class="hideA" @tap="hide">
			</view>
		</view>
		
	</view>
</template>

<script>
	import phoneSearchList from '@/components/phone-directory/phone-search-list.vue'
	export default {
		props: ['list', 'arr'], //数组  arr
		data() {
			return {
				i1: null,
				i2: 0,
				show: false,
				listchild: [],
				newlist: this.list,
				phones:{
					"A": [{
							"id": 56,
							"spell": "aba",
							"name": "阿坝"
					}, {
							"id": 57,
							"spell": "akesu",
							"name": "阿克苏"
					}, {
							"id": 58,
							"spell": "alashanmeng",
							"name": "阿拉善盟"
					}, {
							"id": 59,
							"spell": "aletai",
							"name": "阿勒泰"
					}, {
							"id": 60,
							"spell": "ali",
							"name": "阿里"
					}, {
							"id": 61,
							"spell": "ankang",
							"name": "安康"
					}, {
							"id": 62,
							"spell": "anqing",
							"name": "安庆"
					}, {
							"id": 63,
							"spell": "anshan",
							"name": "鞍山"
					}, {
							"id": 64,
							"spell": "anshun",
							"name": "安顺"
					}, {
							"id": 65,
							"spell": "anyang",
							"name": "安阳"
					}, {
							"id": 338,
							"spell": "acheng",
							"name": "阿城"
					}, {
							"id": 339,
							"spell": "anfu",
							"name": "安福"
					}, {
							"id": 340,
							"spell": "anji",
							"name": "安吉"
					}, {
							"id": 341,
							"spell": "anning",
							"name": "安宁"
					}, {
							"id": 342,
							"spell": "anqiu",
							"name": "安丘"
					}, {
							"id": 343,
							"spell": "anxi",
							"name": "安溪"
					}, {
							"id": 344,
							"spell": "anyi",
							"name": "安义"
					}, {
							"id": 345,
							"spell": "anyuan",
							"name": "安远"
					}],
					"B": [{
							"id": 1,
							"spell": "beijing",
							"name": "北京"
					}, {
							"id": 66,
							"spell": "baicheng",
							"name": "白城"
					}, {
							"id": 67,
							"spell": "baise",
							"name": "百色"
					}, {
							"id": 68,
							"spell": "baishan",
							"name": "白山"
					}]
				}
			}
		},
		components:{
			phoneSearchList
		},
		watch:{
      show:{
				handler(newVal,oldVal){
					this.$emit('onSelectDialog',newVal)
				}
			}
		},
		methods: {
			paramClick (e) {
				console.log(e)
			},
			alertnum(i) {
				this.show = !this.show;
				if (this.i1 != i) {
					this.listchild = [];
					this.i1 = i;
					this.listchild = this.arr[i];
					this.i2 = 0;
					let ins = this.listchild.indexOf(this.newlist[i]);
					if (ins > -1) {
						this.i2 = ins
					}
				}
			},
			chooseOne(i) {
				this.i2 = i;
				setTimeout(() => {
					this.hide()
				}, 300);
				// this.newlist[this.i1] = this.listchild[i];
				this.$emit('chooseLike', [this.i1, this.i2])
			},
			hide() {
				this.show = false;
			},
			confirm(){
				this.hide();
			}
		}
	}
</script>

<style lang="scss" scoped>
	.hideA {
		height: calc(100% - 310upx);
		position: fixed;
	}
  
	.mli {
		/* border: 1upx solid red; */
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 22upx 0;
		border-bottom: 1px solid #d5d5d5;
		color: #4a4a4a;
	}

	.lione {
		background-color: #fff;
		&-items{
			padding: 0 70upx 0 20upx;
		}
		/* height: 262upx; */
	}

	.list_boxs {
		background-color: rgba(84, 80, 80, 0.48);
		position: fixed;
		height: calc(100%);
		width: 100%;
		z-index: 88;
		transition: all .5s;
		transform: translateY(-120%);
	}
	.list_boxs2{
		background-color: rgba(84, 80, 80, 0.48);
		position: absolute;
		height: 100vh;
		width: 100%;
		z-index: 88;
		transform: translateY(0);
		transition: all .5s;
		left:0;
		top: 105rpx;
	}

	.ii {
		width: 16upx;
		height: 11upx;
		display: block;
		margin-left: 12upx;
	}

	.actives {
		color: $uni-color-primary;
		border-color: $uni-color-primary;
		&-title{
			color: #F5A623;
			border-bottom: none;
		}
	}

	.ibox {
		display: flex;
		align-items: center;
	}

	.top_kbox {
		display: flex;
		justify-content: space-between;
		align-items: center;
		background-color: #FFFFFF;
		padding: 28upx 5%;
		width: 90%;
	}
	.boxa{
		position: relative;
	}
</style>
