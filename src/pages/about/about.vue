<template>
  <cmd-page-body>
    <view class="about">
      <view>
        <view class="fixedit" :style="{top:top}">
          <scroll-view class="grace-tab-title grace-center" scroll-x="true" id="grace-tab-title">
            <view class=grace-view-tab>
              <view
                v-for="(cate, index) in categories"
                :key="index"
                :data-cateid="cate.cateid"
                :data-index="index"
                :class="[cateCurrentIndex == index ? 'grace-tab-current' : '']"
                @tap="tabChange"
              >
                <text class="grace-tab-text">
                {{cate.name}}
                </text>
              </view>
            </view>
          </scroll-view>
        </view>
        <!-- 内容区 -->
        <view class="grace-news-list">
          <view v-if="!cateCurrentIndex" class="position-container">
            <view class="title-hot">
              <text>热招职位</text>
            </view>
            <!-- <navigator url="../about/position" open-type="navigate"> -->
              <cmd-cell-item
                v-for="position in positionList"
                :key="position.id"
                :title="position.name"
                :addon="position.release_time"
                arrowDefined
                @click="goDetail(position.id)"
              ></cmd-cell-item>
            <!-- </navigator> -->
          </view>
          <view v-else class="produce-container">
            <view class="title">
              <text>公司简介</text>
            </view>
            <text class="desc">
              北京青彦科技有限公司投身于金融科技（Fintech）领域，矢志于为金融投资研究提供智慧分析的服务与工具。公司口号为“智慧投研”，通过大数据分析、人工智能、知识图谱等IT技术赋能金融投资，让投资人员与分析师能够更高效、更便捷的获取价值数据及深度分析报告，从而领先市场一步。
            </text>
            \n 
            <text class="desc" style="margin-top: 50upx;">
              公司坐落于北京市朝阳区三元桥曙光大厦，毗邻三元桥地铁站。公司是新型创业型公司，已获得种子轮融资，创始人为在金融投资领域浸淫多年的投资专家，对金融投资行业的趋势变化有着深刻的行业理解。
            </text>
            \n 
            <text class="desc" style="margin-top: 50upx;">
              业务联系请联系<text style="color:#3B8FD1;">contact@qxsearch.net</text>
            </text>
            <view class="title">
              <text>创始人简介</text>
            </view>
            <span>龙红亮，CFA，金融系博士：</span>
              \n
            <text class="desc"  style="margin-top: 50upx;">
              商业银行多年的FICC投资管理经验，管理规模超过500亿元；亦曾服务于世界级的金融资讯集团汤森路透。积累了丰富的金融投资行业经验，对Fintech领域有着深刻的理解。出版个人专著《债券投资实战》。
            </text>
            <view class="logo">
              <image src="../../static/logo.png"></image>
            </view>
          </view>
        </view>
      </view>
    </view>
  </cmd-page-body>
</template>

<script>
import cmdPageBody from "@/components/cmd-page-body/cmd-page-body.vue";
import wucTab from "@/components/wuc-tab/wuc-tab.vue";
import cmdCellItem from "@/components/cmd-cell-item/cmd-cell-item.vue";
let page = 1,
  cate = 0;
export default {
  components: {
    cmdPageBody,
    wucTab,
    cmdCellItem
  },
  data() {
    return {
      //tab
      categories: [
        { cateid: 0, name: "加入我们" },
        { cateid: 1, name: "关于我们" }
      ],
      // 当前选中tab
      cateCurrentIndex: 0,
      positionList:[
        {
          id: 1,
          name: '产品总监',
          release_time: '2019-06-13'
        },
        {
          id: 2,
          name: '技术总监',
          release_time: '2019-06-13'
        },
        {
          id: 3,
          name: '金融实习生',
          release_time: '2019-06-13'
        },
        {
          id: 4,
          name: '技术实习生',
          release_time: '2019-06-13'
        }
      ],//职位列表
    };
  },
  onLoad(options) {
  },
  //下拉刷新
  onPullDownRefresh: function() {
    
  },
  // 加载更多
  onReachBottom: function() {
  },
  methods: {
    tabChange: function(e) {
      // 选中的索引
      var index = e.currentTarget.dataset.index;
      // 具体的分类id
      var cateid = e.currentTarget.dataset.cateid;
      this.cateCurrentIndex = index;
      // 动态替换内容
      this.content = this.categories[index].name;
      // 读取分类数据
      cate = cateid; //把分类信息发送给api接口即可读取对应分类的数据
    },
  goDetail(id){
    uni.navigateTo({
      url: './position?position_id='+id,
    })
  }
  }
};
</script>

<style lang="scss">
.about {
  padding-bottom: 75upx;
  background: #fff;
  //加入我们
  .navigator-hover {
    background-color: initial;
    opacity: initial;
  }
  .grace-tab-title {
    height: 80upx;
    line-height: 80upx;
    background: rgba(246, 246, 246, 1);
    margin-top: 0;
    view {
      height: 80upx;
      line-height: 80upx;
      box-sizing: border-box;
      border-bottom: none;
      font-size: 32upx;
    }
    .grace-tab-current text{
      line-height: 70upx;
    }
  }
  .grace-news-list {
    &-items {
      background: initial;
    }
    .title-hot {
      text-align: center;
      font-size: 34upx;
      color: #4a4a4a;
      padding: 77upx 0 30upx;
      position: relative;
      text {
        border-bottom: 2upx solid rgba(74, 74, 74, 1);
        padding-bottom: 20upx;
      }
    }
    & > navigator {
      &:first-child {
        margin-top: 0;
      }
    }
    .cmd-cell-item {
      &-hover {
        background: initial;
      }
      &-body {
        padding: 22upx 0;
        margin: 0 47upx 0 40upx;
      }
      &-title {
        color: #38466E;
        font-size: 32upx;
        line-height: 45upx;
      }
      &-brief {
        margin-top: 20upx;
        color: #4a4a4a;
        font-size: 24upx;
      }
      &-right {
        display: block;
        text {
          color: #4a4a4a;
          font-size: 24upx;
        }
        .cmd-cell-icon-arrow-right {
          text-align: right;
          .cmd-icon {
            display: inline-block;
            width: 32upx;
            height: 32upx;
            background: $uni-color-primary;
            color: #fff;
            font-size: 24upx;
            border-radius: 50%;
          }
        }
      }
    }
  }
  //关于我们
  .produce-container {
    padding: 0 40upx;
    .title {
      padding: 77upx 0 29upx;
      text-align: center;
      color: #4a4a4a;
      text {
        color: 34upx;
        border-bottom: 2upx solid #4a4a4a;
        padding-bottom: 20upx;
      }
    }
    .desc {
      font-size: 24upx;
      line-height: 40upx;
      text-indent: 80upx;
      display: block;
    }
    .logo {
      margin: 78upx auto 0;
      text-align: center;
      image{
        width:252upx;
        height:58upx;
      }
    }
  }
}
</style>
