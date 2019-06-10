<template>
  <cmd-page-body>
    <view class="detail">
      <div class="header">
        <h3 class="title">{{detailInfo.parse_title || detailInfo.parse_chart_title}}</h3>
        <view class="flex-items">
          <view class="item">
            <text class="item-text">{{detailInfo.type}}</text>
          </view>
          <view class="item">
            <image src="../../static/icon_building.png" class="item-img"></image>
            <text class="item-text">{{detailInfo.parse_orgnization}}</text>
          </view>
          <view class="item">
            <image src="../../static/icon_page.png" class="item-img"></image>
            <text class="item-text">{{detailInfo.parse_pagecount}}P</text>
          </view>
          <view class="item">
            <text class="item-text">{{detailInfo.parse_reportdate}}</text>
          </view>
        </view>
      </div>
      <div class="content">
        <view class="resource">
          <view class="user">
            <image src="https://avatar.bbs.miui.com/images/noavatar_small.gif" class="icon"></image>
            <text class="item-text">{{detailInfo.parse_authors}}</text>
          </view>
          <button type="default" class="btn" @click="checkResource">查看原文</button>
        </view>
        <view class="desc">
          {{detailInfo.parse_keypoint}}
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
    return {
      detailInfo: {}
    };
  },
  onLoad(options) {
    this.detailInfo = JSON.parse(options.detail)
    

  },
  methods: {
    
    /**
     * 查看原文
     */
    checkResource(){
      uni.downloadFile({
        url: `https://api.qxsearch.net/api/res/pdf/${this.detailInfo.parse_pdf_filepath}`,
        success: function (res) {
          var filePath = res.tempFilePath;
          uni.openDocument({
            filePath: filePath,
            fileType: "pdf"
          });
        }
      });
    },
  }
};
</script>

<style lang="scss">
.detail {
  .header {
    .title {
      font-size: 32upx;
      padding: 33upx 40upx 34upx;
      text-align: center;
    }
    .flex-items {
      display: flex;
      align-items: center;
      justify-content: space-around;
      border-bottom: 1upx solid rgba(227, 227, 227, 1);
      padding: 0 40upx 12upx;
      .item {
        font-size: 24upx;
        color: #9b9b9b;
        overflow: hidden;
        text-overflow:ellipsis;
        white-space: nowrap;
        padding: 0 12upx;
        &:first-child {
          width: 20%;
          color: $uni-color-primary;
          height: 36upx;
          border-radius: 18upx;
          border: 1upx solid rgba(59, 143, 209, 1);
          text-align: center;
          line-height: 36upx;
        }
        &:nth-child(3) {
          width: 15%;
          text-align: center;
          color: $uni-color-primary;
        }
        &:last-child{
          width:164upx;
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
        text-align: center;
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
