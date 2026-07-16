<template>
  <view class="card-item">
    <!-- 点击卡片主体进入详情 -->
    <view class="card-body" @click="handleClick">
      <image class="card-cover" :src="item.image" mode="aspectFill"></image>
      <view class="card-info">
        <text class="card-title">{{ item.title }}</text>
        <view class="card-bottom">
          <view class="author">
            <image class="avatar" :src="item.avatar" mode="aspectFill"></image>
            <text class="name">{{ item.author }}</text>
          </view>
        </view>
      </view>
    </view>

    <!-- 底部交互栏：点赞与收藏 -->
    <view class="action-bar">
      <view class="action-item" @click.stop="toggleLike">
        <text class="action-icon">{{ isLiked ? '❤️' : '🤍' }}</text>
        <text class="action-text">{{ getLikesCount(item.id) }}</text>
      </view>
      <view class="action-item" @click.stop="toggleCollect">
        <text class="action-icon">{{ isCollected ? '⭐' : '☆' }}</text>
        <text class="action-text">{{ isCollected ? '已收藏' : '收藏' }}</text>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  name: 'PostCard',
  props: { item: { type: Object, default: () => ({}) } },
  data() {
    return {
      isLiked: false,
      isCollected: false
    }
  },
  mounted() {
    this.initStatus();
  },
  methods: {
    handleClick() {
      this.$emit('click', this.item);
    },
    initStatus() {
      // 初始化点赞状态
      const likeKey = `post_${this.item.id}`;
      const storedLike = uni.getStorageSync(likeKey);
      if (storedLike !== '' && storedLike !== undefined) {
        this.isLiked = true;
      }

      // 初始化收藏状态
      const collectedIds = uni.getStorageSync('collected_posts') || [];
      this.isCollected = collectedIds.includes(this.item.id);
    },
    getLikesCount(id) {
      const key = `post_${id}`;
      const storedCount = uni.getStorageSync(key);
      return storedCount !== '' && storedCount !== undefined ? storedCount : this.item.likes;
    },
    toggleLike() {
      const likeKey = `post_${this.item.id}`;
      const userLikeKey = `user_like_${this.item.id}`;
      const currentCount = this.getLikesCount(this.item.id);

      if (this.isLiked) {
        uni.setStorageSync(likeKey, currentCount - 1);
        uni.setStorageSync(userLikeKey, false);
        this.isLiked = false;
        uni.showToast({ title: '已取消点赞', icon: 'none' });
      } else {
        uni.setStorageSync(likeKey, currentCount + 1);
        uni.setStorageSync(userLikeKey, true);
        this.isLiked = true;
        uni.showToast({ title: '点赞成功！', icon: 'success' });
      }
    },
    toggleCollect() {
      let collectedIds = uni.getStorageSync('collected_posts') || [];
      if (this.isCollected) {
        // 取消收藏
        collectedIds = collectedIds.filter(id => id !== this.item.id);
        uni.setStorageSync('collected_posts', collectedIds);
        this.isCollected = false;
        uni.showToast({ title: '已取消收藏', icon: 'none' });
      } else {
        // 添加收藏
        collectedIds.push(this.item.id);
        uni.setStorageSync('collected_posts', collectedIds);
        this.isCollected = true;
        uni.showToast({ title: '收藏成功！', icon: 'success' });
      }
    }
  }
}
</script>

<style scoped>
.card-item { width: 48%; background: #fff; border-radius: 12rpx; overflow: hidden; margin-bottom: 20rpx; box-shadow: 0 4rpx 16rpx rgba(0,0,0,0.03); box-sizing: border-box; }
.card-body:active { opacity: 0.8; }
.card-cover { width: 100%; height: 220rpx; display: block; background-color: #eee; }
.card-info { padding: 12rpx; box-sizing: border-box; }
.card-title { display: -webkit-box; -webkit-box-orient: vertical; -webkit-line-clamp: 2; overflow: hidden; font-size: 28rpx; font-weight: 500; line-height: 1.4; margin-bottom: 16rpx; color: #1f1f1f; }
.card-bottom { display: flex; justify-content: space-between; align-items: center; }
.author { display: flex; align-items: center; }
.avatar { width: 40rpx; height: 40rpx; border-radius: 50%; margin-right: 8rpx; }
.name { font-size: 24rpx; color: #8c8f97; max-width: 80rpx; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }

/* 新增底部交互栏 */
.action-bar { display: flex; justify-content: space-around; padding: 12rpx 0; border-top: 1rpx solid #f5f7fa; background: #fafbfc; }
.action-item { display: flex; align-items: center; justify-content: center; padding: 4rpx 16rpx; border-radius: 30rpx; transition: background 0.1s; }
.action-item:active { background: #f0f0f0; }
.action-icon { font-size: 28rpx; margin-right: 6rpx; }
.action-text { font-size: 24rpx; color: #666; }
</style>
