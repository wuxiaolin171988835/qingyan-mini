<template>
  <cmd-page-body :backgroundColor="$uni-bg-color">
    <view class="news">
        <!-- 顶级tab -->
        <wuc-tab
          :tab-list="tabList"
          :tabCur.sync="TabCur"
          tab-class="text-center"
          select-class="tab-selected"
          @change="tabChangeTop"
        ></wuc-tab>
        <!-- tab内容区 -->
        <div class="cu-bar bg-white solid-bottom" style="margin-top:100upx">
          <div class="content">
            <block>
              <view class="search-box">
                <mSearch
                  placeholder="输入你想找的关键字"
                  :mode="2"
                  button="inside"
                  @search="doSearch($event)"
                  class="input-block"
                ></mSearch>
                <text @click="resetSearch" class="btn-reset">重置</text>
              </view>
              <ChooseLits :list="list" :arr="arr" @chooseLike="chooseLike"></ChooseLits>
              <!-- tab项 -->
              <view>
                <view class="fixedit" :style="{top:top}">
                  <scroll-view
                    class="grace-tab-title grace-center"
                    scroll-x="true"
                    id="grace-tab-title"
                  >
                    <view
                      v-for="(cate, index) in categories"
                      :key="index"
                      :data-cateid="cate.cateid"
                      :data-index="index"
                      :class="[cateCurrentIndex == index ? 'grace-tab-current' : '']"
                      @tap="tabChangeBottom"
                    >{{cate.name}}</view>
                  </scroll-view>
                </view>
                <!-- 内容区 -->
                <view class="grace-news-list">
                  <block v-for="(item, index) in artList" :key="index">
                    <navigator url="./detail" open-type="navigate" class="grace-news-list-items">
                      <view class="grace-news-list-img-news">
                        <view class="grace-news-list-img-big">
                          <image src="../../static/demo.jpg" mode="widthFix" class="img"></image>
                          <image
                            src="../../static/icon_magnifier.png"
                            class="icon_magnifier"
                            @click.stop="magnifierImg"
                          ></image>
                        </view>
                        <view class="grace-news-list-info">
                          <text class="grace-news-list-title-main">{{item.title}}</text>
                          <text class="grace-news-list-title-desc">{{item.desc}}</text>
                        </view>
                      </view>
                      <view class="flex-items">
                        <view class="item">
                          <image src="../../static/icon_building.png" class="item-img"></image>
                          <text class="item-text">东北证券</text>
                        </view>
                        <view class="item">
                          <image src="../../static/icon_person.png" class="item-img"></image>
                          <text class="item-text">王悦</text>
                        </view>
                        <view class="item">
                          <image src="../../static/icon_page.png" class="item-img"></image>
                          <text class="item-text">25P</text>
                        </view>
                        <view class="item">
                          <text class="item-text">2019-03-01</text>
                        </view>
                      </view>
                    </navigator>
                  </block>
                </view>
              </view>
            </block>
            
          </div>
        </div>
      <!-- 图片放大弹窗 -->
      <min-modal ref="modal">
        <img
          src="../../static/demo.jpg"
          alt
          srcset="../../static/demo.jpg"
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

let page = 1,
  cate = 0;
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
      tabList: [{ name: "研报查询" }, { name: "研报精选" }], //顶级tab
      TabCur: 0, //一级tab选中项
      list: ["行业", "类别", "机构", "日期", "页数"], //select选项卡
      arr: [
        ["全部行业", "石油石化", "石油石化", "石油石化", "石油石化"],
        ["全部类别", "晨会", "周报", "东北证券", "东北证券"],
        ["全部机构", "5k以下", "5k-10k", "10k以上"],
        ["全部日期", "1个月内", "3个月内", "6个月内", "1年以内"],
        ["全部页数", "1-5页", "6-20页", "20页以上"]
      ], //select选项值
      keyword: "", //搜索关键字
      top: 0,
      
      categories: [
        { cateid: 0, name: "标题" },
        { cateid: 1, name: "要点" },
        { cateid: 2, name: "图标" },
        { cateid: 3, name: "正文" }
      ],//二级tab
      cateCurrentIndex: 0,//二级tab选中项
      // mock数据
      artList: [
        {
          id: 0,
          title: "预收账款靓丽，管理效率提升",
          desc: "事件：公司发布年报，18年公司实现营收19.58亿元，同比...."
        },
        {
          id: 1,
          title: "预收账款靓丽，管理效率提升",
          desc: "事件：公司发布年报，18年公司实现营收19.58亿元，同比...."
        },
        {
          id: 2,
          title: "预收账款靓丽，管理效率提升",
          desc: "事件：公司发布年报，18年公司实现营收19.58亿元，同比...."
        },
        {
          id: 3,
          title: "预收账款靓丽，管理效率提升",
          desc: "事件：公司发布年报，18年公司实现营收19.58亿元，同比...."
        },
        {
          id: 4,
          title: "预收账款靓丽，管理效率提升",
          desc: "事件：公司发布年报，18年公司实现营收19.58亿元，同比...."
        }
      ]
    };
  },
  onLoad() {
    this.top = "44px";
    page = 1;
    // artList: [];
    this.getNewsList();
  },
  //下拉刷新
  onPullDownRefresh: function() {
    // 重置分页及数据
    page = 1;
    this.artList = [];
    this.getNewsList();
  },
  // 加载更多
  onReachBottom: function() {
    this.getNewsList();
  },
  methods: {
    /**
     * tab切换
     */
    tabChangeTop(index) {
      this.TabCur = index;
      if(index===1){
        this.cateCurrentIndex=0;
      }
    },
    /**
     * select选择
     */
    chooseLike(key) {
      console.log(key[0], key[1]);
    },
    /**
     * 确认搜索框
     */
    doSearch(e) {
      this.keyword = e;
    },
    /**
     * 重置搜索项
     */
    resetSearch() {
      this.keyword = "";
    },
    // 数据和分页是模拟的，实际也是这样写
    getNewsList: function() {
      // uni.showLoading({});
      // // 假设已经到底，实际根据api接口返回值判断
      // if (page >= 3) {
      //   uni.showToast({ title: "已经加载全部", icon: "none" });
      //   return;
      // }
      // uni.request({
      //   url:
      //     "https://www.easy-mock.com/mock/5cb9655c01e2e57715d324b0/example/imgnewlist?page=" +
      //     page +
      //     "#!method=get&cate=" +
      //     cate,
      //   method: "GET",
      //   data: {},
      //   success: res => {
      //     console.log(res);
      //     var newsList = res.data.data;
      //     this.artList = this.artList.concat(newsList);
      //     uni.hideLoading();
      //     page++;
      //   },
      //   complete: res => {
      //     uni.hideLoading();
      //     uni.stopPullDownRefresh();
      //   }
      // });
    },

    tabChangeBottom: function(e) {
      // 选中的索引
      var index = e.currentTarget.dataset.index;
      // 具体的分类id
      var cateid = e.currentTarget.dataset.cateid;
      this.cateCurrentIndex = index;
      // 动态替换内容
      this.content = this.categories[index].name;

      // 读取分类数据
      cate = cateid; //把分类信息发送给api接口即可读取对应分类的数据
      // 重置分页及数据
      page = 1;
      // this.artList = [];
      // 加载对应分类数据覆盖上一个分类的展示数据 加载更多是继续使用这个分类
      // this.getNewsList();
    },
    /**
     * 图片放大
     */
    magnifierImg(e) {
      this.$refs.modal.handleShow({
        title: "",
        maskClose: "true",
        success: res => {
          console.log(res);
        }
      });
    }
  }
};
</script>

<style lang="scss">
.news {
  .flex-items {
    display: flex;
    justify-content: center;
    align-items: center;
    border-top: 1upx solid #e3e3e3;
    .item {
      height: 82upx;
      flex: 1;
      position: relative;
      padding-left: 36upx;
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
  .tab-selected {
    color: $uni-color-primary;
    border-bottom: 1px solid $uni-color-primary;
  }
}
</style>
<style lang="scss">
.search {
  background: none !important;
  border-bottom: none !important;
  .content {
    height: 70upx !important;
  }
}
.search .content .content-box .input.center {
  width: 100% !important;
  height: 70upx !important;
  line-height: 70upx !important;
  transition: none !important;
}
.search-box {
  display: flex;
  background-color: rgba(98, 183, 233, 1);
  padding: 23upx 40upx 20upx;
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
