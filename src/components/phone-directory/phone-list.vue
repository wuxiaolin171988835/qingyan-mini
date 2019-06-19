<template>
  <view>
    <scroll-view
      class="scroll-list"
      scroll-y="true"
    >
      <view class="phone-list">
        <checkbox-group @change="handleClick">
          <view class="selected-phones list-item" v-if="selectedPhones.length">
            <view class="list-item-content">
              <view class="list-item-title">已选</view>
              <label
                class="list-item-phone"
                hover-class="commonly-hover"
                :hover-start-time="20"
                :hover-stay-time="70"
                v-for="(item,index) in selectedPhones"
                :key="index"
              >
                <checkbox :value="item.name" :checked="item.checked"/>
                {{item.name}}
              </label>
            </view>
          </view>
          <view class="hot-phones list-item" v-if="hotPhones.length">
            <view class="list-item-content">
              <view class="list-item-title">热门</view>
              <label
                class="list-item-phone"
                hover-class="commonly-hover"
                :hover-start-time="20"
                :hover-stay-time="70"
                v-for="(item,index) in hotPhones"
                :key="index"
              >
                <checkbox :value="item.name" :checked="item.checked"/>
                {{item.name}}
              </label>
            </view>
          </view>
          <view class="phones list-item" v-for="(item, key) of phonesCopy" :key="key" :id="key">
            <view class="list-item-content">
              <view class="list-item-title">{{key}}</view>
              <label
                class="list-item-phone"
                hover-class="commonly-hover"
                :hover-start-time="20"
                :hover-stay-time="70"
                v-for="(innerItem,index) in item"
                :key="index"
              >
                <checkbox :value="innerItem.name" :checked="innerItem.checked"/>
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
    hotPhones: Array,
    stockCompanies: String
  },
  data() {
    return {
      phonesCopy: this.phones,
      selectedPhones: []
    };
  },
  watch: {},
  computed: {
    selected() {
      this.selectedPhones = []
      for (let i in this.phonesCopy) {
        this.phonesCopy[i].forEach(item => {
          if (this.stockCompanies.indexOf(item.name) > -1) {
            item.checked = true;
            this.selectedPhones.push(item);
          } else {
            item.checked = false;
          }
        });
      }
      this.$forceUpdate();
    }
  },
  mounted() {
    
  },
  methods: {
    handleClick: function(e) {
      this.$emit("handleClick", e.detail.value);
    },
    
  },
 
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
    padding-left: 80upx;
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
  width: 48%;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
</style>
