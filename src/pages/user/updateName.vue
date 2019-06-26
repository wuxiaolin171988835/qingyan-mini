<template>
  <cmd-page-body>
    <view class="update-name">
      <view class="content">
        <input v-model="username">
        <button class="btn-save" @tap="save">保存</button>
      </view>
      <view class="tips">2到9个字符，只包含数字，字母，汉字</view>
    </view>
  </cmd-page-body>
</template>

<script>
import cmdPageBody from "@/components/cmd-page-body/cmd-page-body.vue";
export default {
  components: {
    cmdPageBody
  },
  data() {
    return {
      username: ""
    };
  },
  onLoad(options) {
    this.username = options.username;
  },
  methods: {
    /**
     * 保存用户名
     */
    save() {
      uni.request({
        url: "https://api.qxsearch.net/api/user/update",
        data: {
          token: uni.getStorageSync("token"),
          field: "name",
          value: this.username
        },
        method: "POST",
        header: {
          "content-type": "application/x-www-form-urlencoded"
        },
        success: res => {
          if (res.data.code == "OK" && res.data.status == "Success") {
            uni.showToast({
              title: res.data.message,
              icon: "none"
            });
            setTimeout(() => {
              uni.reLaunch({
                url: "/pages/user/user"
              });
            }, 800);
          } else if (
            res.data.code == "INVALID_TOKEN" &&
            res.data.status == "Failure"
          ) {
            //token过期
            uni.showToast({
              title: res.data.message,
              icon: "none"
            });
            setTimeout(() => {
              uni.navigateTo({
                url: "./bindTel"
              });
            }, 800);
          } else {
            uni.showToast({
              title: res.data.message,
              icon: "none"
            });
          }
        }
      });
    }
  }
};
</script>

<style lang="scss">
.update-name {
  padding: 0 30upx;
  .tips {
    height: 37rpx;
    color: #9b9b9b;
    font-size: 26rpx;
    font-family: PingFangSC-Regular;
  }
  .content {
    padding-top: 46upx;
    border-bottom: 1upx #3e4a72 solid;
    display: flex;
    input {
      color: #4a4a4a;
      padding-left: 10upx;
      font-size: 34upx;
    }
    .btn-save {
      width: 180upx;
      height: 64upx;
      text-align: center;
      line-height: 64upx;
      border-radius: 32px;
      background-color: $uni-color-primary;
      color: #fff;
      font-size: 28upx;
      margin-bottom: 10upx;
      margin-right: 0;
      &-active {
        background: $uni-color-primary;
      }

      &-hover {
        background: $uni-color-primary;
      }
    }
  }
}
</style>
