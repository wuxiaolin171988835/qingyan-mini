<template>
  <view class="position">
    <cmd-transition name="fade-up">
      <div class="header">
        <h3 class="title">销售经理</h3>
        <ul class="desc">
          <li class="item first">职位类别：采销类</li>
          <li class="item">所属部门：销售部</li>
          <li class="item">2019-03-01</li>
        </ul>
      </div>
      <div class="content">
        <span class="title">岗位描述</span>
        <p class="desc">
          针对企业员工关怀产品，开发企业行政/hr／工会类型的客户；
          宣传公司产品、服务、品牌理念，完成个人营收业绩；
          发现客户需求，为客户提供优质的员工关怀解决方案；
          和客户保持良好的沟通关系，代表公司回馈关怀客户；
          收集、反馈市场信息，帮助公司完善产品设计，帮助公司制定业绩规划；
        </p>
        <span class="title">任职要求</span>
        <p class="desc">
          两年以上销售、市场或公关类的工作经验；
          熟悉企业生日、娱乐、团建、下午茶等服务类项目的运作；
          大专以上学历，30岁以下；
          具备优秀的沟通能力，机会洞察力和亲和力；
          性格开朗、个性阳光，形象气质佳。
        </p>
        <p class="tips">如您有求职意向，请将简历发送至HR@QINGYAN.COM，我们会尽快和您联系。</p>
      </div>
    </cmd-transition>
  </view>
</template>

<script>
import cmdTransition from "@/components/cmd-transition/cmd-transition.vue";
export default {
  components: { cmdTransition },
  data() {
    return {};
  },
  onLoad() {},
  methods: {}
};
</script>

<style lang="scss">
.position {
  .header {
    .title {
      font-size: 40upx;
      padding: 40upx 0 30upx;
    }
    .desc {
      padding-bottom: 10upx;
      border-bottom: 1px solid $uni-text-color-grey;
      width: 100%;
      color: $uni-text-color-grey;
      .item {
        display: inline;
        padding: 0 15upx;
        border-right: 1px solid $uni-text-color-grey;
        &.first {
          padding-left: 0;
        }
      }
    }
  }
  .content {
    .title {
      font-size: 32upx;
      color: $uni-color-primary;
      // padding: 40upx 0 30upx;
      height: 150upx;
      line-height: 150upx;
    }
    .desc {
      line-height: 1.6;
    }
    .tips {
      color: $uni-color-primary;
      margin-top: 100upx;
    }
  }
}
</style>
