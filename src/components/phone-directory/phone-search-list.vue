<template>
  <view>
    <view class="search">
      <text class="searc-desc" style="display: inline-block;width:340upx;">请选择机构，支持多选</text>
      <view class="search-block">
        <span class="icon-search"></span>
        <input class="search-input" type="text" v-model="keyword">
        <view class="search-result">
          <ul v-if="isShowResult">
            <li
              v-for="(item,index) in searchResult"
              :key="index"
              :data-cid="item.id"
              :data-index="index"
              @click="selectedCompany(item)"
            >{{item.name}}</li>
          </ul>
          <view class="no-data" v-if="keyword && searchResult.length==0">未匹配到数据！</view>
        </view>
      </view>
    </view>
    <view class="search-main" style="display: flex;">
      <view class="search-main-errtitle" v-if="hasNoData">无搜索结果</view>
      <phone-list
        :phones="phones"
        :hotPhones="hotPhones"
        :letter="letter"
        :scrollAnimationOFF="scrollAnimationOFF"
        @change="handlePhoneListIndex"
        @reset="handleReset"
        @handleClick="handleClick"
        :stockCompanies="stockCompanies"
        @confirm="confirm"
        ref="phonesList"
      ></phone-list>
    </view>
  </view>
</template>

<script>
import phoneList from "./phone-list.vue";
import phoneAlphabet from "./phone-alphabet.vue";
export default {
  name: "phone-search-list",
  props: {
    phones: Object,
    stockCompanies: String,
    hotPhones: Array
  },
  components: {
    phoneList,
    phoneAlphabet
  },
  data() {
    return {
      keyword: "",
      timer: null,
      letter: "A",
      scrollAnimationOFF: true,
      phoneListIndex: "A",
      reset: true,
      searchResult: [],
      isShowResult: false
    };
  },
  computed: {},
  watch: {
    keyword() {
      this.searchResult = [];
      for (let i in this.phones) {
        this.phones[i].forEach(item => {
          if (
            item.abbr.indexOf(this.keyword.toUpperCase()) === 0 ||
            item.name.indexOf(this.keyword) === 0
          ) {
            this.searchResult.push(item);
          }
        });
      }
      if (!this.keyword) {
        this.searchResult = [];
      }
      this.isShowResult = this.searchResult.length ? true : false;
    }
  },
  methods: {
    selectedCompany(company) {
      this.isShowResult = false;
      this.keyword = "";
      this.$refs.phonesList.handlerCompany(company);
    },
    handleClick(value) {
      this.$emit("paramClick", value);
    },
    handleDatasetKey(val) {
      this.letter = val;
    },
    handleScrollAnimationOFF(val) {
      this.scrollAnimationOFF = val;
    },
    handlePhoneListIndex(val) {
      if (this.reset) {
        this.phoneListIndex = val;
      }
    },
    handleReset(val) {
      if (val) {
        this.letter = "";
      }
      this.reset = val;
    },
    confirm(selectedPhones) {
      this.$emit("confirm",selectedPhones);
    }
  }
};
</script>

<style lang="scss">
.hover {
  background-color: #eee;
}
.search {
  background-color: #fff;
  padding: 0 36upx 0 40upx;
  border-bottom: 1px solid #e5e5e5;
  display: flex;
  background-color: #f4f5f6;
  height: 98upx;
  line-height: 98upx;
  &-block {
    display: flex;
    height: 60upx;
    margin-left: 38upx;
    margin-top: 18upx;
    background: #fff;
    left: 166px;
    border-radius: 5px;
    position: absolute;
    border: 1px solid rgba(231, 230, 230, 1);
    .icon-search {
      display: inline-block;
      width: 34upx;
      height: 32upx;
      margin: 13upx 29upx 0 27upx;
      background-image: url("../../static/icon_search.png");
      background-size: 100% 100%;
    }
  }
  &-result {
    position: absolute;
    top: 65rpx;
    left: 0;
    width: 100%;
    z-index: 3;
    max-height: 300upx;
    overflow: auto;
    background: #fff;
    border: 1px solid rgba(231, 230, 230, 1);
    border-top: 0;
    li {
      width: 100%;
      padding: 8rpx 20rpx;
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
    }
    .no-data {
      color: rgba(231, 230, 230, 1);
      padding: 30rpx;
    }
  }
}

.search-input {
  font-size: 28upx;
  border: 1px solid #e5e5e5;
  border-radius: 3px;
  padding: 0 20upx;
  font-size: 26upx;
  color: #b2b2b2;
  height: 60upx;
  border: none;
}

.search-main {
  height: 100%;
  background-color: #fff;
  overflow: hidden;
  &-errtitle {
    height: 100upx;
    line-height: 100upx;
    text-align: center;
    color: #4a4a4a;
    display: block;
    width: 100%;
  }
}
</style>
