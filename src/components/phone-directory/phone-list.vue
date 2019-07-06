<template>
  <view>
    <scroll-view class="scroll-list" scroll-y="true">
      <view class="phone-list">
        <view class="selected-phones list-item" v-if="selectedPhones.length">
          <view class="list-item-content">
            <view class="list-item-title">已选</view>
            <label
              class="list-item-phone"
              style="width: 33%;"
              hover-class="commonly-hover"
              :hover-start-time="20"
              :hover-stay-time="70"
              v-for="(item,index) in selectedPhones"
              :key="index"
            >
              {{item}}
              <icon type="clear" size="14" @click="clearSelect(item)" class="btn-clear" />
            </label>
          </view>
        </view>
        <checkbox-group @change="handleClickHot">
          <view class="hot-phones list-item" v-if="hotPhonesCopy.length">
            <view class="list-item-content">
              <view class="list-item-title">热门</view>
              <label
                class="list-item-phone"
                hover-class="commonly-hover"
                :hover-start-time="20"
                :hover-stay-time="70"
                v-for="(item,index) in hotPhonesCopy"
                :key="index"
              >
                <checkbox :value="item.name" :checked="item.checked" />
                {{item.name}}
              </label>
            </view>
          </view>
        </checkbox-group>
        <checkbox-group @change="handleClick">
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
                <checkbox :value="innerItem.name" :checked="innerItem.checked" />
                {{innerItem.name}}
              </label>
            </view>
          </view>
        </checkbox-group>
      </view>
    </scroll-view>
    <button @click="confirm" class="btn-confirm">确定</button>
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
      hotPhonesCopy: [...this.hotPhones],
      selectedPhones: [],
      hotSelectedPhones: [],
      normalSelectedPhones: []
    };
  },
  watch: {
    selectedPhones: {
      handler(newVal, oldVal) {
        this.hotPhonesCopy.forEach(item => {
          let checked = this.selectedPhones.includes(item.name) ? true : false;
          this.$set(item, "checked", checked);
        });
        for (const key in this.phonesCopy) {
          let item = this.phonesCopy[key];
          item.forEach(phone => {
            let checked = this.selectedPhones.includes(phone.name)
              ? true
              : false;
            this.$set(phone, "checked", checked);
          });
        }
      },
      deep: true
    },
    stockCompanies: {
      handler(newVal, oldVal) {
        this.selectedPhones = this.stockCompanies
          ? [...this.stockCompanies.split(",")]
          : [];
      },
      deep: true
    }
  },
  computed: {},
  mounted() {
    this.selectedPhones = this.stockCompanies
      ? [...this.stockCompanies.split(",")]
      : [];
  },
  methods: {
    handleClick(e) {
      this.normalSelectedPhones = e.detail.value;
      this.selectedPhones = [
        ...new Set([...this.normalSelectedPhones, ...this.hotSelectedPhones])
      ];
    },
    handleClickHot(e) {
      this.hotSelectedPhones = e.detail.value;
      this.selectedPhones = [
        ...new Set([...this.normalSelectedPhones, ...this.hotSelectedPhones])
      ];
    },
    clearSelect(item) {
      this.selectedPhones = this.selectedPhones.filter(phone => phone !== item);
    },
    handlerCompany(company) {
      this.selectedPhones = [
        ...new Set([...this.selectedPhones, company.name])
      ];
    },
    confirm() {
      this.$emit("confirm", this.selectedPhones);
    }
  }
};
</script>

<style lang="scss" scoped>
.btn-clear {
  margin-left: 2rpx;
  top: -3px;
  position: relative;
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
