<template>
  <view class="page">
    <view class="search-container">
      <view class="search-bar" @click="jumpToSearch">
        <text class="iconfont icon-sousuokuang search-icon"></text>
        <input class="search-input" type="text" placeholder="搜索Boss/武器/攻略..." readonly />
      </view>
    </view>

    <view class="swiper-wrapper">
      <swiper class="swiper" :indicator-dots="true" :autoplay="true" :interval="interval" :duration="duration" circular>
        <swiper-item v-for="(item, index) in imgList" :key="index">
          <image :src="item" mode="aspectFill" class="swiper-img"></image>
        </swiper-item>
      </swiper>
    </view>

    <view class="nav-container">
      <NavItem v-for="(item, index) in navList" :key="index" :text="item.text" :emoji="item.icon" @click="handleNavClick" />
    </view>

    <view class="section-container">
      <SectionHeader icon="⚔️" title="武器评测" @more-click="handleMoreClick" />
      <view class="card-list">
        <PostCard v-for="(item, index) in weaponList" :key="index" :item="item" @click="handleCardClick" />
      </view>
    </view>

    <view class="section-container">
      <SectionHeader icon="👹" title="Boss打法" @more-click="handleMoreClick" />
      <view class="card-list">
        <PostCard v-for="(item, index) in bossList" :key="index" :item="item" @click="handleCardClick" />
      </view>
    </view>
  </view>
</template>

<script>
import NavItem from '@/components/NavItem/NavItem.vue'
import SectionHeader from '@/components/SectionHeader/SectionHeader.vue'
import PostCard from '@/components/PostCard/PostCard.vue'

export default {
  components: { NavItem, SectionHeader, PostCard },
  data() {
    return {
      imgList: ['/static/banner1.png', '/static/banner2.png', '/static/banner3.png'],
      interval: 3000, duration: 500,
      navList: [
        { text: '武器', icon: '🗡️' }, { text: 'Boss', icon: '👹' },
        { text: '剧情', icon: '📜' }, { text: '地图', icon: '🗺️' }
      ],
      weaponList: [
        { id: 1, image: '/static/list6.jpg', title: '路德维希圣剑：全游戏最万金油的武器评测', author: 'Morris', avatar: '/static/avatar1.png', likes: 120 },
        { id: 2, image: '/static/list2.jpg', title: '锯肉刀：新手开局神器，变形斩教你做人', author: 'Candy', avatar: '/static/avatar2.png', likes: 369 }
      ],
      bossList: [
        { id: 3, image: '/static/list3.jpg', title: '路德维希：最崇高的怪物！二阶段必看反制秘籍', author: 'GameMaster', avatar: '/static/avatar1.png', likes: 88 },
        { id: 4, image: '/static/list4.jpg', title: '科斯的孤儿：硬核无伤速通路线分享', author: '萌新导师', avatar: '/static/avatar2.png', likes: 203 }
      ]
    }
  },
  methods: {
    jumpToSearch() { uni.navigateTo({ url: '/pages/search/search' }); },
    handleNavClick(name) { uni.navigateTo({ url: `/pages/category/category?type=${encodeURIComponent(name)}` }); },
    handleMoreClick(section) { uni.navigateTo({ url: `/pages/category/category?type=${encodeURIComponent(section)}` }); },
    handleCardClick(item) { uni.navigateTo({ url: `/pages/detail/detail?id=${item.id}&title=${encodeURIComponent(item.title)}` }); }
  }
}
</script>

<style>
/* 防止横向溢出和排版错乱 */
.page { display: flex; flex-direction: column; min-height: 100vh; background-color: #f5f7fa; padding-bottom: env(safe-area-inset-bottom); width: 100%; box-sizing: border-box; }
.search-container { padding: 20rpx 32rpx; background: transparent; width: 100%; box-sizing: border-box; }
.search-bar { display: flex; align-items: center; background: #ffffff; border-radius: 40rpx; padding: 12rpx 24rpx; box-shadow: 0 4rpx 16rpx rgba(0,0,0,0.03); box-sizing: border-box; width: 100%; }
.search-icon { margin-right: 12rpx; font-size: 36rpx; color: #a0a5b0; }
.search-input { flex: 1; font-size: 28rpx; color: #333; background: transparent; pointer-events: none; }
.swiper-wrapper { margin: 0 32rpx; border-radius: 20rpx; overflow: hidden; box-shadow: 0 4rpx 16rpx rgba(0,0,0,0.03); }
.swiper { width: 100%; height: 340rpx; }
.swiper-img { width: 100%; height: 100%; }
.nav-container { display: flex; justify-content: space-around; margin: 20rpx 32rpx; padding: 24rpx 0; background-color: #ffffff; border-radius: 20rpx; box-shadow: 0 4rpx 16rpx rgba(0,0,0,0.03); box-sizing: border-box; }
.section-container { background-color: #ffffff; padding: 24rpx 20rpx; margin: 0 32rpx 20rpx 32rpx; border-radius: 20rpx; box-shadow: 0 4rpx 16rpx rgba(0,0,0,0.03); box-sizing: border-box; }
.card-list { display: flex; justify-content: space-between; flex-wrap: wrap; width: 100%; box-sizing: border-box; }
</style>
