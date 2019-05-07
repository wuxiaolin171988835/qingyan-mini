<template>
  <cmd-page-body :backgroundColor="$uni-bg-color">
    <view class="update">
      <view v-if="status">
        <view class="update-phone">
          <view class="label">新手机号</view>
          <input type="number" focus maxlength="11" placeholder="请输入手机号">
          <view style="width:180upx;"></view>
        </view>
        <view class="update-code">
          <view class="label">验证码</view>
          <input v-model="mobile.code" type="number" maxlength="6" placeholder="请输入验证码">
          <view
            class="update-code-getcode"
            @tap="!safety.state ? fnGetPhoneCode() : ''"
          >{{!safety.state&&'获取验证码'||(safety.time+' s')}}</view>
        </view>
        <view class="tips">
          <text>重新绑定后，原手机号码将不能用于登录</text>
        </view>
        <button
          class="btn-update"
          :class="registerMobile ? 'btn-update-active':''"
          hover-class="btn-update-hover"
          @tap="fnRegister"
        >确定</button>
      </view>
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
      account: {
        username: "",
        password: ""
      },
      usernameReg: /^[A-Za-z0-9]+$/,
      passwordReg: /^\w+$/,
      registerAccount: false,
      mobile: {
        phone: "",
        code: ""
      },
      phoneReg: /^[1](([3][0-9])|([4][5-9])|([5][0-3,5-9])|([6][5,6])|([7][0-8])|([8][0-9])|([9][1,8,9]))[0-9]{8}$/,
      registerMobile: false,
      safety: {
        time: 60,
        state: false,
        interval: ""
      },
      status: true // true手机注册,false账号注册
    };
  },
  watch: {
    /**
     * 监听手机注册数值
     */
    mobile: {
      handler(newValue) {
        if (this.phoneReg.test(newValue.phone) && newValue.code.length === 6) {
          this.registerMobile = true;
        } else {
          this.registerMobile = false;
        }
      },
      deep: true
    },
    /**
     * 监听账号注册数值
     */
    account: {
      handler(newValue) {
        if (
          this.usernameReg.test(newValue.username) &&
          newValue.username.length >= 8 &&
          (this.passwordReg.test(newValue.password) &&
            newValue.password.length >= 8)
        ) {
          this.registerAccount = true;
        } else {
          this.registerAccount = false;
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
      uni.showToast({ title: "手机号修改成功", icon: "success" });
      setTimeout(() => {
        uni.reLaunch({
          url: "/pages/user/user"
        });
      }, 500);
      if (this.status) {
        console.log(JSON.stringify(this.mobile));
      } else {
        console.log(JSON.stringify(this.account));
      }
    },
    /**
     * 获取验证码
     */
    fnGetPhoneCode() {
      if (this.phoneReg.test(this.mobile.phone)) {
        uni.showToast({
          title: "正在发送验证码",
          icon: "loading",
          success: () => {
            // 成功后显示倒计时60s后可在点击
            this.safety.state = true;
            // 倒计时
            this.safety.interval = setInterval(() => {
              if (this.safety.time-- <= 0) {
                this.safety.time = 60;
                this.safety.state = false;
                clearInterval(this.safety.interval);
              }
            }, 1000);
            uni.showToast({
              title: "发送成功",
              icon: "success"
            });
          }
        });
      } else {
        uni.showToast({
          title: "手机号不正确",
          icon: "none"
        });
      }
    }
  },
  beforeDestroy() {
    /**
     * 关闭页面清除轮询器
     */
    clearInterval(this.safety.interval);
  }
};
</script>

<style lang="scss">
.update {
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
    background: #fff;
    padding: 0 46upx 0 34upx;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
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
    border-bottom: 2upx #dedede solid;
  }

  &-password {
    border-bottom: 2upx #dedede solid;
  }

  .btn-update {
    border-radius: 50upx;
    font-size: 36upx;
    margin: 0 40upx;
    color: #fff;
    background: $uni-color-primary;

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
