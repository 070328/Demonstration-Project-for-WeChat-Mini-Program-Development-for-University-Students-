<template>
  <view class="page">
    <view class="header-section">
      <text class="list-title">{{ pageTitle }} ({{ listData.length }})</text>
    </view>

    <view class="list-container">
      <view class="user-item" v-for="(user, index) in listData" :key="index">
        <image class="user-avatar" :src="user.avatar" mode="aspectFill"></image>
        <view class="user-info">
          <text class="user-name">{{ user.nickname }}</text>
          <text class="user-sig">{{ user.signature }}</text>
        </view>
        <!-- 模拟的互关/关注状态标签 -->
        <view class="status-tag" :class="user.isMutual ? 'mutual' : 'follow'">
          <text>{{ user.isMutual ? '互相关注' : '已关注' }}</text>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      pageTitle: '列表',
      listType: '', // 用于区分是 'follow' 还是 'fans'
      listData: []
    }
  },
  onLoad(options) {
    this.listType = options.type || 'follow';
    this.pageTitle = this.listType === 'follow' ? '我的关注' : '我的粉丝';

    // 模拟生成关注/粉丝的数据（你可以把头像换成你项目里的本地图片）
    const mockUsers = [
      { id: 1, avatar: '/static/avatar1.png', nickname: '亚楠老猎人', signature: '枪反是本能，莽就完了', isMutual: true },
      { id: 2, avatar: '/static/avatar2.png', nickname: '圣剑路德维希', signature: '愿月光指引你', isMutual: false },
      { id: 3, avatar: '/static/list1.jpg', nickname: '该隐赫斯特贵族', signature: '血质才是力量的源头', isMutual: true },
      { id: 4, avatar: '/static/list2.jpg', nickname: '治愈教会修女', signature: '愿古神庇佑你', isMutual: false }
    ];
    this.listData = mockUsers;
  }
}
</script>

<style>
.page { min-height: 100vh; background-color: #f5f7fa; padding: 30rpx 32rpx; box-sizing: border-box; }
.header-section { margin-bottom: 30rpx; }
.list-title { font-size: 34rpx; font-weight: bold; color: #1f1f1f; }

.list-container { background: #ffffff; border-radius: 24rpx; box-shadow: 0 4rpx 16rpx rgba(0,0,0,0.03); overflow: hidden; }
.user-item { display: flex; align-items: center; padding: 24rpx 30rpx; border-bottom: 1rpx solid #f0f0f0; transition: background 0.1s; }
.user-item:last-child { border-bottom: none; }
.user-item:active { background-color: #f8f8f8; }

.user-avatar { width: 90rpx; height: 90rpx; border-radius: 50%; background: #eee; margin-right: 20rpx; flex-shrink: 0; }
.user-info { flex: 1; display: flex; flex-direction: column; justify-content: center; }
.user-name { font-size: 30rpx; font-weight: bold; color: #1f1f1f; margin-bottom: 6rpx; }
.user-sig { font-size: 24rpx; color: #8c8f97; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; max-width: 300rpx; }

.status-tag { padding: 6rpx 20rpx; border-radius: 30rpx; font-size: 24rpx; }
.status-tag.mutual { background: #eef2f7; color: #555; }
.status-tag.follow { background: #f5f7fa; color: #999; }
</style>
