<template>
  <view class="news">
    <cmd-transition name="fade-up">
      <!-- 顶级tab -->
      <wuc-tab
        :tab-list="tabList"
        :tabCur.sync="TabCur"
        tab-class="text-center"
        select-class="tab-selected"
        @change="tabChange"
      ></wuc-tab>
      <!-- tab内容区 -->
      <div class="cu-bar bg-white solid-bottom" style="margin-top:100upx">
        <div class="content">
          <!-- 研报查询 -->
          <template v-if="!TabCur">
            <view class="search-box">
              <mSearch
                :mode="2"
                button="inside"
                placeholder="输入你想找的关键字"
                @search="doSearch(false)"
                @input="inputChange"
                @confirm="doSearch(false)"
                v-model="keyword"
                class="search-block"
              ></mSearch>
            </view>
            <!-- <ChooseLits :list="list" :arr="arr" @chooseLike="chooseLike"></ChooseLits> -->
            <div class="data-list"></div>
          </template>
          <!-- 研报精选 -->
          <template v-else>
            <div>研报精选</div>
          </template>
        </div>
      </div>
    </cmd-transition>
  </view>
</template>
<script>
import cmdTransition from "@/components/cmd-transition/cmd-transition.vue";
import wucTab from "@/components/wuc-tab/wuc-tab.vue";
import ChooseLits from "@/components/choose-Cade/choose-Cade.vue";
import mSearch from "@/components/mehaotian-search-revision/mehaotian-search-revision.vue";

export default {
  components: {
    cmdTransition,
    wucTab,
    ChooseLits,
    mSearch
  },
  data() {
    return {
      tabList: [{ name: "研报查询" }, { name: "研报精选" }], //顶级tab
      TabCur: 0, //当前选中的tab
      // list: ["行业", "类别", "机构", "日期", "页数"], //select选项卡
      // arr: [
      //   ["全部", "石油石化", "石油石化", "石油石化", "石油石化"],
      //   ["全部", "晨会", "周报", "东北证券", "东北证券"],
      //   ["全部", "5k以下", "5k-10k", "10k以上"],
      //   ["全部", "1个月内", "3个月内", "6个月内", "1年以内"],
      //   ["全部", "1-5页", "6-20页", "20页以上"]
      // ], //select选项值
      keyword: "" //搜索关键字
    };
  },
  onLoad() {},
  methods: {
    /**
     * tab切换
     */
    tabChange(index) {
      this.TabCur = index;
    },
    /**
     * select选择
     */
    // chooseLike(key) {
    //   console.log(key[0], key[1]);
    // },
    /**
     * 确认搜索框
     */
    doSearch() {},
    /**
     * 搜索框
     */
    inputChange() {}
  }
};
</script>

<style lang="scss">
.news {
  .text-center {
    text-align: center;
  }
  .cu-bar {
    margin-top: 0 !important;
  }
  .news {
    .boxa {
      padding-top: 0;
      .top_kbox {
        position: static;
      }
    }
  }
  .list_boxs {
    display: none;
  }
  .list_boxs2 {
    transform: none;
  }
  .tab-selected {
    color: $uni-color-primary;
    border-bottom: 1px solid $uni-color-primary;
  }
}
</style>
<style>
/* 搜索框 */
.search-block {
  width: 100%;
}
.serach .content .content-box .input.center {
  width: 100% !important;
}
.search-box {
  width: 95%;
  background-color: rgb(242, 242, 242);
  padding: 7.5px 2.5%;
  display: flex;
  justify-content: space-between;
}
.search-box .input-box {
  width: 85%;
  flex-shrink: 1;
  display: flex;
  justify-content: center;
  align-items: center;
}
.search-box .search-btn {
  width: 15%;
  margin: 0 0 0 2%;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-shrink: 0;
  font-size: 14px;
  color: #fff;
  background: linear-gradient(to right, #ff9801, #ff570a);
  border-radius: 30px;
}
.search-box .input-box > input {
  width: 100%;
  height: 30px;
  font-size: 16px;
  border: 0;
  border-radius: 30px;
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  padding: 0 3%;
  margin: 0;
  background-color: #ffffff;
}
.placeholder-class {
  color: #9e9e9e;
}
.search-keyword {
  width: 100%;
  background-color: rgb(242, 242, 242);
}
.keyword-list-box {
  height: calc(100vh - 55px);
  padding-top: 5px;
  border-radius: 10px 10px 0 0;
  background-color: #fff;
}
.keyword-entry-tap {
  background-color: #eee;
}
.keyword-entry {
  width: 94%;
  height: 40px;
  margin: 0 3%;
  font-size: 15px;
  color: #333;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: solid 1px #e7e7e7;
}
.keyword-entry image {
  width: 30px;
  height: 30px;
}
.keyword-entry .keyword-text,
.keyword-entry .keyword-img {
  height: 40px;
  display: flex;
  align-items: center;
}
.keyword-entry .keyword-text {
  width: 90%;
}
.keyword-entry .keyword-img {
  width: 10%;
  justify-content: center;
}
.keyword-box {
  height: calc(100vh - 55px);
  border-radius: 10px 10px 0 0;
  background-color: #fff;
}
.keyword-box .keyword-block {
  padding: 5px 0;
}
.keyword-box .keyword-block .keyword-list-header {
  width: 94%;
  padding: 5px 3%;
  font-size: 13.5px;
  color: #333;
  display: flex;
  justify-content: space-between;
}
.keyword-box .keyword-block .keyword-list-header image {
  width: 20px;
  height: 20px;
}
.keyword-box .keyword-block .keyword {
  width: 94%;
  padding: 3px 3%;
  display: flex;
  flex-flow: wrap;
  justify-content: flex-start;
}
.keyword-box .keyword-block .hide-hot-tis {
  display: flex;
  justify-content: center;
  font-size: 14px;
  color: #6b6b6b;
}
.keyword-box .keyword-block .keyword > view {
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 30px;
  padding: 0 10px;
  margin: 5px 10px 5px 0;
  height: 30px;
  font-size: 14px;
  background-color: rgb(242, 242, 242);
  color: #6b6b6b;
}
</style>
