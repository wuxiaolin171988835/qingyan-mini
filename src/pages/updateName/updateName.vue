<template>
  <cmd-page-body>
    <view class="update-name">
      <view class="content">
        <input v-model="username">
        <button class="btn-save" @tap="save">保存</button>
      </view>
    </view>
  </cmd-page-body>
</template>

<script>
import cmdPageBody from "@/components/cmd-page-body/cmd-page-body.vue";
import { setTimeout } from "timers";
export default {
  components: {
    cmdPageBody
  },
  data() {
    return {
      username: "某某"
    };
  },
  onLoad() {},
  methods: {
    /**
     * 保存用户名
     */
    save() {
      uni.showToast({ title: "姓名修改成功", icon: "success" });
      setTimeout(() => {
        uni.reLaunch({
          url: "/pages/user/user"
        });
      }, 500);
    }
  }
};
</script>

<style lang="scss">
.update-name {
  padding: 0 30upx;
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
