<template>
  <cmd-page-body :backgroundColor="$uni-bg-color">
    <view class="validate">
      <view v-if="status">
        <view class="validate-phone">
          <view class="label">手机号</view>
          <input
            type="number"
            focus
            maxlength="11"
            :value="phone.mobile"
            disabled
            placeholder="请输入手机号"
          >
          <view style="width:180upx;"></view>
        </view>
        <view class="validate-code">
          <view class="label">验证码</view>
          <input v-model="phone.verityCode" type="number" maxlength="4" placeholder="请输入验证码">
          <view class="validate-code-getcode" @tap="fnGetPhoneCode()">获取验证码</view>
        </view>
        <button
          class="btn-validate"
          :class="registerMobile ? 'btn-validate-active':''"
          hover-class="btn-validate-hover"
          @tap="fnRegister"
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
      phone: {
        mobile: "",
        verityCode: ""
      },
      phoneReg: /^[1](([3][0-9])|([4][5-9])|([5][0-3,5-9])|([6][5,6])|([7][0-8])|([8][0-9])|([9][1,8,9]))[0-9]{8}$/,
      registerMobile: false,
      status: true // true手机注册,false账号注册
    };
  },
  onLoad(options) {
    this.phone.mobile = options.mobile;
  },
  watch: {
    /**
     * 监听手机注册数值
     */
    phone: {
      handler(newValue) {
        if (
          this.phoneReg.test(newValue.mobile) &&
          newValue.verityCode.length === 4
        ) {
          this.registerMobile = true;
        } else {
          this.registerMobile = false;
        }
      },
      deep: true
    }
  },
  methods: {
    /**
     * 注册按钮点击执行
     */
    fnRegister() {
      //先跳转
      uni.request({
        url: "https://apitest.qxsearch.net/api/user/verityMobile",
        data: this.phone,
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
              uni.navigateTo({
                url: "./updateTel"
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
    },
    /**
     * 获取验证码
     */
    fnGetPhoneCode() {
      if (this.phoneReg.test(this.phone.mobile)) {
        uni.request({
          url: "https://apitest.qxsearch.net/api/user/verity",
          data: {
            mobile: this.phone.mobile
          },
          method: "POST",
          header: {
            "content-type": "application/x-www-form-urlencoded"
          },
          success: res => {
            uni.showToast({
              title: res.data.message,
              icon: "none"
            });
          }
        });
      } else {
        uni.showToast({
          title: "手机号格式不正确",
          icon: "none"
        });
      }
    }
  },
  beforeDestroy() {}
};
</script>

<style lang="scss">
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
