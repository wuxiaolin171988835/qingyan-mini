<template>
  <cmd-page-body>
    <view class="position">
      <div class="header">
        <text class="title">销售经理</text>
        <view class="desc">
          <text class="item first">职位类别：采销类</text>
          <text class="item">所属部门：销售部</text>
          <text class="item">2019-03-01</text>
        </view>
      </div>
      <view class="content">
        <view class="title">岗位描述</view>
        <view class="desc">
          <text>
            全面负责事业部技术研发和项目管理工作，带领团队完成公司制定的研发目标；
            规划部门技术发展路线，及时了解和监督技术发展战略规划的执行情况；
            参与重大技术项目的决策，指导并参与核心代码的书写，组织解决项目开发过程中的重大技术问题；
            监督及指导技术部门的工作，建立并完善公司各项研发规范及流程；
            培养技术人才，提高团队整体专业技能。
          </text>
        </view>
        <view class="title">任职要求</view>
        <view class="desc">
          <text>
            三年以上后端开发经验，精通JAVA，熟悉python者优先;
            熟练运用常用的数据库MySQL, MongoDB、Redis等;
            熟悉垂直领域搜索引擎的搭建，及常见的技术框架；熟悉Elastic Search或者Lucene等搜索平台技术。
            熟悉Linux操作系统及shell; 有阿里云使用经验者优先。
          </text>
        </view>
        <view class="tips">如您有求职意向，请将您的简历发送至HR@QINGYAN.COM，我们会尽快和您联系。</view>
      </view>
    </view>
  </cmd-page-body>
</template>

<script>
import cmdPageBody from "@/components/cmd-page-body/cmd-page-body.vue";
export default {
  components: { cmdPageBody },
  data() {
    return {};
  },
  onLoad() {},
  methods: {}
};
</script>

<style lang="scss">
.position {
  padding: 0 40upx 75upx;
  background: #fff;
  min-height: 100%;
  .header {
    color: #4a4a4a;
    .title {
      font-size: 34upx;
      padding: 59upx 0 24upx;
      display: block;
    }
    .desc {
      font-size: 24upx;
      height: 56upx;
      line-height: 56upx;
      border-bottom: 1upx solid $uni-text-color-grey;
      width: 100%;
      color: $uni-text-color-grey;
      .item {
        display: inline;
        padding: 0 15upx;
        border-right: 1upx solid $uni-text-color-grey;
        &.first {
          padding-left: 0;
        }
      }
    }
  }
  .content {
    line-height: 40upx;
    .title {
      font-size: 28upx;
      color: $uni-color-primary;

      padding: 51upx 0 27upx;
    }
    .desc {
      font-size: 24upx;
      color: #4a4a4a;
      border-bottom: 1upx solid #e3e3e3;
      padding-bottom: 38upx;
    }
    .tips {
      color: $uni-color-primary;
      margin-top: 30upx;
      line-height: 40upx;
      font-size: 24upx;
    }
  }
}
</style>
