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
					<phone-search-list :phones="institutions" :hotPhones="hotInstitutions" @confirm="confirm" :stockCompanies="queryParams.stockCompanies"></phone-search-list>
				</block>
				<block  v-else>
					<scroll-view class="lione-items"  scroll-y="true">
							<view class="mli">
								<text class="text" v-for="(item,i) in listchild" :key="i" @tap="chooseItems(i,item)" :class="{'actives':(status[i1]===item.name || (!status[i1] && !i))}">{{item.short_name}}</text>
							</view>
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
		props: ['list', 'arr', 'institutions','hotInstitutions','queryParams'], //数组  arr
		data() {
			return {
				i1: null,
				i2: 0,
				show: false,
				listchild: [],
				newlist: [...this.list],
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
					this.status=[newVal.industries,newVal.rptTypes,'',newVal.reportDate,newVal.pageCount];
					if(!(newVal.industries || newVal.rptTypes || newVal.reportDate || newVal.pageCount)){
						//重置
						this.newlist=[...this.list];
					}else{
						if(!this.i1){
							//行业中任意行业，类别变成行业研究,点击全部行业，类别为全部类别
							this.newlist[1]=newVal.industries?'行研':this.list[1];
						}else if(this.i1===1){
							//用户直接选择“类别”中的某一项时，“行业”的默认值都是ALL（因为客户选择非“行业研究”类别时，“行业”选项是没有意义的）。
							newVal.rptTypes != '行研'?this.newlist[0] = this.list[0]:'';
						}
					}
					this.$forceUpdate();
				},
				deep: true
			}
		},
		methods: {
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
			chooseItems(i,item) {
				this.i2 = i;
				this.newlist[this.i1]=item.name?item.short_name:this.list[this.i1];
				this.$emit('chooseLike', [this.i1, item]);
				this.hide();
			},
			hide() {
				this.show = false;
			},
			confirm(selectedPhones){
				this.hide();
				this.$emit('handleConfirmSelect',selectedPhones)
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
		align-items: center;
		padding: 0 30upx 30upx 30upx;
		border-bottom: 1px solid #d5d5d5;
		color: #4a4a4a;
		.text{
			text-align: center;
			width: 22%;
			display: inline-block;
			margin-left: 4%;
			margin-top: 16upx;
			border: 1px solid #4a4a4a;
			border-radius: 2px;
			box-sizing: border-box;
			&:nth-child(4n+1){
				margin-left: 0;
			}
			&.actives {
				color: #fff;
				border-color: $uni-color-primary;
				background-color: $uni-color-primary;
				border-color: $uni-color-primary;
			}
		}
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

	.actives-title{
		color: #F5A623;
		border-bottom: none;
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
