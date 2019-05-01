<template>
  <view class="update">
    <cmd-transition name="fade-up">
      <view v-if="status">
        <view class="update-phone">
          <cmd-input
            v-model="mobile.phone"
            type="number"
            focus
            maxlength="11"
            placeholder="请输入新手机号"
          ></cmd-input>
          <view
            class="update-phone-getcode"
            @tap="!safety.state ? fnGetPhoneCode() : ''"
          >{{!safety.state&&'获取验证码'||(safety.time+' s')}}</view>
        </view>
        <view class="update-code">
          <cmd-input v-model="mobile.code" type="number" maxlength="6" placeholder="请输入短信验证码"></cmd-input>
        </view>
        <button
          class="btn-update"
          :class="loginMobile ? 'btn-login-active':''"
          :disabled="!loginMobile"
          hover-class="btn-update-hover"
          @tap="fnLogin"
        >确定</button>
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
      // 账号登录部分数据
      account: {
        username: "",
        password: ""
      },
      usernameReg: /^[A-Za-z0-9]+$/,
      passwordReg: /^\w+$/,
      loginAccount: false,
      // 手机登录部分数据
      mobile: {
        phone: "",
        code: ""
      },
      phoneReg: /^[1](([3][0-9])|([4][5-9])|([5][0-3,5-9])|([6][5,6])|([7][0-8])|([8][0-9])|([9][1,8,9]))[0-9]{8}$/,
      loginMobile: false,
      // 验证码
      safety: {
        time: 60,
        state: false,
        interval: ""
      },
      status: true // true手机登录,false账号登录
    };
  },
  watch: {
    /**
     * 监听手机登录数值
     */
    mobile: {
      handler(newValue) {
        if (this.phoneReg.test(newValue.phone) && newValue.code.length === 6) {
          this.loginMobile = true;
        } else {
          this.loginMobile = false;
        }
      },
      deep: true
    },
    /**
     * 监听账号登录数值
     */
    account: {
      handler(newValue) {
        if (
          this.usernameReg.test(newValue.username) &&
          newValue.username.length >= 8 &&
          (this.passwordReg.test(newValue.password) &&
            newValue.password.length >= 8)
        ) {
          this.loginAccount = true;
        } else {
          this.loginAccount = false;
        }
      },
      deep: true
    }
  },
  methods: {
    /**
     * 登录按钮点击执行
     */
    fnLogin() {
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
    },
    /**
     * 改变登录方式状态 reset作为重置标识
     */
    fnChangeStatus(reset) {
      // 手机登录部分
      this.mobile = {
        phone: "",
        code: ""
      };
      this.loginMobile = false;
      // 账号登录部分
      this.account = {
        username: "",
        password: ""
      };
      this.loginAccount = false;
      // 验证码时间状态还原
      clearInterval(this.safety.interval);
      this.safety.time = 60;
      this.safety.state = false;
      if (!reset) {
        // 可以延迟3毫秒后切换
        this.status = !this.status;
      }
    },
    /**
     * 跳转注册页面
     */
    fnRegisterWin() {
      uni.navigateTo({
        url: "/pages/user/register/register"
      });
      /**
       * 改变状态重置，跳转不会摧毁实例
       */
      this.fnChangeStatus(true);
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
$update-margin-h: 72upx;
$update-margin-v: 56upx;

.update {
  margin-top: $update-margin-v;
  margin-right: $update-margin-h;
  margin-left: $update-margin-h;
  &-phone {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    border-bottom: 2upx #dedede solid;
    margin-top: $update-margin-v;
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
    margin-top: $update-margin-v;
    margin-bottom: 40upx;
    border-bottom: 2upx #dedede solid;
  }

  &-password {
    border-bottom: 2upx #dedede solid;
  }

  .btn-update {
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
