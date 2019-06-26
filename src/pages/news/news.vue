<template>
  <cmd-page-body :backgroundColor="$uni-bg-color">
    <view class="news" :class="{'no-scroll':isSelectDialogShow}"> 
      <!-- 顶级tab -->
      <wuc-tab
        :tab-list="tabList"
        :tabCur.sync="TabCur"
        tab-class="text-center fixedit"
        style="position: relative;"
        @change="tabChangeTop"
      ></wuc-tab>
      <!-- tab内容区 -->
      <div class="cu-bar bg-white solid-bottom" style="margin-top:100upx">
        <div class="search-content" v-if="!TabCur">
          <block>
            <view class="search-box fixedit">
              <mSearch
                placeholder="输入您想找的关键字"
                :mode="2"
                button="inside"
                @search="doSearch($event)"
                class="input-block"
                ref="input"
              ></mSearch>
              <text @click="resetSearch" class="btn-reset">重置</text>
            </view>
            <ChooseLits :list="list" :arr="selectList" @chooseLike="chooseLike" @onSelectDialog="onSelectDialog" :institutions="institutions" @handleConfirmSelect="handleConfirmSelect" :hotInstitutions="hotInstitutions" :queryParams="queryParams"></ChooseLits>
            <!-- tab项 -->
            <view>
              <view class="fixedit">
                <scroll-view
                  class="grace-tab-title grace-center"
                  scroll-x="true"
                  id="grace-tab-title"
                >
                  <view class="grace-view-tab">
                    <view
                      v-for="(cate, index) in categories"
                      :key="index"
                      :data-cateid="cate.cateid"
                      :data-index="index"
                      :class="[cateCurrentIndex == index ? 'grace-tab-current' : '']"
                      @tap="tabChangeBottom(cate)"
                    >
                      <text class="grace-tab-text">
                        {{cate.name}}
                      </text>
                    </view>
                  </view>
                  <view class="sort-block">
                    <text @click="sort(0)" :class="{'active':sortCheckedIndex===0}">按相关性</text>
                    <text @click="sort(1)" :class="{'active':sortCheckedIndex===1}">按时间</text>
                  </view>
                </scroll-view>
              </view>
              <!-- 内容区 -->
              <view class="grace-news-list">
                <block v-if="artList.length">
                  <block v-for="(item, index) in artList" :key="index">
                    <navigator :url="'./detail?id=' + item.id+'&queryType='+queryParams.queryType" open-type="navigate" class="grace-news-list-items">
                      <view class="grace-news-list-img-news">
                        <view class="grace-news-list-img-big"  v-if="cateCurrentIndex===2">
                          <image :src="`https://api.qxsearch.net/api/res/image/${item.parse_chart_filepath}`" mode="widthFix" class="img"></image>
                          <view class="icon_magnifier" @click.stop="magnifierImg(`https://api.qxsearch.net/api/res/image/${item.parse_chart_filepath}`)">
                            <image
                              src="../../static/icon_magnifier.png"
                              class="img"
                            ></image>
                          </view>
                        </view>
                        <view class="grace-news-list-info" :class="{'chartSelected':cateCurrentIndex===2}">
                          <view class="grace-news-list-title-main">
                            <span style="float:left;">
                              <text class="btn" v-if="cateCurrentIndex!==2" style="margin-right: 8upx;">{{item.type_short_name}}</text>
                              <text class="btn" v-if="cateCurrentIndex!==2 && item.industry_short_name" style="margin-right: 8upx;">{{item.industry_short_name}}</text>
                              <text class="btn" v-if="cateCurrentIndex!==2 && item.company" style="margin-right: 8upx;">{{item.company}}</text>
                            </span>
                            <rich-text :nodes="item.parse_chart_title" class="title-text chart-title" v-if="item.parse_chart_title"></rich-text>
                            <rich-text :nodes="item.parse_title" class="title-text title"></rich-text>
                          </view>
                          <view class="grace-news-list-title-desc" v-if="cateCurrentIndex!==1 && item.abstractText">
                            <view style="text-align: right;">
                              <text class="btn" v-if="cateCurrentIndex===2" style="margin-right: 8upx;">{{item.type_short_name}}</text>
                              <text class="btn" v-if="cateCurrentIndex===2 && item.industry_short_name" style="margin-right: 8upx;">{{item.industry_short_name}}</text>
                              <text class="btn" v-if="cateCurrentIndex===2 && item.company" style="margin-right: 8upx;">{{item.company}}</text>
                            </view>
                            <rich-text :nodes="item.abstractText.replace(/[\r\n]/g,'')" class="desc-text"></rich-text>
                          </view>
                        </view>
                      </view>
                      <view class="flex-items">
                        <view class="item">
                          <image src="../../static/icon_building.png" class="item-img"></image>
                          <text class="item-text">{{item.parse_orgnization}}</text>
                        </view>
                        <view class="item">
                          <image src="../../static/icon_person.png" class="item-img"></image>
                          <text class="item-text" v-if="item.parse_authors">{{item.parse_authors[0]}}</text>
                        </view>
                        <view class="item">
                          <image src="../../static/icon_page.png" class="item-img"></image>
                          <text class="item-text">{{item.parse_pagecount}}P</text>
                        </view>
                        <view class="item">
                          <text class="item-text">{{item.parse_reportdate}}</text>
                        </view>
                      </view>
                    </navigator>
                  </block>
                </block>
                <view v-if="!loadingStatus && !artList.length" class="empty">
                  暂无相关数据！
                </view>
              </view>
            </view>
          </block>
          
        </div>
        <div v-else class="selection-content">
          敬请期待...
        </div>
      </div>
      <!-- 图片放大弹窗 -->
      <min-modal ref="modal">
        <img
          :src="bigImgUrl"
          style="width:100%;height:100%;"
        >
      </min-modal>
    </view>
  </cmd-page-body>
</template>
<script>
import cmdPageBody from "@/components/cmd-page-body/cmd-page-body.vue";
import wucTab from "@/components/wuc-tab/wuc-tab.vue";
import ChooseLits from "@/components/choose-Cade/choose-Cade.vue";
import mSearch from "@/components/mehaotian-search/mehaotian-search.vue";
import minModal from "@/components/min-modal/min-modal";
import qs from 'qs';
let page = 0;
export default {
  components: {
    cmdPageBody,
    wucTab,
    ChooseLits,
    mSearch,
    minModal
  },
  data() {
    return {
      sortCheckedIndex: 0,
      isSelectDialogShow: false,
      tabList: [{ name: "研报查询" }, { name: "研报精选" }], 
      TabCur: 0, //一级tab选中项
      list: ["行业", "类别", "机构", "日期", "页数"], 
      selectList: [
        [{
          short_name: '全部',
          name: ''
        }],
        [{
          short_name: '全部',
          name: ''
        }],
        [],
        [{
          short_name: '全部',
          name: ''
        },{
          short_name: '1个月以内',
          name: '1m'
        },{
          short_name: '3个月以内',
          name: '3m'
        },{
          short_name: '6个月以内',
          name: '6m'
        },{
          short_name: '1年以内',
          name: '1Y'
        }],
        [{
          short_name: '全部',
          name: ''
        },{
          short_name: '1-5页',
          name: '1p'
        },{
          short_name: '6-20页',
          name: '6p'
        },{
          short_name: '20页以上',
          name: '20p'
        }]
      ],
      institutions: {},//机构
      hotInstitutions: [],//热门机构
      categories: [
        { cateid: 0, name: "摘要", value: 'keypoint' },
        { cateid: 1, name: "正文", value: 'content' },
        { cateid: 2, name: "图表", value: 'chart' }
      ],
      cateCurrentIndex: 0,
      artList: [],//研报列表
      total: 0,
      queryParams: {
        industries: "",
        rptTypes: "",
        stockCompanies: "",
        reportDate: "",
        pageCount: "",
        queryType: "keypoint",
        content: "",
      },//查询参数
      bigImgUrl: '',
      loadingStatus: true

    };
  },
  onLoad() {
    this.getNewsList();
    this.getIndustry();
    this.getType();
    this.getCompany();
  },
  // 加载更多
  onReachBottom: function() {
    this.getNewsList();
  },
  watch: {
    queryParams: {
      handler(newVal, oldVal) {
        this.artList = [];
        page = 0;
        this.getNewsList();
      },
      deep: true,

    }
  },
  methods: {
    /**
     * 排序
     */
    sort(type){
      this.sortCheckedIndex = type;
      this.artList = [];
      page = 0;
      this.getNewsList();
    },
    /**
     * 获取行业
     */
    async getIndustry () {
      var [error, res] = await uni.request({
          url: 'https://api.qxsearch.net/api/res/industry',
          method: "GET", 
          header: {
            'Content-Type' : 'text/plain;charset=utf-8'
          },
      });
      res.data.hits.map(item=>{
        this.selectList[0].push(item)
      })
    },
    /**
     * 获取类别     
     */
    async getType () {
      var [error, res] = await uni.request({
          url: 'https://api.qxsearch.net/api/res/type'
      });
      res.data.hits.map(item=>{
        this.selectList[1].push(item)
      })
    },
    /**
     * 获取机构
     */
    async getCompany () {
      var [error, res] = await uni.request({
          url: 'https://api.qxsearch.net/api/res/company'
      });
      let lists = res.data.hits;
      let keys = [];
      lists.map(v=>keys.push(v.firstChar));
      keys = [...new Set(keys)].sort(); //顺序获取首字母
      keys.map(u=>{
         let target=lists.filter(v=>v.firstChar===u);
         this.institutions[u]=target;
      })
      this.hotInstitutions = res.data.hots;
    },
    /**
     * 下拉菜单是否展示
     */
    onSelectDialog(status){
      this.isSelectDialogShow = status
    },
    /**
     * tab切换
     */
    tabChangeTop(index) {
      this.TabCur = index;
    },
    /**
     * select选择
     */
    chooseLike(key) {
      let selectedIndex=key[0];
      let value = key[1].name;
      let currentKey = Object.keys(this.queryParams)[key[0]];
      this.queryParams[currentKey]=value;
      if(!selectedIndex){
        //行业中任意行业，类别变成行业研究,点击全部行业，类别为全部类别
        this.queryParams.rptTypes=value?'行业研究':'';
      }else if(selectedIndex===1){
        //用户直接选择“类别”中的某一项时，“行业”的默认值都是ALL（因为客户选择非“行业研究”类别时，“行业”选项是没有意义的）。
        value != '行业研究'?this.queryParams.industries = '':'';
      }
    },
    chooseCheckBox(value){
      this.phoneListMiddleVal = value;
    },
    /**
     * 确认选择
     */
    handleConfirmSelect(selectedPhones){
      this.queryParams.stockCompanies = selectedPhones.join();
    },
    /**
     * 确认搜索框
     */
    doSearch(e) {
      this.queryParams.content = e;
    },
    /**
     * 重置搜索项
     */
    resetSearch() {
      this.queryParams={
        industries: "",
        rptTypes: "",
        stockCompanies: "",
        reportDate: "",
        pageCount: "",
        queryType: "keypoint",
        content: "",
        from: 0,
        size: 10
      }
      this.cateCurrentIndex = 0;
      this.$refs.input.resetInput();
      page=0;
    },
    // 数据和分页是模拟的，实际也是这样写
    getNewsList() {
      let url = this.sortCheckedIndex?'rptSearchByTime':'rptSearch';
      // 假设已经到底，实际根据api接口返回值判断
      if (page >= this.total/10 && this.total>0) {
        uni.showToast({ title: "已经加载全部", icon: "none" });
        return;
      }
      uni.showLoading({});
      this.loadingStatus=true;
      let data = qs.stringify({...this.queryParams,from: page*10, size:10})
      uni.request({
        url:
          `https://api.qxsearch.net/api/search/${url}`,
        data: data,
        method: "POST", 
        header: {
          'content-type':'application/x-www-form-urlencoded',
        },
        success: res => {
          this.total = res.data.total;
          var newsList = res.data.hits;
          this.artList = this.artList.concat(newsList);
          uni.hideLoading();
          this.loadingStatus=false;
          page++;
        },
        complete: res => {
          uni.hideLoading();
          this.loadingStatus=false;
          uni.stopPullDownRefresh();
        }
      });
    },

    tabChangeBottom(cate) {
      if(cate.cateid===this.cateCurrentIndex){
          return false;
      }
      // 选中的索引
      this.cateCurrentIndex = cate.cateid;
      this.queryParams.queryType = cate.value;
      this.artList = [];
      page = 0;
    },
    /**
     * 图片放大
     */
    async magnifierImg(img_url) {
      this.bigImgUrl = img_url;
      this.$refs.modal.handleShow({
        maskClose: "true",
      });
    }
  }
};
</script>

<style lang="scss" scoped>
.news {
  .sort-block{
    position: absolute;
    right:0;
    top:0;
    text{
      padding: 10upx 16upx;
      border-radius: 4upx;
      &.active{
        background: $uni-color-primary;
        color: #fff;
      }
    }
  }
  .chartSelected{
    .title-text{
      display:-webkit-box;
      -webkit-box-orient:vertical;
      -webkit-line-clamp:1;
      overflow:hidden;
    }
    .title{
      font-size: 26upx;
    }
    .grace-news-list-title-desc{
      margin-top: 0;
    }
  }
  .desc-text{
    display:-webkit-box;
    -webkit-box-orient:vertical;
    -webkit-line-clamp:2;
    overflow:hidden;
    line-height:36rpx;
    max-height: 72upx;
    font-size: 24upx;
  }
  .title-text{
    // display:-webkit-box;
    // -webkit-box-orient:vertical;
    // -webkit-line-clamp:2;
    overflow:hidden;
    font-size: 28upx;
    max-height: 88upx;
  }
  .selection-content{
    padding-top: 200upx;
    text-align: center;
  }
  .empty{
    text-align: center;
    padding: 40upx;
    color:#9B9B9B;
  }
  &.no-scroll{
    height: 100vh;
    overflow: hidden;
    box-sizing: border-box;
  }
  .flex-items {
    display: flex;
    justify-content: center;
    align-items: center;
    border-top: 1upx solid #e3e3e3;
    .item {
      height: 82upx;
      flex: 1;
      position: relative;
      width: 100%;
      overflow: hidden;
      text-overflow:ellipsis;
      white-space: nowrap;
      font-size: 24upx;
      color: #9b9b9b;
      text-align: center;
      &:first-child {
        color: #4a4a4a;
      }
      &:nth-child(3) {
        color: $uni-color-primary;
      }
      &-text {
        line-height: 82upx;
      }
      &-img {
        width: 31upx;
        height: 31upx;
        display: inline-block;
        margin-right: 8upx;
        vertical-align: middle;
      }
    }
  }
  .text-center {
    text-align: center;
  }
  .cu-bar {
    margin-top: 0 !important;
  }
  .news {
    .boxa {
      padding-top: 0;
      .top_kbox {
        position: static;
      }
    }
  }
  .list_boxs {
    display: none;
  }
  .list_boxs2 {
    transform: none;
  }
 
}
</style>
<style lang="scss">
.search-box {
  display: flex;
  background-color: rgba(98, 183, 233, 1);
  padding: 10upx 40upx 8upx;
  .input-block {
    flex: 1;
  }
  .btn-reset {
    padding-left: 30upx;
    line-height: 95upx;
    font-size: 28upx;
    color: rgba(155, 155, 155, 1);
  }
}
.search-box .input-box {
  width: 85%;
  flex-shrink: 1;
  display: flex;
  justify-content: center;
  align-items: center;
}
.search-box .search-btn {
  width: 15%;
  margin: 0 0 0 2%;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-shrink: 0;
  font-size: 14px;
  color: #fff;
  background: linear-gradient(to right, #ff9801, #ff570a);
  border-radius: 30px;
}
.search-box .input-box > input {
  width: 100%;
  height: 30px;
  font-size: 16px;
  border: 0;
  border-radius: 30px;
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  padding: 0 3%;
  margin: 0;
  background-color: #ffffff;
}
.placeholder-class {
  color: #9e9e9e;
}
.search-keyword {
  width: 100%;
  background-color: rgb(242, 242, 242);
}
.keyword-list-box {
  height: calc(100vh - 55px);
  padding-top: 5px;
  border-radius: 10px 10px 0 0;
  background-color: #fff;
}
.keyword-entry-tap {
  background-color: #eee;
}
.keyword-entry {
  width: 94%;
  height: 40px;
  margin: 0 3%;
  font-size: 15px;
  color: #333;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: solid 1px #e7e7e7;
}
.keyword-entry image {
  width: 30px;
  height: 30px;
}
.keyword-entry .keyword-text,
.keyword-entry .keyword-img {
  height: 40px;
  display: flex;
  align-items: center;
}
.keyword-entry .keyword-text {
  width: 90%;
}
.keyword-entry .keyword-img {
  width: 10%;
  justify-content: center;
}
.keyword-box {
  height: calc(100vh - 55px);
  border-radius: 10px 10px 0 0;
  background-color: #fff;
}
.keyword-box .keyword-block {
  padding: 5px 0;
}
.keyword-box .keyword-block .keyword-list-header {
  width: 94%;
  padding: 5px 3%;
  font-size: 13.5px;
  color: #333;
  display: flex;
  justify-content: space-between;
}
.keyword-box .keyword-block .keyword-list-header image {
  width: 20px;
  height: 20px;
}
.keyword-box .keyword-block .keyword {
  width: 94%;
  padding: 3px 3%;
  display: flex;
  flex-flow: wrap;
  justify-content: flex-start;
}
.keyword-box .keyword-block .hide-hot-tis {
  display: flex;
  justify-content: center;
  font-size: 14px;
  color: #6b6b6b;
}
.keyword-box .keyword-block .keyword > view {
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 30px;
  padding: 0 10px;
  margin: 5px 10px 5px 0;
  height: 30px;
  font-size: 14px;
  background-color: rgb(242, 242, 242);
  color: #6b6b6b;
}
</style>
