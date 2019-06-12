<template>
	<view class="boxa">
		<view class="top_kbox">
			<block v-for="(item,i) in newlist" :key="i">
				<view class="ibox" @tap="alertnum(i)" :class="[show && i== i1?'actives-title':'']">
					<text class="uni_14">{{item}}</text>
					<image v-if="!show || i != i1" class="ii" src="../../static/icon_arrow_b.png" mode=""></image>
					<image v-else class="ii" src="../../static/icon_arrow_b_selected.png" mode=""></image>
				</view>
			</block>
		</view>
		<view  :style="{display:show?'block':'none'}" class="list_boxs2">
			<view class="lione">
				<block v-if="i1===2">
					<phone-search-list  :phones="institutions" @paramClick="paramClick" @confirm="confirm" :stockCompanies="queryParams.stockCompanies"></phone-search-list>
				</block>
				<block  v-else>
					<scroll-view class="lione-items"  scroll-y="true">
						<block v-for="(item,i) in listchild" :key="i">
							<view class="mli" @tap="chooseItems(i)" :class="{'actives':(status[i1]===item || (!status[i1] && !i))}">
								<text class="uni_14">{{item}}</text>
							</view>
						</block>
					</scroll-view>
				</block>
			</view>
			<view class="hideA" @tap="hide">
			</view>
		</view>
		
	</view>
</template>

<script>
	import phoneSearchList from '@/components/phone-directory/phone-search-list.vue'
	export default {
		props: ['list', 'arr', 'institutions','queryParams'], //数组  arr
		data() {
			return {
				i1: null,
				i2: 0,
				show: false,
				listchild: [],
				newlist: this.list,
				status: []
			}
		},
		components:{
			phoneSearchList
		},
		onLoad(){
				
		},
		watch:{
      show:{
				handler(newVal,oldVal){
					this.$emit('onSelectDialog',newVal)
				}
			},
			queryParams: {
				handler(newVal,oldVal){
					const DATE_MAP={"1m":"<1M",
													"3m":"<3M",
													"6m":"<6M",
													"1Y":"<1Y",
												};					
					const PAGE_MAP={"1p":"1-5P",
													"6p":"6-20P",
													"20p":">20P"
												};
					this.status[0]=newVal.industries;
					this.status[1]=newVal.rptTypes;
					this.status[3]=DATE_MAP[newVal.reportDate];
					this.status[4]=PAGE_MAP[newVal.pageCount];
					this.$forceUpdate();
				},
				deep: true
			}
		},
		methods: {
			paramClick (value) {
				this.$emit('chooseCheckBox',value);
			},
			alertnum(i) {
				if (this.i1 != i) {
					this.show=true;
					this.listchild = [];
					this.i1 = i;
					this.listchild = this.arr[i];
					this.i2 = 0;
					let ins = this.listchild.indexOf(this.newlist[i]);
					if (ins > -1) {
						this.i2 = ins
					}
				}else{
					this.show=!this.show;
				}
			},
			chooseItems(i) {
				this.i2 = i;
				this.$emit('chooseLike', [this.i1, this.i2]);
				this.hide();
			},
			hide() {
				this.show = false;
			},
			confirm(company=""){
				this.hide();
				this.$emit('handleConfirmSelect',company)
			}
		}
	}
</script>

<style lang="scss" scoped>
	.hideA {
		height: calc(100% - 310upx);
		position: fixed;
		width:100%;
	}
	.lione-items{
		max-height: 660upx;
		overflow: auto;
	}
  
	.mli {
		/* border: 1upx solid red; */
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 22upx 70upx 22upx 20upx;
		border-bottom: 1px solid #d5d5d5;
		color: #4a4a4a;
	}

	.lione {
		background-color: #fff;
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
		color: #fff;
		border-color: $uni-color-primary;
		background-color: $uni-color-primary;
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
