<template>
  <view>
    <scroll-view
      class="scroll-list"
      :scroll-top="1"
      scroll-y="true"
      :scroll-with-animation="scrollAnimationOFF"
      :scroll-into-view="scrollViewId"
      @scroll="handleScroll"
    >
      <view class="phone-list">
        <checkbox-group @change="handleClick">
          <view class="list-item" v-for="(item, key) of phones" :key="key" :id="key">
            <view class="list-item-content">
              <view class="list-item-title">{{key}}</view>
              <label
                class="list-item-phone"
                hover-class="commonly-hover"
                :hover-start-time="20"
                :hover-stay-time="70"
                v-for="innerItem in item"
                :key="innerItem.id"
                :data-name="innerItem.name"
                :data-id="innerItem.id"
                :data-phoneNumber="innerItem.phoneNumber"
              >
                <checkbox :value="innerItem.name"/>
                {{innerItem.name}}
              </label>
            </view>
          </view>
        </checkbox-group>
      </view>
    </scroll-view>
  </view>
</template>

<script>
export default {
  name: "phone-list",
  props: {
    phones: Object,
    letter: String,
    scrollAnimationOFF: Boolean
  },
  data() {
    return {
      winHeight: 0,
      scrollTop: 0,
      letterDetails: [],
      timer: null,
      institutions: []
    };
  },
  computed: {
    scrollViewId() {
      return this.letter;
    }
  },
  mounted() {
    // #ifndef APP-PLUS
    // this.winHeight = uni.getSystemInfoSync().windowHeight - 49.5;
    // //#endif
    // //#ifdef APP-PLUS
    // this.winHeight = uni.getSystemInfoSync().windowHeight - 100;
    //#endif
  },
  methods: {
    handleClick: function(e) {
      this.$emit("handleClick", e.detail.value);
    },
    handleScroll(e) {
      // if (this.letterDetails.length === 0) {
      //   let view = uni.createSelectorQuery().selectAll(".list-item");
      //   view
      //     .boundingClientRect(data => {
      //       let top = data[0].top;
      //       data.forEach((item, index) => {
      //         item.top = item.top - top;
      //         item.bottom = item.bottom - top;
      //         this.letterDetails.push({
      //           id: item.id,
      //           top: item.top,
      //           bottom: item.bottom
      //         });
      //       });
      //     })
      //     .exec();
      // }
      // const scrollTop = e.detail.scrollTop;
      // this.letterDetails.some((item, index) => {
      //   if (scrollTop >= item.top && scrollTop <= item.bottom - 5) {
      //     this.$emit("change", item.id);
      //     this.$emit("reset", true);
      //     return true;
      //   }
      // });
    }
  },
  /**
   * 确定
   */
  confirm() {}
};
</script>

<style lang="scss">
.commonly-hover {
  background-color: #eee;
}

.scroll-list {
  flex: 1;
  max-height: 400upx;
  overflow: auto;
}

.phone-list {
  display: flex;
  background-color: #fff;
  flex-direction: column;
  position: relative;
  width: 100%;
  padding: 20upx 40upx 0;
  box-sizing: border-box;
}

.list-item {
  width: 100%;
  height: 92upx;
  background-color: #fff;
  height: 100%;
  &-content {
    width: 100%;
    padding-left: 56upx;
    display: flex;
    align-items: center;
    flex-wrap: wrap;
    position: relative;
    box-sizing: border-box;
  }
}

.list-item > .list-item-phone {
  font-weight: normal;
}

.list-item-title {
  position: absolute;
  top: 0;
  left: 0;
  line-height: 92upx;
  color: #4a4a4a;
  font-size: 28upx;
}

/* .list-item-title, */
.list-item-phone {
  height: 92upx;
  line-height: 92upx;
  font-size: 28upx;
  color: #4a4a4a;
  padding-right: 40upx;
}
</style>
