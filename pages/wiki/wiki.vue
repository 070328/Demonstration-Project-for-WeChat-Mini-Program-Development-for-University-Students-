<template>
  <view class="page">
    <view class="search-container">
      <view class="search-bar" @click="jumpToSearch">
        <text class="iconfont icon-sousuokuang search-icon"></text>
        <input class="search-input" type="text" placeholder="搜索武器、Boss、攻略..." readonly />
      </view>
    </view>

    <view class="banner-section">
      <view class="banner-content">
        <text class="banner-title">《血源诅咒》猎人工坊</text>
        <text class="banner-subtitle">探索亚楠的黑暗传说，掌握古神与猎人的秘辛</text>
      </view>
    </view>

    <view class="category-grid">
      <view class="cat-item" v-for="(cat, index) in categories" :key="index" @click="goCategory(cat.name)">
        <view class="cat-icon-wrap"><text class="cat-emoji">{{ cat.icon }}</text></view>
        <text class="cat-name">{{ cat.name }}</text>
        <text class="cat-desc">{{ cat.desc }}</text>
      </view>
    </view>

    <view class="section-container">
      <view class="section-header"><text class="section-title">📜 堕落之血：剧情深度解析</text><text class="more" @click="goWikiList('剧情')">更多 ></text></view>
      <view class="article-list">
        <view class="article-item" v-for="(item, index) in loreList" :key="index" @click="goArticle(item)">
          <image class="article-img" :src="item.image" mode="aspectFill"></image>
          <view class="article-info">
            <text class="article-title">{{ item.title }}</text>
            <text class="article-desc">{{ item.desc }}</text>
            <view class="article-bottom">
              <text class="article-views">👁️ {{ item.views }} 浏览</text>
              <text class="article-tag" :style="{backgroundColor: item.tagColor}">{{ item.tag }}</text>
            </view>
          </view>
        </view>
      </view>
    </view>

    <view class="section-container">
      <view class="section-header"><text class="section-title">🛡️ 猎人必修：生存与输出</text><text class="more" @click="goWikiList('技巧')">更多 ></text></view>
      <view class="article-list">
        <view class="article-item" v-for="(item, index) in tipsList" :key="index" @click="goArticle(item)">
          <image class="article-img" :src="item.image" mode="aspectFill"></image>
          <view class="article-info">
            <text class="article-title">{{ item.title }}</text>
            <text class="article-desc">{{ item.desc }}</text>
            <view class="article-bottom">
              <text class="article-views">👁️ {{ item.views }} 浏览</text>
              <text class="article-tag" :style="{backgroundColor: item.tagColor}">{{ item.tag }}</text>
            </view>
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
      categories: [
        { name: 'Boss', icon: '👹', desc: '速杀技巧与图鉴' },
        { name: '武器', icon: '🗡️', desc: '面板数值与搭配' },
        { name: '圣杯地牢', icon: '🏛️', desc: '迷宫探索与铭文' },
        { name: '符文', icon: '💎', desc: '猎人印记与配装' }
      ],
      loreList: [
        { id: 1, title: '治愈教会的崛起与堕落', desc: '治愈教会如何发现古老之血，并最终导致亚楠被野兽入侵...', image: '/static/list6.jpg', views: '2.4w', tag: '深度解析', tagColor: '#e5e5e5' },
        { id: 2, title: '科斯孤儿与梦境的诅咒', desc: '来自海洋的古老神祇“科斯”是如何催生了整个猎人的梦境...', image: '/static/list2.jpg', views: '1.8w', tag: '核心剧情', tagColor: '#e5e5e5' },
        { id: 3, title: '古神：月之魔物', desc: '隐藏在猎人梦境之后的最终操纵者，月亮的存在到底是什么...', image: '/static/list5.jpg', views: '9.5k', tag: '背景设定', tagColor: '#e5e5e5' }
      ],
      tipsList: [
        { id: 4, title: '枪反的绝对奥义：从入门到精通', desc: '掌握枪反与内脏暴击的帧率机制，任何Boss都将成为你的玩具...', image: '/static/list3.jpg', views: '5.6k', tag: '硬核教学', tagColor: '#f0f8ff' },
        { id: 5, title: '全稀有宝石速刷路线分享', desc: '分享几套刷取物理/血质/火焰宝石的必备铭文，提升武器上限...', image: '/static/list4.jpg', views: '3.2k', tag: '刷子攻略', tagColor: '#f0f8ff' }
      ]
    }
  },
  methods: {
    jumpToSearch() { uni.navigateTo({ url: '/pages/search/search' }); },
    goCategory(name) { uni.navigateTo({ url: `/pages/category/category?type=${encodeURIComponent(name)}` }); },
    goWikiList(type) { uni.navigateTo({ url: `/pages/category/category?type=${encodeURIComponent(type)}` }); },
    goArticle(item) { uni.navigateTo({ url: `/pages/detail/detail?id=${item.id}&title=${encodeURIComponent(item.title)}` }); }
  }
}
</script>

<style>
.page { display: flex; flex-direction: column; min-height: 100vh; background-color: #f5f7fa; padding-bottom: env(safe-area-inset-bottom); width: 100%; box-sizing: border-box; }
.search-container { padding: 20rpx 32rpx; background: transparent; width: 100%; box-sizing: border-box; }
.search-bar { display: flex; align-items: center; background: #ffffff; border-radius: 40rpx; padding: 12rpx 24rpx; box-shadow: 0 4rpx 16rpx rgba(0,0,0,0.03); box-sizing: border-box; }
.search-icon { margin-right: 12rpx; font-size: 36rpx; color: #a0a5b0; }
.search-input { flex: 1; font-size: 28rpx; color: #333; background: transparent; pointer-events: none; }
.banner-section { margin: 0 32rpx 20rpx; padding: 40rpx 30rpx; background: #ffffff; border-radius: 20rpx; box-shadow: 0 4rpx 16rpx rgba(0,0,0,0.03); box-sizing: border-box; }
.banner-title { font-size: 34rpx; font-weight: bold; color: #1f1f1f; display: block; margin-bottom: 10rpx; }
.banner-subtitle { font-size: 26rpx; color: #8c8f97; }
.category-grid { display: flex; justify-content: space-between; margin: 0 32rpx 20rpx; width: calc(100% - 64rpx); box-sizing: border-box; }
.cat-item { display: flex; flex-direction: column; align-items: center; background: #ffffff; padding: 24rpx 12rpx; width: 23%; border-radius: 20rpx; box-shadow: 0 4rpx 16rpx rgba(0,0,0,0.03); transition: transform 0.1s; box-sizing: border-box; }
.cat-item:active { transform: scale(0.95); }
.cat-icon-wrap { width: 80rpx; height: 80rpx; background: #eef2f7; border-radius: 50%; display: flex; align-items: center; justify-content: center; margin-bottom: 10rpx; flex-shrink: 0; }
.cat-emoji { font-size: 40rpx; }
.cat-name { font-size: 28rpx; font-weight: bold; color: #1f1f1f; }
.cat-desc { font-size: 20rpx; color: #8c8f97; margin-top: 4rpx; }
.section-container { background: #ffffff; margin: 0 32rpx 20rpx; border-radius: 20rpx; padding: 24rpx 20rpx; box-shadow: 0 4rpx 16rpx rgba(0,0,0,0.03); box-sizing: border-box; }
.section-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 20rpx; }
.section-title { font-size: 32rpx; font-weight: bold; color: #1f1f1f; }
.more { font-size: 26rpx; color: #8c8f97; }
.article-list { display: flex; flex-direction: column; gap: 20rpx; }
.article-item { display: flex; background: #f9fafb; border-radius: 16rpx; padding: 16rpx; align-items: center; transition: transform 0.1s; box-sizing: border-box; }
.article-item:active { transform: scale(0.98); }
.article-img { width: 160rpx; height: 120rpx; border-radius: 12rpx; background: #eee; flex-shrink: 0; }
.article-info { flex: 1; margin-left: 16rpx; display: flex; flex-direction: column; justify-content: space-between; height: 120rpx; }
.article-title { font-size: 28rpx; font-weight: bold; color: #1f1f1f; }
.article-desc { font-size: 24rpx; color: #666; display: -webkit-box; -webkit-box-orient: vertical; -webkit-line-clamp: 2; overflow: hidden; line-height: 1.4; }
.article-bottom { display: flex; justify-content: space-between; align-items: center; }
.article-views { font-size: 22rpx; color: #999; }
.article-tag { font-size: 22rpx; padding: 4rpx 16rpx; border-radius: 12rpx; color: #333; }
</style>
