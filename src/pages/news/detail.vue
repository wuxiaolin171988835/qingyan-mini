<template>
  <cmd-page-body>
    <view class="detail" v-if="detailInfo">
      <div class="header">
        <h3 class="title">
          <view v-if="detailInfo.parse_chart_title">{{detailInfo.parse_chart_title}}</view>
          <view style="font-size: 26upx;">{{detailInfo.parse_title}}</view>
        </h3>
        <view class="flex-items">
          <view class="item" v-if="detailInfo.type_short_name || detailInfo.industry_short_name">
            <text class="item-text">{{detailInfo.type_short_name}}</text>
            <text class="item-text" v-if="detailInfo.industry_short_name">{{detailInfo.industry_short_name}}</text>
            <text class="item-text" v-if="detailInfo.company">{{detailInfo.company}}</text>
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
        <rich-text class="desc" :nodes="detailInfo.abstractRichText">
        </rich-text>
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
      detailInfo: {},
      queryType:''
    };
  },
  onLoad(options) {
    this.queryType=options.queryType;
    uni.request({
      url:
        "https://apitest.qxsearch.net/api/search/rptSearchId",
      data: {id:options.id,queryType:options.queryType},
      method: "POST", 
      header: {
        'content-type':'application/x-www-form-urlencoded',
      },
      success: res => {
        this.detailInfo=res.data.hits[0]
      }
    })
    

  },
  methods: {
    
    /**
     * 查看原文
     */
    checkResource(){
      uni.showLoading();
      uni.downloadFile({
        url: `https://apitest.qxsearch.net/api/res/pdf/${this.detailInfo.parse_pdf_filepath}`,
        success: function (res) {
          var filePath = res.tempFilePath;
          uni.openDocument({
            filePath: filePath,
            fileType: "pdf",
            success: function(res) {
              uni.hideLoading();
            }
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
      view{
        overflow: hidden;
        text-overflow:ellipsis;
        white-space: nowrap;
      }
    }
    .flex-items {
      display: flex;
      align-items: center;
      justify-content: space-around;
      border-bottom: 1upx solid rgba(227, 227, 227, 1);
      padding: 0 20upx 12upx;
      .item {
        font-size: 24upx;
        color: #9b9b9b;
        overflow: hidden;
        text-overflow:ellipsis;
        white-space: nowrap;
        &:first-child {
          color: $uni-color-primary;
          .item-text{
            border-radius: 18upx;
            border: 1upx solid rgba(59, 143, 209, 1);
            display: inline-block;
            text-align: center;
            padding: 0 10upx;
            box-sizing: border-box;
            max-width: 120upx;
            height: 34upx;
            line-height: 32upx;
            &:first-child{
              margin-right: 6upx;
            }
          }
        }
        &:nth-child(3) {
          text-align: center;
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
        text-align: center;
      }
    }
    .desc {
      line-height: 48upx;
      color: #4a4a4a;
      font-size: 24upx;
      text-indent: 2em;
    }
  }
}
</style>
