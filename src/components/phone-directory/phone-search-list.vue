<template>
  <view>
    <view class="search">
      <input @input="handleInput" class="search-input" type="text" focus placeholder="请输入要搜索的联系人">
    </view>
    <view class="search-main" v-if="keyword" style="display: flex;">
      <view class="search-main-errtitle" v-if="hasNoData">无搜索结果</view>
      <!-- <view class="search-main-title"
			hover-class="hover" 
			@click="handleClick"
			:hover-start-time="20" 
			:hover-stay-time="70" 
			v-for="item of list" 
			:key="item.id"
			:data-name="item.name"
			:data-id="item.id"
			:data-phoneNumber="item.phoneNumber">
				{{item.name}}
      </view>-->
      <phone-list
        :phones="phonesCopy"
        :letter="letter"
        :scrollAnimationOFF="scrollAnimationOFF"
        @change="handlePhoneListIndex"
        @reset="handleReset"
        @handleClick="handleClick"
      ></phone-list>
      <phone-alphabet
        :phones="phonesCopy"
        :phoneListIndex="phoneListIndex"
        @change="handleDatasetKey"
        @scrollAnimationOFF="handleScrollAnimationOFF"
        @reset="handleReset"
      ></phone-alphabet>
    </view>
  </view>
</template>

<script>
import phoneList from "./phone-list.vue";
import phoneAlphabet from "./phone-alphabet.vue";
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
      phonesCopy: {},
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
        this.phonesCopy = Object.assign(this.phones, this.phonesCopy);
        return;
      }
      this.timer = setTimeout(() => {
        const result = [];
        for (let i in this.phones) {
          this.phones[i].forEach(item => {
            if (
              item.spell.indexOf(this.keyword) > -1 ||
              item.name.indexOf(this.keyword) > -1
            ) {
              result.push(item);
            }
          });
        }
        if (result.length > 0) {
          this.phoneListIndex = result[0].spell.slice(0, 1).toUpperCase();
          this.phonesCopy = { [this.phoneListIndex]: result };
        }
        console.log("搜索结果：", this.phonesCopy);
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

<style>
.hover {
  background-color: #eee;
}
.search {
  background-color: #fff;
  padding: 10upx 20upx;
  border-bottom: 1px solid #e5e5e5;
}

.search-input {
  font-size: 28upx;
  border: 1px solid #e5e5e5;
  border-radius: 3px;
  padding: 10upx 20upx 10upx 20upx;
}

.search-main {
  height: 100%;
  padding-bottom: 20upx;
  background-color: #fff;
  overflow: hidden;
}

.search-main-errtitle,
.search-main-title {
  width: 100%;
  height: 92upx;
  line-height: 92upx;
  font-size: 32upx;
  padding: 0 20upx;
  border-bottom: 1px solid #e5e5e5;
}
</style>
