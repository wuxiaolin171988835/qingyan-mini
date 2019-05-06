<template>
  <cmd-page-body>
    <view class="detail">
      <div class="header">
        <h3 class="title">预收账款靓丽，管理效率提升</h3>
        <view class="flex-items">
          <view class="item">
            <text class="item-text">宏观证券</text>
          </view>
          <view class="item">
            <image src="../../static/icon_building.png" class="item-img"></image>
            <text class="item-text">东北证券</text>
          </view>
          <view class="item">
            <image src="../../static/icon_page.png" class="item-img"></image>
            <text class="item-text">25P</text>
          </view>
          <view class="item">
            <text class="item-text">2019-03-01</text>
          </view>
        </view>
      </div>
      <div class="content">
        <view class="resource">
          <view class="user">
            <image src="https://avatar.bbs.miui.com/images/noavatar_small.gif" class="icon"></image>
            <text class="item-text">王悦</text>
          </view>
          <button type="default" class="btn">查看原文</button>
        </view>
        <view class="desc">
          关注增长的预期差经济在一季度企稳，已经是既定事实。现在市场的关注点是，企稳能持续 多久、力度有多强，目前似乎有不少分歧。
          本报告围绕这些分歧展开，分成三部分，一是分析支撑一季度经济企稳的 动力是什么，二是宽信用政策对实体经济的拉动效果如何，三是判断未来的经 济形势。
          我们认为，今年三季度末、四季度初，对经济的乐观情绪，可能再度 面临修正的压力。
          \n
          一、一季度经济企稳，主要是地产投资在支撑，由于建安投资增速转正， 
          也带动了相关产品的生产。
          \n
          二、一季度社融放量，房地产行业其实不是最为受益的，真正对社融回升起决定性作用的，是从去年三季度开始小幅反弹的基建。基建投资有 季节性规律，一些基建项目，比如道路工程，摊铺沥青对温度有很高 要求，冬季不适合施工。这可能是制约基建投资反弹力度的因素之一。 随着温度升高，二季度基建投资增速可能加快上升，宽信用对实体经 济的拉动体现出来。
        </view>
      </div>
    </view>
  </cmd-page-body>
</template>

<script>
import cmdPageBody from "@/components/cmd-page-body/cmd-page-body.vue";
export default {
  components: { cmdPageBody },
  data() {
    return {};
  },
  onLoad() {},
  methods: {}
};
</script>

<style lang="scss">
.detail {
  .header {
    .title {
      font-size: 32upx;
      padding: 33upx 0 34upx;
      text-align: center;
    }
    .flex-items {
      display: flex;
      justify-content: center;
      align-items: center;
      border-bottom: 1upx solid rgba(227, 227, 227, 1);
      justify-content: space-around;
      padding: 0 0 12upx;
      .item {
        font-size: 24upx;
        color: #9b9b9b;
        &:first-child {
          color: $uni-color-primary;
          width: 124upx;
          height: 36upx;
          border-radius: 18upx;
          border: 1upx solid rgba(59, 143, 209, 1);
          text-align: center;
          line-height: 36upx;
        }
        &:nth-child(3) {
          color: $uni-color-primary;
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
  }
  .content {
    padding: 0 40upx 86upx;
    .resource {
      padding: 20upx 0 11upx;
      height: 64upx;
      .user {
        line-height: 64upx;
        float: left;
        color: #4a4a4a;
        font-size: 24upx;
        .icon {
          width: 32upx;
          height: 32upx;
          border-radius: 100%;
          vertical-align: middle;
          margin-right: 12upx; 
          margin-top: -5upx;
        }
      }
      .btn {
        float: right;
        width: 180upx;
        height: 64upx;
        line-height: 64upx;
        border-radius: 32px;
        font-size: 26upx;
        background-color: $uni-color-primary;
        color: #fff;
      }
    }
    .desc {
      line-height: 48upx;
      color: #4a4a4a;
      font-size: 24upx;
    }
  }
}
</style>
