<template>
  <view class="update-name">
    <cmd-transition name="fade-up">
      <view class="username">
        <cmd-input v-model="username"></cmd-input>
      </view>
      <button class="btn-save" hover-class="btn-validate-hover" @tap="save">保存</button>
    </cmd-transition>
  </view>
</template>

<script>
import cmdTransition from "@/components/cmd-transition/cmd-transition.vue";
import cmdInput from "@/components/cmd-input/cmd-input.vue";
export default {
  components: {
    cmdTransition,
    cmdInput
  },
  data() {
    return {
      username: "某某"
    };
  },
  onLoad() {},
  methods: {
    /**
     * 保存用户名
     */
    save() {}
  }
};
</script>

<style lang="scss">
.update-name {
  .username {
    border-bottom: 2upx #dedede solid;
  }
  .btn-save {
    margin-top: 100upx;
    border-radius: 50upx;
    font-size: 16px;
    color: #fff;
    background: linear-gradient(to right, #88a1f9, #9ac6ff);

    &-active {
      background: linear-gradient(to right, #365fff, #36bbff);
    }

    &-hover {
      background: linear-gradient(to right, #365fdd, #36bbfa);
    }
  }
}
</style>
