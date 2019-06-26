<template>
  <cmd-page-body :backgroundColor="$uni-bg-color">
    <view class="user" v-if="isAuth">
      <view class="avatar">
        <cmd-cell-item title="头像" slot-right arrow>
          <cmd-avatar :src="userInfo.photoContent"></cmd-avatar>
        </cmd-cell-item>
      </view>
      <view @tap="updateName">
        <cmd-cell-item title="姓名" :addon="userInfo.name" arrow></cmd-cell-item>
      </view>
      <view style="margin-top:20upx">
        <cmd-cell-item title="邮箱" :addon="userInfo.email" arrow/>
        <view @tap="updateTel">
          <cmd-cell-item title="手机" :addon="userInfo.mobile" arrow/>
        </view>
      </view>
    </view>
    <button
      type="primary"
      open-type="getUserInfo"
      lang="zh_CN"
      v-else
      @getuserinfo="bindGetUserInfo"
      class="btn-auth"
    >微信授权登录</button>
  </cmd-page-body>
</template>

<script>
import cmdPageBody from "@/components/cmd-page-body/cmd-page-body.vue";
import cmdCellItem from "@/components/cmd-cell-item/cmd-cell-item.vue";
import cmdAvatar from "@/components/cmd-avatar/cmd-avatar.vue";
export default {
  components: { cmdPageBody, cmdCellItem, cmdAvatar },
  data() {
    return {
      userInfo: {
        name: "",
        photoContent: "",
        mobile: "",
        email: ""
      },
      isAuth: false
    };
  },
  onLoad() {
    // 查看是否授权
    uni.getSetting({
      success: res => {
        if (res.authSetting["scope.userInfo"]) {
          this.isAuth = true;
          const userInfo = uni.getStorageSync("userInfo");
          if (userInfo) {
            this.userInfo = userInfo;
            let token = uni.getStorageSync("token");
            let mobile = uni.getStorageSync("mobile");
            this.userInfo.mobile = mobile;
            if (token) {
              this.getUserInfo(token, mobile);
            }
          }
        } else {
          this.isAuth = false;
        }
      }
    });
  },
  methods: {
    /**
     * 获取用户信息
     */
    getUserInfo(token, mobile) {
      uni.request({
        url: "https://api.qxsearch.net/api/user/userInfo",
        data: {
          token: token,
          mobile: mobile
        },
        method: "POST",
        header: {
          "content-type": "application/x-www-form-urlencoded"
        },
        success: res => {
          if (res.data.code == "OK" && res.data.status == "Success") {
            this.userInfo = res.data.data[0];
            this.userInfo.mobile = mobile;
          } else {
            //token过期
            uni.showToast({
              title: res.data.message,
              icon: "none"
            });
            uni.showToast({
              title: "请重新登录",
              icon: "none"
            });
            setTimeout(() => {
              uni.navigateTo({
                url: "../user/bindTel" //跳转页面的路径，可带参数 ？隔开，不同参数用 & 分隔；相对路径，不需要.wxml后缀
              });
            }, 800);
          }
        }
      });
    },
    /**
     *微信授权登录
     */
    bindGetUserInfo(e) {
      this.userInfo.name = e.detail.userInfo.nickName;
      this.userInfo.photoContent = e.detail.userInfo.avatarUrl;
      uni.setStorageSync("userInfo", this.userInfo);
      uni.getSetting({
        success: res => {
          if (res.authSetting["scope.userInfo"]) {
            this.isAuth = true;
          } else {
            this.isAuth = false;
          }
        }
      });
    },
    checkIsLogin() {
      return uni.getStorageSync("token");
    },
    /**
     * 修改姓名
     */
    updateName() {
      if (!this.checkIsLogin()) {
        uni.showToast({
          title: "请绑定手机号",
          icon: "none"
        });
        setTimeout(() => {
          uni.navigateTo({
            url: "../user/bindTel" //跳转页面的路径，可带参数 ？隔开，不同参数用 & 分隔；相对路径，不需要.wxml后缀
          });
        }, 800);
      } else {
        uni.navigateTo({
          url: `../user/updateName?username=${this.userInfo.name}`
        });
      }
    },
    /**
     * 修改手机号
     */
    updateTel() {
      if (!this.checkIsLogin()) {
        uni.showToast({
          title: "请绑定手机号",
          icon: "none"
        });
        setTimeout(() => {
          uni.navigateTo({
            url: "../user/bindTel" //跳转页面的路径，可带参数 ？隔开，不同参数用 & 分隔；相对路径，不需要.wxml后缀
          });
        }, 500);
      } else {
        uni.navigateTo({
          url: `../user/validateTel?mobile=${this.userInfo.mobile}`
        });
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.btn-auth {
  background: $uni-color-primary;
  margin-top: 100upx;
  width: 50%;
}
.user {
  .navigator-hover {
    background-color: initial;
  }
  .avatar {
    .cmd-cell-item-body {
      height: 159upx;
      line-height: 159upx;
    }
    .cmd-avatar--md {
      width: 100upx;
      height: 100upx;
    }
  }
  .btn-tel {
    position: absolute;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
    // opacity: 0;
  }
  .cmd-cell-item {
    background: #fff;
    border-color: #d8d8d8;
    &-body {
      margin-left: 0;
      margin-right: 0;
      padding: 0 36upx 0 40upx;
      height: 88upx;
      line-height: 88upx;
    }
    &-title {
      color: #4a4a4a;
      font-size: 34upx;
    }
    &-right {
      text {
        color: #b2b2b2;
        font-size: 32upx;
      }
    }
  }
}
</style>
