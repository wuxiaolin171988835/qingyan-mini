<template>
  <cmd-page-body>
    <view class="position">
      <div class="header">
        <text class="title">{{positionDetail.name}}</text>
        <view class="desc">
          <text class="item first">发布时间：{{positionDetail.release_time}}</text>
        </view>
      </div>
      <view class="content">
        <view class="title">岗位描述</view>
        <view class="desc">
          <view v-for="(duty,i) in positionDetail.duty" :key="i">{{duty}}</view>
        </view>
        <view class="title">任职要求</view>
        <view class="desc">
          <view v-for="(demand,j) in positionDetail.demand" :key="j">{{demand}}</view>
        </view>
        <view class="tips">如您有求职意向，请将您的简历发送至hr@qxsearch.net，我们会尽快和您联系。</view>
      </view>
    </view>
  </cmd-page-body>
</template>

<script>
import cmdPageBody from "@/components/cmd-page-body/cmd-page-body.vue";
const POSITION_DETAIL=[
  {
    id:1,
    name: '产品总监',
    release_time: '2019-06-13',
    duty: ['1.紧密跟踪用户，从用户端调研业务需求，对用户需求进行重新整理并进行深入分析；','2.负责产品功能设计，持续优化产品体验；','3.关注产品运营数据，提升产品核心竞争力。'],
    demand: ['1.本科及以上学历，2年以上产品经理经验，有独立负责过一款APP或一条产品线的优先考虑；','2.对UI、UE敏感，熟悉Android与iOS的交互规划，喜欢研究各类APP；','3.对用户需求敏感，有从需求分析到产品发布全流程的经验，能独立设计产品。'],
    salary_range: '10-30K/月'
  },
  {
    id:2,
    name: '技术总监',
    release_time: '2019-06-13',
    duty: ['1.负责网络爬虫的开发及pdf/word及其他文档的内容解析；','2.负责金融垂直领域的搜索引擎的框架搭建及技术实现。'],
    demand: ['1.熟悉垂直领域搜索引擎的搭建，及常见的技术框架；熟悉elastic-search；','2.精通JAVA、熟悉python;','3.熟悉MySQL, Mongo DB; 熟悉Linux平台开发。'],
    salary_range: '10-30K/月'
  },
  {
    id:3,
    name: '金融实习生',
    release_time: '2019-06-13',
    duty: ['1.负责对产品进行业务测试及用户体验测试，并撰写测试报告；','2.人工整理或撰写金融研究报告。'],
    demand: ['1.金融、经济或相关专业的本科或研究生在读；','2.平均每天至少有4小时工作时间。可以接受远程办公。'],
    salary_range: '100-400/天'
  },
  {
    id:4,
    name: '技术实习生',
    release_time: '2019-06-13',
    duty: ['1.根据需求对产品前后端进行开发或维护。'],
    demand: ['1.计算机或相关专业的本科或研究生在读；','2.平均每天至少有4小时工作时间。可以接受远程办公；','3.悉Linux平台的开发；熟悉Java或python。熟悉前端或后端技术开发；','4.有搜索相关或自然语言处理相关的经验优先。'],
    salary_range: '100-500/天'
  },
]
export default {
  components: { cmdPageBody },
  data() {
    return {
      positionDetail:{}
    };
  },
  onLoad(options) {
    let index = POSITION_DETAIL.findIndex(item=>item.id===JSON.parse(options.position_id));
    this.positionDetail=POSITION_DETAIL[index]
  },
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
        // border-right: 1upx solid $uni-text-color-grey;
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
