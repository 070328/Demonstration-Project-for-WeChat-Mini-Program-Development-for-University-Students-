<template>
  <view class="page">
    <view class="login-card">
      <text class="title">欢迎归来，猎人</text>
      <text class="subtitle">登录以同步你的猎人工坊数据</text>

      <view class="input-group">
        <text class="label">姓名</text>
        <input class="input" v-model="name" type="text" placeholder="请输入姓名" />
      </view>

      <view class="input-group">
        <text class="label">学号</text>
        <input class="input" v-model="studentId" type="text" placeholder="请输入学号" />
      </view>

      <button class="login-btn" @click="handleLogin">登录 / 注册</button>
      <text class="tip"></text>

      <!-- 第三方登录区 -->
      <view class="third-party-area">
        <text class="divider-text">其他方式登录</text>
        <view class="social-icons">
          <!-- 微信图标 -->
          <view class="social-icon wechat" @click="handleThirdLogin('微信')"></view>
          <!-- QQ图标 -->
          <view class="social-icon qq" @click="handleThirdLogin('QQ')"></view>
          <!-- 新浪微博图标 -->
          <view class="social-icon weibo" @click="handleThirdLogin('微博')"></view>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      name: '111',
      studentId: 'www'
    }
  },
  methods: {
    handleLogin() {
      uni.setStorageSync('is_logged_in', true);
      const userInfo = {
        nickname: this.name || '111',
        studentId: this.studentId || 'www',
        following: 156,
        fans: 289
      };
      uni.setStorageSync('user_info', userInfo);
      uni.showToast({ title: '登录成功！', icon: 'success', duration: 1500 });
      setTimeout(() => {
        uni.switchTab({ url: '/pages/mine/mine' });
      }, 1500);
    },
    handleThirdLogin(platform) {
      uni.showToast({
        title: `进入 ${platform} 授权（演示功能）`,
        icon: 'none'
      });
    }
  }
}
</script>

<style>
.page { display: flex; justify-content: center; align-items: center; min-height: 100vh; background-color: #f5f7fa; padding: 40rpx; width: 100%; box-sizing: border-box; }
.login-card { width: 100%; max-width: 600rpx; background: #ffffff; border-radius: 30rpx; padding: 60rpx 40rpx; box-shadow: 0 8rpx 32rpx rgba(0,0,0,0.05); display: flex; flex-direction: column; align-items: center; box-sizing: border-box; }
.title { font-size: 40rpx; font-weight: bold; color: #1f1f1f; margin-bottom: 8rpx; }
.subtitle { font-size: 26rpx; color: #8c8f97; margin-bottom: 50rpx; }
.input-group { width: 100%; margin-bottom: 30rpx; }
.label { display: block; font-size: 26rpx; color: #555; margin-bottom: 10rpx; margin-left: 8rpx; }
.input { width: 100%; height: 80rpx; background: #f5f7fa; border-radius: 16rpx; padding: 0 24rpx; font-size: 28rpx; color: #333; box-sizing: border-box; }
.login-btn { width: 100%; height: 80rpx; background: #1f1f1f; color: #ffffff; border-radius: 40rpx; font-size: 30rpx; font-weight: bold; display: flex; align-items: center; justify-content: center; margin-top: 20rpx; transition: transform 0.1s; }
.login-btn:active { transform: scale(0.97); background: #333333; }
.tip { font-size: 22rpx; color: #999; margin-top: 30rpx; }

/* 第三方登录区 */
.third-party-area { width: 100%; margin-top: 40rpx; display: flex; flex-direction: column; align-items: center; }
.divider-text { font-size: 24rpx; color: #ccc; margin-bottom: 24rpx; position: relative; padding: 0 20rpx; background: #fff; }
.divider-text::before, .divider-text::after { content: ''; position: absolute; top: 50%; width: 80rpx; height: 2rpx; background: #eee; }
.divider-text::before { right: 100%; }
.divider-text::after { left: 100%; }

.social-icons { display: flex; gap: 40rpx; }
.social-icon { width: 90rpx; height: 90rpx; border-radius: 50%; transition: transform 0.1s; background-size: cover; background-position: center; }
.social-icon:active { transform: scale(0.9); }

/* 嵌入真实品牌图标 SVG */
.wechat { background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='%2307C160'%3E%3Cpath d='M8.5 13.5c-3.5 0-6.5-2.5-6.5-5.5S5 2.5 8.5 2.5s6.5 2.5 6.5 5.5-3 5.5-6.5 5.5zm7 1.5c-2.8 0-5-2-5-4.5s2.2-4.5 5-4.5 5 2 5 4.5-2.2 4.5-5 4.5z'/%3E%3C/svg%3E"); background-color: #f4f8f6; }
.qq { background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='%2312B7F5'%3E%3Cpath d='M12 2C9.2 2 7 4.2 7 7c0 2.8 2.2 5 5 5s5-2.2 5-5c0-2.8-2.2-5-5-5zm0 11.5c-3.8 0-6.9 3.1-6.9 6.9v2.5c0 .6.4 1.1 1 1.1h11.8c.6 0 1.1-.5 1.1-1.1v-2.5c0-3.8-3.1-6.9-7-6.9z'/%3E%3C/svg%3E"); background-color: #f3f8fc; }
.weibo { background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='%23FF8200'%3E%3Cpath d='M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm0 18c-4.42 0-8-3.58-8-8s3.58-8 8-8 8 3.58 8 8-3.58 8-8 8z'/%3E%3C/svg%3E"); background-color: #fdf6f0; }
</style>
