<script>
export default {
  onLaunch: function() {
    console.log("App Launch");
    // #ifdef APP-PLUS
    // 锁定屏幕方向
    plus.screen.lockOrientation("portrait-primary"); //锁定
    // 检测升级
    uni.request({
      url: "https://uniapp.dcloud.io/update", //检查更新的服务器地址
      data: {
        appid: plus.runtime.appid,
        version: plus.runtime.version,
        imei: plus.device.imei
      },
      success: res => {
        console.log("success", res);
        if (res.statusCode == 200 && res.data.isUpdate) {
          let openUrl =
            plus.os.name === "iOS" ? res.data.iOS : res.data.Android;
          // 提醒用户更新
          uni.showModal({
            title: "更新提示",
            content: res.data.note ? res.data.note : "是否选择更新",
            success: showResult => {
              if (showResult.confirm) {
                plus.runtime.openURL(openUrl);
              }
            }
          });
        }
      }
    });
    // #endif
  },
  onShow: function() {
    console.log("App Show");
  },
  onHide: function() {
    console.log("App Hide");
  }
};
</script>

<style lang="scss">
@import "../graceUI/graceUI.css";
/* uni.css - 通用组件、模板样式库，可以当作一套ui库应用 */
@import "./common/uni.css";

/* 以下样式用于 hello uni-app 演示所需 */
page {
  background-color: #f4f5f6;
  font-size: 28upx;
  line-height: 1.8;
  height: 100%;
}

.uni-header-logo {
  padding: 30upx;
  text-align: center;
  margin-top: 10upx;
}

.uni-header-logo image {
  width: 140upx;
  height: 140upx;
}

.uni-hello-text {
  color: #7a7e83;
}

.uni-hello-addfile {
  text-align: center;
  line-height: 300upx;
  background: #fff;
  padding: 50upx;
  margin-top: 10px;
  font-size: 38upx;
  color: #808080;
}
/* checkbox未选中时样式 */
.news {
  checkbox .wx-checkbox-input {
    /* 自定义样式.... */
    width: 30upx;
    height: 30upx;
    background-color: rgba(233, 233, 233, 1);
    border-radius: 100%;
  }

  /* checkbox选中时样式 */

  checkbox .wx-checkbox-input.wx-checkbox-input-checked::before {
    /* 自定义样式.... */
    content: "";
    position: absolute;
    top: 50%;
    left: 50%;
    width: 14upx;
    height: 14upx;
    border-radius: 50%;
    background-color: rgba(59, 143, 209, 1);
    transform: translate(-50%, -50%);
    box-sizing: border-box;
  }
}
</style>
