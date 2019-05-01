<template>
  <view class="user">
    <cmd-transition name="fade-up">
      <cmd-cell-item title="头像" slot-right arrow>
        <cmd-avatar src="https://avatar.bbs.miui.com/images/noavatar_small.gif"></cmd-avatar>
      </cmd-cell-item>
      <navigator url="../updateName/updateName" open-type="navigate">
        <cmd-cell-item title="姓名" addon="胡凯" arrow></cmd-cell-item>
      </navigator>
      <cmd-cell-item title="公司名称" addon="青彦科技" arrow/>
      <cmd-cell-item title="邮箱" addon="HR@QINGYAN.COM" arrow/>
      <navigator url="../validateTel/validateTel" open-type="navigate">
        <cmd-cell-item title="手机" addon="13812345678" arrow/>
      </navigator>
    </cmd-transition>
  </view>
</template>

<script>
import cmdTransition from "@/components/cmd-transition/cmd-transition.vue";
import cmdCellItem from "@/components/cmd-cell-item/cmd-cell-item.vue";
import cmdAvatar from "@/components/cmd-avatar/cmd-avatar.vue";
export default {
  components: { cmdTransition, cmdCellItem, cmdAvatar },
  data() {
    return {};
  },
  onLoad() {},
  methods: {}
};
</script>

<style>
</style>
