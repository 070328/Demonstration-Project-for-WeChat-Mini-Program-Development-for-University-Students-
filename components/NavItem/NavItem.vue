<template>
  <view class="nav-item" @click="handleClick">
    <view class="nav-icon-wrap"><text class="nav-emoji">{{ emoji }}</text></view>
    <text class="nav-text">{{ text }}</text>
  </view>
</template>
<script>
export default {
  props: { text: String, emoji: String },
  methods: { handleClick() { this.$emit('click', this.text); } }
}
</script>
<style scoped>
.nav-item { display: flex; flex-direction: column; align-items: center; transition: transform 0.1s; }
.nav-item:active { transform: scale(0.92); }
.nav-icon-wrap { width: 100rpx; height: 100rpx; border-radius: 50%; background-color: #eef2f7; display: flex; justify-content: center; align-items: center; margin-bottom: 12rpx; box-sizing: border-box; }
.nav-emoji { font-size: 48rpx; }
.nav-text { font-size: 26rpx; color: #1f1f1f; font-weight: 500; }
</style>
