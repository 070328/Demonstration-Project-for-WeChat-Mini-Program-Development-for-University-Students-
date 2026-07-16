<template>
  <view class="section-header">
    <view class="title-box"><text class="title-icon">{{ icon }}</text><text class="title-text">{{ title }}</text></view>
    <text class="more" @click="handleMoreClick">更多 ></text>
  </view>
</template>
<script>
export default {
  props: { icon: String, title: String },
  methods: { handleMoreClick() { this.$emit('more-click', this.title); } }
}
</script>
<style scoped>
.section-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 20rpx; }
.title-box { display: flex; align-items: center; }
.title-icon { font-size: 36rpx; margin-right: 10rpx; }
.title-text { font-size: 32rpx; font-weight: bold; color: #1f1f1f; }
.more { font-size: 26rpx; color: #8c8f97; }
</style>
