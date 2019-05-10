<template>
  <cmd-page-body :backgroundColor="$uni-bg-color">
    <view class="user">
      <view class="avatar">
        <cmd-cell-item title="头像" slot-right arrow>
          <cmd-avatar :src="userInfo.avatarUrl"></cmd-avatar>
        </cmd-cell-item>
      </view>
      <navigator url="../user/updateName" open-type="navigate">
        <cmd-cell-item title="姓名" :addon="userInfo.nickName" arrow></cmd-cell-item>
      </navigator>
      <cmd-cell-item title="公司名称" addon="青彦科技" arrow/>
      <view style="margin-top:20upx">
        <cmd-cell-item title="邮箱" addon="HR@QINGYAN.COM" arrow/>
        <block>
          <navigator url="../user/validateTel" open-type="navigate" v-if="userInfo.tel">
            <cmd-cell-item title="手机" addon="13812345678" arrow/>
          </navigator>
          <cmd-cell-item title="绑定手机号" slot-right arrow v-else>
            <button
              open-type="getPhoneNumber"
              @getphonenumber="bindGetPhoneNumber"
              class="btn-tel"
            >绑定手机号</button>
          </cmd-cell-item>
        </block>
      </view>
    </view>
    <button
      type="primary"
      open-type="getUserInfo"
      lang="zh_CN"
      @getuserinfo="bindGetUserInfo"
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
      userInfo: {}
    };
  },
  onLoad() {
    const userInfo = uni.getStorageSync("userInfo");
    if (userInfo) {
      this.userInfo = userInfo;
    }
  },
  methods: {
    /**
     *微信授权登录
     */
    bindGetUserInfo(e) {
      this.userInfo = e.detail.userInfo;
      uni.setStorageSync("userInfo", e.detail.userInfo);
    }
  },
  /**
   * 获取手机号
   */
  bindGetPhoneNumber(e) {
    console.log(e);
  }
};
</script>

<style lang="scss">
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
