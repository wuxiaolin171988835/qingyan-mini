<template>
  <view class="about">
    <cmd-transition name="fade-up">
      <wuc-tab
        :tab-list="tabList"
        :tabCur.sync="TabCur"
        tab-class="text-center bg-white"
        select-class="tab-selected"
        @change="tabChange"
      ></wuc-tab>
      <div class="cu-bar bg-white solid-bottom" style="margin-top:100upx;">
        <template v-if="!TabCur">
          <navigator url="../position/position" open-type="navigate">
            <cmd-cell-item
              v-for="i in 10"
              :key="i"
              title="销售经理"
              brief="职位类别：采销类"
              addon="2019-03-01"
              arrow
            />
          </navigator>
        </template>
        <template v-else>
          <h3 class="title">公司简介</h3>
          <p class="desc">
            北京青彦科技有限公司投身于金融科技领域，致力于为金融投资研究工作提供只会分析的服务与工具，北京青彦科技有限公司投身于金融科技领域，致力于为金融投资研究工作提供只会分析的服务与工具北京青彦科技有限公司投身于金融科技领域，
            致力于为金融投资研究工作提供只会分析的服务与工具北京青彦科技有限公司投身于金融科技领域，致力于为金融投资研究工作提供只会分析的服务与工具北京青彦科技有限公司投身于金融科技领域，致力于为金融投资研究工作提供只会分析的服务与
            工具北京青彦科技有限公司投身于金融科技领域，致力于为金融投资研究工作提供只会分析的服务与工具北京青彦科技有限公司投身于金融科技领域，致力于为金融投资研究工作提供只会分析的服务与工具
          </p>
          <p class="desc">
            北京青彦科技有限公司投身于金融科技领域，致力于为金融投资研究工作提供只会分析的服务与工具，北京青彦科技有限公司投身于金融科技领域，致力于为金融投资研究工作提供只会分析的服务与工具北京青彦科技有限公司投身于金融科技领域，
            致力于为金融投资研究工作提供只会分析的服务与工具北京青彦科技有限公司投身于金融科技领域，致力于为金融投资研究工作提供只会分析的服务与工具北京青彦科技有限公司投身于金融科技领域，致力于为金融投资研究工作提供只会分析的服务与
            工具北京青彦科技有限公司投身于金融科技领域，致力于为金融投资研究工作提供只会分析的服务与工具北京青彦科技有限公司投身于金融科技领域，致力于为金融投资研究工作提供只会分析的服务与工具
          </p>
          <h3 class="title">公司团队</h3>
          <p class="desc">
            北京青彦科技有限公司投身于金融科技领域，致力于为金融投资研究工作提供只会分析的服务与工具，北京青彦科技有限公司投身于金融科技领域，致力于为金融投资研究工作提供只会分析的服务与工具北京青彦科技有限公司投身于金融科技领域，
            致力于为金融投资研究工作提供只会分析的服务与工具北京青彦科技有限公司投身于金融科技领域，致力于为金融投资研究工作提供只会分析的服务与工具北京青彦科技有限公司投身于金融科技领域，致力于为金融投资研究工作提供只会分析的服务与
            工具北京青彦科技有限公司投身于金融科技领域，致力于为金融投资研究工作提供只会分析的服务与工具北京青彦科技有限公司投身于金融科技领域，致力于为金融投资研究工作提供只会分析的服务与工具
          </p>
        </template>
      </div>
    </cmd-transition>
  </view>
</template>

<script>
import cmdTransition from "@/components/cmd-transition/cmd-transition.vue";
import wucTab from "@/components/wuc-tab/wuc-tab.vue";
import cmdCellItem from "@/components/cmd-cell-item/cmd-cell-item.vue";

export default {
  components: {
    cmdTransition,
    wucTab,
    cmdCellItem
  },
  data() {
    return {
      tabList: [{ name: "加入我们" }, { name: "关于我们" }],
      TabCur: 0
    };
  },
  onLoad() {},
  methods: {
    tabChange(index) {
      this.TabCur = index;
    }
  }
};
</script>

<style lang="scss">
.about {
  .navigator-hover {
    background-color: initial;
    opacity: initial;
  }
  .cu-bar {
    margin-top: 20upx !important;
  }
  .text-center {
    text-align: center;
  }
  .hot {
    padding: 40upx 0;
  }
  .title {
    font-size: 40upx;
    text-align: center;
  }
  .desc {
    line-height: 1.6;
    padding-bottom: 80upx;
  }
  .tab-selected {
    color: $uni-color-primary;
    border-bottom: 1px solid $uni-color-primary;
  }
}
</style>
