<template>
  <view class="validate">
    <cmd-transition name="fade-up">
      <view v-if="status">
        <view class="validate-phone">
          <cmd-input value="13812345678" type="number" focus maxlength="11" disabled></cmd-input>
          <view
            class="validate-phone-getcode"
            @tap="!safety.state ? fnGetPhoneCode() : ''"
          >{{!safety.state&&'获取验证码'||(safety.time+' s')}}</view>
        </view>
        <view class="validate-code">
          <cmd-input v-model="mobile.code" type="number" maxlength="6" placeholder="请输入短信验证码"></cmd-input>
        </view>
        <button
          class="btn-validate"
          :class="registerMobile ? 'btn-validate-active':''"
          hover-class="btn-validate-hover"
          @tap="fnRegister"
        >下一步</button>
      </view>
    </cmd-transition>
  </view>
</template>

<script>
import cmdTransition from "@/components/cmd-transition/cmd-transition.vue";
import cmdInput from "@/components/cmd-input/cmd-input.vue";

export default {
  components: {
    cmdTransition,
    cmdInput
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
      //先跳转
      wx.navigateTo({
        url: "../updateTel/updateTel" //跳转页面的路径，可带参数 ？隔开，不同参数用 & 分隔；相对路径，不需要.wxml后缀
      });
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
$validate-margin-h: 72upx;
$validate-margin-v: 56upx;

.validate {
  margin-top: $validate-margin-v;
  margin-right: $validate-margin-h;
  margin-left: $validate-margin-h;

  &-title {
    font-size: 56upx;
    font-weight: 500;
  }

  &-explain {
    font-size: 28upx;
    color: #9e9e9e;
  }

  &-phone {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    border-bottom: 2upx #dedede solid;
    margin-top: $validate-margin-v;
    margin-bottom: 40upx;

    &-getcode {
      color: #3f51b5;
      text-align: center;
      min-width: 140upx;
    }
  }

  &-code {
    border-bottom: 2upx #dedede solid;
  }

  &-username {
    margin-top: $validate-margin-v;
    margin-bottom: 40upx;
    border-bottom: 2upx #dedede solid;
  }

  &-password {
    border-bottom: 2upx #dedede solid;
  }

  .btn-validate {
    margin-top: 100upx;
    border-radius: 50upx;
    font-size: 16px;
    color: #fff;
    background: linear-gradient(to right, #88a1f9, #9ac6ff);

    &-active {
      background: linear-gradient(to right, #365fff, #36bbff);
    }

    &-hover {
      background: linear-gradient(to right, #365fdd, #36bbfa);
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
