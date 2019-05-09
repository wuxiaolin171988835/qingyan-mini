<template>
  <view>
    <view class="search">
      <text class="searc-desc">请选择机构，支持多选</text>
      <input @input="handleInput" class="search-input" type="text">
    </view>
    <view class="search-main" style="display: flex;">
      <view class="search-main-errtitle" v-if="hasNoData">无搜索结果</view>
      <phone-list
        :phones="phonesCopy"
        :letter="letter"
        :scrollAnimationOFF="scrollAnimationOFF"
        @change="handlePhoneListIndex"
        @reset="handleReset"
        @handleClick="handleClick"
      ></phone-list>
    </view>
    <button @click="confirm" class="btn-confirm">确定</button>
  </view>
</template>

<script>
import phoneList from "./phone-list.vue";
import phoneAlphabet from "./phone-alphabet.vue";
import { constants } from "crypto";
export default {
  name: "phone-search-list",
  props: {
    phones: Object
  },
  components: {
    phoneList,
    phoneAlphabet
  },
  data() {
    return {
      phonesCopy: JSON.parse(JSON.stringify(this.phones)),
      keyword: "",
      timer: null,
      letter: "A",
      scrollAnimationOFF: true,
      phoneListIndex: "A",
      reset: true
    };
  },
  computed: {
    hasNoData() {
      return JSON.stringify(this.phonesCopy) == "{}";
    }
  },
  watch: {
    keyword() {
      if (this.timer) {
        clearTimeout(this.timer);
      }
      if (!this.keyword) {
        this.phonesCopy = JSON.parse(JSON.stringify(this.phones));
        console.log("清空筛选：", this.phonesCopy);
        return;
      }
      this.timer = setTimeout(() => {
        const result = [];
        for (let i in this.phones) {
          this.phones[i].forEach(item => {
            if (
              item.spell.indexOf(this.keyword) === 0 ||
              item.name.indexOf(this.keyword) === 0
            ) {
              result.push(item);
            }
          });
        }
        if (result.length > 0) {
          this.phoneListIndex = result[0].spell.slice(0, 1).toUpperCase();
          this.phonesCopy = { [this.phoneListIndex]: result };
        } else {
          this.phonesCopy = {};
        }
      }, 100);
    }
  },
  methods: {
    handleInput(e) {
      this.keyword = e.detail.value;
    },
    // handleClick(e) {
    //   this.$emit("paramClick", e.target.dataset);
    // },
    handleClick(e) {
      this.$emit("paramClick", e);
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
}

.search-input {
  font-size: 28upx;
  border: 1px solid #e5e5e5;
  border-radius: 3px;
  padding: 0 20upx;
  height: 36upx;
  font-size: 26upx;
  color: #b2b2b2;
  margin-left: 38upx;
  margin-top: 31upx;
  background: #fff;
  flex: 1;
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
.btn-confirm {
  width: 100%;
  height: 102upx;
  line-height: 102upx;
  background-color: #f4f5f6;
  color: $uni-color-primary;
  font-size: 32upx;
  border: none;
}
</style>
