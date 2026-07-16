<template>
  <view class="page">
    <view class="post-container">
      <view class="post-header">
        <text class="post-title">{{ postTitle }}</text>
        <view class="author-info">
          <image class="avatar" src="/static/avatar1.png" mode="aspectFill"></image>
          <text class="author-name">资深猎人 · Morris</text>
        </view>
      </view>
      <view class="post-content">
        <image class="content-img" :src="currentPostData?.image || '/static/list6.jpg'" mode="widthFix"></image>
        <view class="content-text">
          <text>欢迎来到猎人工坊！「{{ postTitle }}」是血源诅咒中极具研究价值的内容。

作为一名亚楠猎人，掌握正确的战斗机制和应对策略是活下来的关键。本篇文章将带你深入解析该主题背后的核心玩法、伤害计算公式，以及实战中的变形斩、枪反应对技巧。</text>
        </view>

        <!-- 底部互动区：点赞 + 收藏 -->
        <view class="action-area">
          <view class="action-section" @click="toggleLike">
            <text class="action-icon">{{ isLiked ? '❤️' : '🤍' }}</text>
            <text class="action-text">{{ isLiked ? '已赞' : '点赞' }} {{ localLikeCount }}</text>
          </view>
          <view class="action-section" @click="toggleCollect">
            <text class="action-icon">{{ isCollected ? '⭐' : '☆' }}</text>
            <text class="action-text">{{ isCollected ? '已收藏' : '收藏' }}</text>
          </view>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      postId: '',
      postTitle: '加载中...',
      currentPostData: null,
      localLikeCount: 0,
      isLiked: false,
      isCollected: false
    }
  },
  onLoad(options) {
    if (options.id) {
      this.postId = Number(options.id);
      const allPosts = uni.getStorageSync('all_posts_data') || [];
      this.currentPostData = allPosts.find(p => p.id === this.postId) || null;
    }
    if (options.title) this.postTitle = decodeURIComponent(options.title);
    this.initStatus();
  },
  methods: {
    initStatus() {
      // 初始化点赞
      const likeKey = `post_${this.postId}`;
      const storedLike = uni.getStorageSync(likeKey);
      if (storedLike !== '' && storedLike !== undefined) {
        this.localLikeCount = storedLike;
      } else if (this.currentPostData) {
        this.localLikeCount = this.currentPostData.likes || 0;
      }
      const userLikeStatus = uni.getStorageSync(`user_like_${this.postId}`);
      this.isLiked = userLikeStatus === true;

      // 初始化收藏
      const collectedIds = uni.getStorageSync('collected_posts') || [];
      this.isCollected = collectedIds.includes(this.postId);
    },
    toggleLike() {
      const likeKey = `post_${this.postId}`;
      const userLikeKey = `user_like_${this.postId}`;
      if (this.isLiked) {
        this.localLikeCount--;
        uni.setStorageSync(likeKey, this.localLikeCount);
        uni.setStorageSync(userLikeKey, false);
        this.isLiked = false;
        uni.showToast({ title: '已取消点赞', icon: 'none' });
      } else {
        this.localLikeCount++;
        uni.setStorageSync(likeKey, this.localLikeCount);
        uni.setStorageSync(userLikeKey, true);
        this.isLiked = true;
        uni.showToast({ title: '点赞成功！', icon: 'success' });
      }
    },
    toggleCollect() {
      let collectedIds = uni.getStorageSync('collected_posts') || [];
      if (this.isCollected) {
        collectedIds = collectedIds.filter(id => id !== this.postId);
        uni.setStorageSync('collected_posts', collectedIds);
        this.isCollected = false;
        uni.showToast({ title: '已取消收藏', icon: 'none' });
      } else {
        collectedIds.push(this.postId);
        uni.setStorageSync('collected_posts', collectedIds);
        this.isCollected = true;
        uni.showToast({ title: '收藏成功！', icon: 'success' });
      }
    }
  }
}
</script>

<style>
.page { min-height: 100vh; background: #f5f7fa; width: 100%; box-sizing: border-box; }
.post-container { background: #fff; padding: 30rpx; margin: 20rpx 32rpx; border-radius: 20rpx; box-shadow: 0 4rpx 16rpx rgba(0,0,0,0.03); box-sizing: border-box; }
.post-header { border-bottom: 1rpx solid #f0f0f0; padding-bottom: 20rpx; margin-bottom: 20rpx; }
.post-title { font-size: 36rpx; font-weight: bold; color: #1f1f1f; margin-bottom: 20rpx; display: block; }
.author-info { display: flex; align-items: center; }
.avatar { width: 60rpx; height: 60rpx; border-radius: 50%; margin-right: 16rpx; }
.author-name { font-size: 26rpx; color: #666; }
.post-content { display: flex; flex-direction: column; gap: 20rpx; }
.content-img { width: 100%; border-radius: 8rpx; }
.content-text { font-size: 28rpx; color: #444; line-height: 1.6; }

/* 互动区 */
.action-area { display: flex; flex-direction: row; justify-content: flex-start; gap: 30rpx; margin-top: 20rpx; }
.action-section { display: flex; align-items: center; background: #f5f7fa; padding: 16rpx 32rpx; border-radius: 40rpx; transition: transform 0.1s; }
.action-section:active { transform: scale(0.95); }
.action-icon { font-size: 32rpx; margin-right: 10rpx; }
.action-text { font-size: 28rpx; color: #333; }
</style>
