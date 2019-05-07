<template>
  <cmd-page-body :backgroundColor="$uni-bg-color">
    <view class="user">
      <view class="avatar">
        <cmd-cell-item title="头像" slot-right arrow>
          <cmd-avatar src="https://avatar.bbs.miui.com/images/noavatar_small.gif"></cmd-avatar>
        </cmd-cell-item>
      </view>
      <navigator url="../user/updateName" open-type="navigate">
        <cmd-cell-item title="姓名" addon="某某" arrow></cmd-cell-item>
      </navigator>
      <cmd-cell-item title="公司名称" addon="青彦科技" arrow/>
      <view style="margin-top:20upx">
        <cmd-cell-item title="邮箱" addon="HR@QINGYAN.COM" arrow/>
        <navigator url="../user/validateTel" open-type="navigate">
          <cmd-cell-item title="手机" addon="13812345678" arrow/>
        </navigator>
      </view>
    </view>
  </cmd-page-body>
</template>

<script>
import cmdPageBody from "@/components/cmd-page-body/cmd-page-body.vue";
import cmdCellItem from "@/components/cmd-cell-item/cmd-cell-item.vue";
import cmdAvatar from "@/components/cmd-avatar/cmd-avatar.vue";
export default {
  components: { cmdPageBody, cmdCellItem, cmdAvatar },
  data() {
    return {};
  },
  onLoad() {},
  methods: {}
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
