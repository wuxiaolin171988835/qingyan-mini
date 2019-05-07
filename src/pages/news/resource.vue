<template>
  <cmd-page-body>
    <view class="resource">
      <embed
        src="http://image.cache.timepack.cn/nodejs.pdf"
        type="application/pdf"
        width="100%"
        height="100%"
      >
    </view>
  </cmd-page-body>
</template>

<script>
import cmdPageBody from "@/components/cmd-page-body/cmd-page-body.vue";
export default {
  components: { cmdPageBody },
  data() {
    return {};
  },
  onLoad() {},
  methods: {}
};
</script>

<style lang="scss">
.resource {
}
</style>
