<template>
  <view class="page">
    <!-- 已登录状态：展示个人资料 -->
    <view v-if="isLoggedIn" class="content-wrapper">
      <view class="profile-card">
        <view class="settings-btn" @click="goSettings"><text class="iconfont icon-settings">⚙️</text></view>
        <view class="avatar-wrapper">
          <image class="avatar" :src="userInfo.avatar" mode="aspectFill"></image>
          <view class="edit-badge" @click="editProfile"><text class="edit-icon">✏️</text></view>
        </view>
        <text class="nickname">{{ userInfo.nickname }}</text>
        <text class="signature">{{ userInfo.studentId || '暂无学号' }}</text>
        <view class="stats-row">
          <view class="stat-item" @click="goFollowList">
            <text class="stat-num">{{ userInfo.following }}</text>
            <text class="stat-label">关注</text>
          </view>
          <view class="divider"></view>
          <view class="stat-item" @click="goFansList">
            <text class="stat-num">{{ userInfo.fans }}</text>
            <text class="stat-label">粉丝</text>
          </view>
        </view>
      </view>

      <!-- 底部功能区列表 -->
      <view class="menu-list">
        <view class="menu-item" @click="goMenu('browse')">
          <view class="menu-left"><text class="menu-icon">📖</text><text class="menu-text">我的浏览</text></view>
          <text class="menu-arrow">></text>
        </view>

        <!-- 我的关注：显示数字 -->
        <view class="menu-item" @click="goMenu('follow')">
          <view class="menu-left"><text class="menu-icon">👤</text><text class="menu-text">我的关注</text></view>
          <view class="menu-right">
            <text class="menu-count">{{ userInfo.following }}</text>
            <text class="menu-arrow">></text>
          </view>
        </view>

        <!-- 我的粉丝：显示数字 -->
        <view class="menu-item" @click="goMenu('fans')">
          <view class="menu-left"><text class="menu-icon">👥</text><text class="menu-text">我的粉丝</text></view>
          <view class="menu-right">
            <text class="menu-count">{{ userInfo.fans }}</text>
            <text class="menu-arrow">></text>
          </view>
        </view>

        <view class="menu-item" @click="goMenu('collect')">
          <view class="menu-left"><text class="menu-icon">⭐</text><text class="menu-text">我的收藏</text></view>
          <text class="menu-arrow">></text>
        </view>

        <view class="menu-item" @click="goMenu('post')">
          <view class="menu-left"><text class="menu-icon">📝</text><text class="menu-text">我的发布</text></view>
          <text class="menu-arrow">></text>
        </view>
      </view>

      <!-- 退出登录按钮 -->
      <view class="logout-wrapper">
        <view class="logout-btn" @click="handleLogout">
          <text>退出登录</text>
        </view>
      </view>
    </view>

    <!-- 未登录状态：提示登录 -->
    <view v-else class="login-prompt">
      <text class="prompt-icon">🐺</text>
      <text class="prompt-text">您还未登录，请登录以解锁您的猎人档案</text>
      <button class="prompt-btn" @click="goLogin">去登录</button>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      isLoggedIn: false,
      userInfo: {
        avatar: '/static/list1.jpg',
        nickname: '学生',
        studentId: '加载中...',
        following: 0,
        fans: 0
      }
    }
  },
  onShow() {
    // 每次显示页面时检查登录状态并拉取数据
    this.isLoggedIn = uni.getStorageSync('is_logged_in') || false;
    if (this.isLoggedIn) {
      const storedInfo = uni.getStorageSync('user_info');
      if (storedInfo) {
        this.userInfo = {
          ...this.userInfo,
          nickname: storedInfo.nickname,
          studentId: storedInfo.studentId,
          following: storedInfo.following,
          fans: storedInfo.fans
        };
      }
    }
  },
  methods: {
    goSettings() { uni.showToast({ title: '进入设置', icon: 'none' }); },
    editProfile() { uni.showToast({ title: '编辑个人资料', icon: 'none' }); },
    goFollowList() {
      // 点击上方的关注数字，也跳转到列表页
      uni.navigateTo({ url: '/pages/userlist/userlist?type=follow' });
    },
    goFansList() {
      // 点击上方的粉丝数字，也跳转到列表页
      uni.navigateTo({ url: '/pages/userlist/userlist?type=fans' });
    },
    goMenu(type) {
      // 如果在底部列表点击了关注或粉丝，跳转到用户列表页
      if (type === 'follow' || type === 'fans') {
        uni.navigateTo({ url: `/pages/userlist/userlist?type=${type}` });
        return;
      }

      let map = {
        browse: '我的浏览',
        collect: '我的收藏',
        post: '我的发布'
      };
      uni.navigateTo({ url: `/pages/category/category?type=${encodeURIComponent(map[type])}` });
    },
    goLogin() { uni.navigateTo({ url: '/pages/login/login' }); },

    handleLogout() {
      uni.showModal({
        title: '提示',
        content: '确定要退出登录吗？',
        success: (res) => {
          if (res.confirm) {
            uni.removeStorageSync('is_logged_in');
            uni.removeStorageSync('user_info');
            this.isLoggedIn = false;
            uni.showToast({ title: '已安全退出', icon: 'success' });
          }
        }
      });
    }
  }
}
</script>

<style>
.page { display: flex; flex-direction: column; align-items: center; min-height: 100vh; background-color: #f5f7fa; padding-top: 30rpx; padding-bottom: env(safe-area-inset-bottom); width: 100%; box-sizing: border-box; }
.content-wrapper { width: 100%; display: flex; flex-direction: column; align-items: center; }
.profile-card { position: relative; width: 92%; background-color: #ffffff; border-radius: 24rpx; display: flex; flex-direction: column; align-items: center; padding: 40rpx 0; margin-bottom: 30rpx; box-shadow: 0 4rpx 16rpx rgba(0,0,0,0.03); box-sizing: border-box; }
.settings-btn { position: absolute; top: 24rpx; right: 24rpx; padding: 10rpx; }
.settings-btn text { font-size: 36rpx; color: #999; }
.avatar-wrapper { position: relative; margin-bottom: 16rpx; }
.avatar { width: 140rpx; height: 140rpx; border-radius: 50%; border: 4rpx solid #f0f0f0; background-color: #f2f2f2; }
.edit-badge { position: absolute; bottom: 4rpx; right: -4rpx; width: 44rpx; height: 44rpx; background-color: #c0c4cc; border-radius: 50%; display: flex; justify-content: center; align-items: center; border: 2rpx solid #fff; }
.edit-icon { font-size: 24rpx; color: #fff; }
.nickname { font-size: 34rpx; font-weight: bold; color: #1f1f1f; margin-bottom: 8rpx; }
.signature { font-size: 26rpx; color: #8c8f97; margin-bottom: 24rpx; }
.stats-row { display: flex; align-items: center; justify-content: center; }
.stat-item { display: flex; flex-direction: row; align-items: center; padding: 0 20rpx; }
.stat-num { font-size: 28rpx; font-weight: bold; color: #444; margin-right: 8rpx; }
.stat-label { font-size: 26rpx; color: #999; }
.divider { width: 2rpx; height: 24rpx; background-color: #ddd; }
.menu-list { width: 92%; background-color: #ffffff; border-radius: 24rpx; overflow: hidden; box-shadow: 0 4rpx 16rpx rgba(0,0,0,0.03); margin-bottom: 20rpx; box-sizing: border-box; }
.menu-item { display: flex; align-items: center; justify-content: space-between; padding: 28rpx 30rpx; border-bottom: 1rpx solid #f0f0f0; transition: background 0.1s; box-sizing: border-box; }
.menu-item:last-child { border-bottom: none; }
.menu-item:active { background-color: #f8f8f8; }
.menu-left { display: flex; align-items: center; }
.menu-icon { font-size: 32rpx; margin-right: 20rpx; color: #555; width: 40rpx; text-align: center; }
.menu-text { font-size: 30rpx; color: #1f1f1f; }

/* 右侧布局调整，让数字和箭头在一起 */
.menu-right { display: flex; align-items: center; }
.menu-count { font-size: 28rpx; color: #999; margin-right: 10rpx; font-weight: bold; }
.menu-arrow { font-size: 28rpx; color: #ccc; font-family: monospace; }

.logout-wrapper { width: 92%; margin-bottom: 40rpx; }
.logout-btn { width: 100%; background-color: #ffffff; border-radius: 24rpx; padding: 28rpx 0; text-align: center; box-shadow: 0 4rpx 16rpx rgba(0,0,0,0.03); box-sizing: border-box; transition: background 0.1s; }
.logout-btn:active { background-color: #f8f8f8; }
.logout-btn text { font-size: 30rpx; color: #444444; font-weight: 500; }

.login-prompt { display: flex; flex-direction: column; align-items: center; justify-content: center; flex: 1; padding: 100rpx; width: 100%; box-sizing: border-box; }
.prompt-icon { font-size: 100rpx; margin-bottom: 30rpx; color: #ccc; }
.prompt-text { font-size: 32rpx; color: #666; text-align: center; margin-bottom: 40rpx; }
.prompt-btn { width: 300rpx; height: 80rpx; background: #1f1f1f; color: #ffffff; border-radius: 40rpx; font-size: 30rpx; font-weight: bold; display: flex; align-items: center; justify-content: center; }
.prompt-btn:active { transform: scale(0.95); background: #333333; }
</style>
