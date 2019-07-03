<template>
  <cmd-page-body :backgroundColor="$uni-bg-color">
    <view class="validate">
      <view>
        <view class="validate-phone">
          <view class="label">邮箱</view>
          <input type="text" focus v-model="email" placeholder="请输入邮箱">
          <view style="width:180upx;"></view>
        </view>
        <button
          class="btn-validate"
          :class="validateStatus ? 'btn-validate-active':''"
          hover-class="btn-validate-hover"
          @tap="updateEmail"
          style="margin-top: 20upx;"
        >确认</button>
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
      email: "",
      emailReg: /^([a-zA-Z0-9]+[_|\_|\.]?)*[a-zA-Z0-9]+@([a-zA-Z0-9]+[_|\_|\.]?)*[a-zA-Z0-9]+\.[a-zA-Z]{2,3}$/,
      validateStatus: false
    };
  },
  onLoad(options) {
    this.email = options.email ? options.email : "";
  },
  watch: {
    /**
     * 监听手机注册数值
     */
    email: {
      handler(newValue) {
        if (this.emailReg.test(newValue)) {
          this.validateStatus = true;
        } else {
          this.validateStatus = false;
        }
      },
      deep: true
    }
  },
  methods: {
    /**
     * 注册按钮点击执行
     */
    updateEmail() {
      //先跳转
      uni.request({
        url: "https://api.qxsearch.net/api/user/update",
        data: {
          token: uni.getStorageSync("token"),
          field: "email",
          value: this.email
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
          } else {
            uni.showToast({
              title: res.data.message,
              icon: "none"
            });
          }
        }
      });
    }
  },
  beforeDestroy() {}
};
</script>

<style lang="scss" scoped>
.validate {
  margin-top: 25upx;
  .tips {
    height: 37upx;
    color: rgba(155, 155, 155, 1);
    font-size: 26upx;
    font-family: PingFangSC-Regular;
    padding: 29upx 40upx 35upx;
  }
  input {
    height: 100upx;
    color: #b9b9b9;
    font-size: 28upx;
  }
  &-title {
    font-size: 56upx;
    font-weight: 500;
  }

  &-explain {
    font-size: 28upx;
    color: #9e9e9e;
  }

  &-phone {
    border-bottom: 2upx #dedede solid;
  }
  &-phone,
  &-code {
    padding: 0 46upx 0 34upx;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    background: #fff;
    .label {
      color: #4a4a4a;
      font-size: 34upx;
      width: 157upx;
    }
  }
  &-code {
    border-bottom: 2upx #dedede solid;
    &-getcode {
      width: 180upx;
      height: 64upx;
      line-height: 64upx;
      border-radius: 32upx;
      background-color: $uni-color-primary;
      color: #fff;
      text-align: center;
    }
  }

  &-username {
    margin-bottom: 40upx;
    border-bottom: 2upx #dfd3d3 solid;
  }

  &-password {
    border-bottom: 2upx #dedede solid;
  }

  .btn-validate {
    border-radius: 50upx;
    font-size: 36upx;
    margin: 0 40upx;
    color: #fff;
    background: #dedede;
    &-active {
      background: $uni-color-primary;
    }

    &-hover {
      background: $uni-color-primary;
    }
  }

  button[disabled] {
    color: #fff;
  }

  &-mode {
    text-align: center;
    margin-top: 32upx;
  }
}
</style>
