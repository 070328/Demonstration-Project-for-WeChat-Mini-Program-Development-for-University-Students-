<template>
  <view class="page">
    <view class="search-container">
      <view class="search-bar" @click="jumpToSearch">
        <text class="iconfont icon-sousuokuang search-icon"></text>
        <input class="search-input" type="text" :placeholder="`搜索${categoryType}内容`" readonly />
      </view>
    </view>

    <view class="header"><text class="title">「{{ categoryType }}」猎人工坊</text></view>
    <view class="card-list">
      <PostCard v-for="(item, index) in listData" :key="index" :item="item" @click="goDetail(item)" />
    </view>
  </view>
</template>

<script>
import PostCard from '@/components/PostCard/PostCard.vue'
export default {
  components: { PostCard },
  data() {
    return {
      categoryType: '综合',
      listData: []
    }
  },
  onLoad(options) {
    this.categoryType = options.type ? decodeURIComponent(options.type) : '综合';
    this.listData = [
      { id: 1, image: '/static/list6.jpg', title: `${this.categoryType} 之神器：无伤攻略`, author: 'Morris', avatar: '/static/avatar1.png', likes: 128 },
      { id: 2, image: '/static/list2.jpg', title: `${this.categoryType} 的战斗技巧与进阶思路`, author: 'Candy', avatar: '/static/avatar2.png', likes: 256 },
      { id: 3, image: '/static/list3.jpg', title: `深度解析 ${this.categoryType} 机制`, author: 'GameMaster', avatar: '/static/avatar1.png', likes: 88 },
    ];
  },
  methods: {
    jumpToSearch() { uni.navigateTo({ url: '/pages/search/search' }); },
    goDetail(item) { uni.navigateTo({ url: `/pages/detail/detail?id=${item.id}&title=${encodeURIComponent(item.title)}` }); }
  }
}
</script>

<style>
.page { min-height: 100vh; background: #f5f7fa; width: 100%; box-sizing: border-box; }
.search-container { padding: 20rpx 32rpx; background: transparent; width: 100%; box-sizing: border-box; }
.search-bar { display: flex; align-items: center; background: #ffffff; border-radius: 40rpx; padding: 12rpx 24rpx; box-shadow: 0 4rpx 16rpx rgba(0,0,0,0.03); box-sizing: border-box; }
.search-icon { margin-right: 12rpx; font-size: 36rpx; color: #a0a5b0; }
.search-input { flex: 1; font-size: 28rpx; color: #333; background: transparent; pointer-events: none; }
.header { background: #fff; border-radius: 20rpx; padding: 24rpx; text-align: center; margin: 0 32rpx 20rpx 32rpx; box-shadow: 0 4rpx 16rpx rgba(0,0,0,0.03); box-sizing: border-box; }
.title { font-size: 32rpx; font-weight: bold; color: #1f1f1f; }
.card-list { display: flex; justify-content: space-between; flex-wrap: wrap; padding: 0 32rpx; width: 100%; box-sizing: border-box; }
</style>
