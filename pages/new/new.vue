<template>
  <view class="page">
    <view v-if="isLoggedIn" class="notice-container">
      <view class="date-group"><text class="date-title">刚刚</text></view>
      <view class="notice-item" v-for="(item, index) in todayList" :key="index" @click="handleNoticeClick(item)">
        <image class="avatar" :src="item.avatar" mode="aspectFill"></image>
        <view class="content">
          <view class="content-top">
            <text class="title">{{ item.title }}</text>
            <text class="time">{{ item.time }}</text>
          </view>
          <text class="desc">{{ item.desc }}</text>
        </view>
        <view class="status-badge" v-if="!item.isRead"><text class="badge-text">新</text></view>
      </view>

      <view class="date-group"><text class="date-title">今天</text></view>
      <view class="notice-item" v-for="(item, index) in weekList" :key="index" @click="handleNoticeClick(item)">
        <image class="avatar" :src="item.avatar" mode="aspectFill"></image>
        <view class="content">
          <view class="content-top">
            <text class="title">{{ item.title }}</text>
            <text class="time">{{ item.time }}</text>
          </view>
          <text class="desc">{{ item.desc }}</text>
        </view>
        <view class="status-badge" v-if="!item.isRead"><text class="badge-text">新</text></view>
      </view>
    </view>

    <view v-else class="login-prompt">
      <text class="prompt-icon">🔒</text>
      <text class="prompt-text">您还未登录，请先登录以查看最新通知</text>
      <button class="prompt-btn" @click="goLogin">去登录</button>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      isLoggedIn: false,
      todayList: [
        { id: 1, avatar: '/static/avatar1.png', title: '科斯孤儿 发布了新打法视频', desc: '《路德维希：最伟大的怪物》', time: '3分钟前', isRead: false },
        { id: 2, avatar: '/static/avatar2.png', title: '治愈教会 回复了你', desc: '“试试用锯肉刀在第二阶段绕后！”', time: '2小时前', isRead: false }
      ],
      weekList: [
        { id: 3, avatar: '/static/avatar1.png', title: '猎人工坊 发布了新帖子', desc: '《圣杯地牢深度铭文分享》', time: '2天前', isRead: true },
        { id: 4, avatar: '/static/avatar2.png', title: 'Doll 关注了你', desc: '“晚安，善良的猎人。”', time: '3天前', isRead: true }
      ]
    }
  },
  onShow() { this.isLoggedIn = uni.getStorageSync('is_logged_in') || false; },
  methods: {
    handleNoticeClick(item) { uni.navigateTo({ url: `/pages/detail/detail?id=${item.id}&title=${encodeURIComponent(item.title)}` }); },
    goLogin() { uni.navigateTo({ url: '/pages/login/login' }); }
  }
}
</script>

<style>
.page { display: flex; flex-direction: column; min-height: 100vh; background-color: #f5f7fa; padding-bottom: env(safe-area-inset-bottom); width: 100%; box-sizing: border-box; }
.notice-container { display: flex; flex-direction: column; padding: 10rpx 0 30rpx; width: 100%; box-sizing: border-box; }
.date-group { padding: 12rpx 32rpx 8rpx; }
.date-title { font-size: 24rpx; color: #8c8f97; font-weight: 500; }
.notice-item { display: flex; align-items: flex-start; background-color: #ffffff; padding: 24rpx 32rpx; border-bottom: 1rpx solid #f0f0f0; transition: background 0.1s; box-sizing: border-box; width: 100%; }
.notice-item:active { background-color: #f9fafb; }
.notice-item:last-child { border-bottom: none; }
.avatar { width: 80rpx; height: 80rpx; border-radius: 50%; background-color: #eee; flex-shrink: 0; margin-right: 20rpx; }
.content { flex: 1; display: flex; flex-direction: column; justify-content: center; }
.content-top { display: flex; justify-content: space-between; align-items: baseline; margin-bottom: 6rpx; }
.title { font-size: 28rpx; font-weight: bold; color: #1f1f1f; }
.time { font-size: 22rpx; color: #999; }
.desc { font-size: 26rpx; color: #666; line-height: 1.4; display: -webkit-box; -webkit-box-orient: vertical; -webkit-line-clamp: 2; overflow: hidden; }
.status-badge { margin-left: 12rpx; min-width: 32rpx; height: 32rpx; background-color: #56b4ef; border-radius: 16rpx; display: flex; align-items: center; justify-content: center; align-self: center; }
.badge-text { font-size: 20rpx; color: #ffffff; padding: 0 8rpx; font-weight: bold; }
.login-prompt { display: flex; flex-direction: column; align-items: center; justify-content: center; flex: 1; padding: 100rpx; width: 100%; box-sizing: border-box; }
.prompt-icon { font-size: 100rpx; margin-bottom: 30rpx; color: #ccc; }
.prompt-text { font-size: 32rpx; color: #666; text-align: center; margin-bottom: 40rpx; }
.prompt-btn { width: 300rpx; height: 80rpx; background: #1f1f1f; color: #ffffff; border-radius: 40rpx; font-size: 30rpx; font-weight: bold; display: flex; align-items: center; justify-content: center; }
.prompt-btn:active { transform: scale(0.95); background: #333333; }
</style>
