<template>
  <view class="page">
    <view class="search-header">
      <view class="search-bar">
        <text class="iconfont icon-sousuokuang search-icon"></text>
        <input class="search-input" v-model="keyword" :focus="true" placeholder="搜索Boss/武器/攻略" confirm-type="search" @confirm="doSearch" />
        <text v-if="keyword" class="iconfont icon-clear clear-icon" @click="clearInput"></text>
      </view>
      <text class="cancel-btn" @click="goBack">取消</text>
    </view>

    <view v-if="!hasSearched" class="search-panel">
      <view class="history-section" v-if="historyList.length">
        <view class="section-header">
          <text class="section-title">搜索历史</text>
          <text class="clear-history" @click="clearHistory">清除</text>
        </view>
        <view class="tag-list flex-wrap">
          <text class="tag" v-for="(item, index) in historyList" :key="index" @click="quickSearch(item)">{{ item }}</text>
        </view>
      </view>

      <view class="hot-section">
        <view class="section-header">
          <text class="section-title">热门搜索</text>
        </view>
        <view class="hot-tags-wrapper">
          <text class="hot-tag" v-for="(item, index) in hotList" :key="index" @click="quickSearch(item)">{{ item }}</text>
        </view>
      </view>

      <view class="category-section">
        <view class="section-header">
          <text class="section-title">搜索分类</text>
        </view>
        <view class="category-grid">
          <view class="category-item" v-for="(item, index) in categoryList" :key="index" @click="quickSearch(item.name)">
            <view class="circle-icon"><text class="category-icon-text">{{ item.icon }}</text></view>
            <text class="category-name">{{ item.name }}</text>
          </view>
        </view>
      </view>

      <view class="more-btn-wrapper">
        <view class="more-btn" @click="loadMore">更多</view>
      </view>
    </view>

    <view v-else class="result-list">
      <view v-if="resultList.length === 0" class="empty-tip">暂无相关结果</view>
      <view class="result-item" v-for="(item, index) in resultList" :key="index" @click="goDetail(item)">
        <text class="item-title">{{ item.title }}</text>
        <text class="item-desc">{{ item.desc }}</text>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      keyword: '',
      historyList: [],
      hotList: ['路德维希', '科斯孤儿', '枪反技巧', '地牢铭文', '血质宝石'],
      categoryList: [
        { name: '武器', icon: '🗡️' }, { name: 'Boss', icon: '👹' },
        { name: '血质', icon: '💧' }, { name: '剧情', icon: '📜' }
      ],
      resultList: [],
      hasSearched: false
    }
  },
  onLoad(options) {
    if (options.keyword) {
      this.keyword = decodeURIComponent(options.keyword);
      this.doSearch();
    }
    this.historyList = uni.getStorageSync('searchHistory') || [];
  },
  methods: {
    doSearch() {
      const kw = this.keyword.trim();
      if (!kw) return uni.showToast({ title: '请输入关键词', icon: 'none' });
      this.hasSearched = true;
      this.resultList = this.mockSearchResult(kw);
      this.saveHistory(kw);
    },
    mockSearchResult(keyword) {
      if (keyword.includes('武器')) return [{ title: '路德维希圣剑', desc: '面板与属性分析' }, { title: '锯肉刀变形斩', desc: '如何打出最高伤害' }];
      if (keyword.includes('Boss')) return [{ title: '科斯的孤儿', desc: '无伤逃课打法' }, { title: '路德维希二阶段', desc: '避雷指南' }];
      return [{ title: `${keyword} 攻略合集`, desc: '包含剧情、收集与Boss打法...' }, { title: `${keyword} 进阶技巧`, desc: '高手才知道的隐藏机制' }];
    },
    saveHistory(keyword) {
      let history = uni.getStorageSync('searchHistory') || [];
      history = history.filter(item => item !== keyword);
      history.unshift(keyword);
      if (history.length > 10) history.pop();
      uni.setStorageSync('searchHistory', history);
      this.historyList = history;
    },
    clearInput() { this.keyword = ''; this.hasSearched = false; this.resultList = []; },
    quickSearch(tag) { this.keyword = tag; this.doSearch(); },
    clearHistory() { uni.removeStorageSync('searchHistory'); this.historyList = []; },
    goBack() { uni.navigateBack(); },
    goDetail(item) { uni.showToast({ title: `查看：${item.title}`, icon: 'none' }); },
    loadMore() { uni.showToast({ title: '加载更多内容', icon: 'none' }); }
  }
}
</script>

<style>
.page { display: flex; flex-direction: column; min-height: 100vh; background-color: #f5f7fa; padding-bottom: env(safe-area-inset-bottom); width: 100%; box-sizing: border-box; }
.search-header { display: flex; align-items: center; padding: 20rpx 30rpx; background-color: #ffffff; border-bottom: 1rpx solid #eeeeee; box-sizing: border-box; }
.search-bar { flex: 1; display: flex; align-items: center; background: #f2f2f2; border-radius: 40rpx; padding: 12rpx 24rpx; box-sizing: border-box; }
.search-icon { font-size: 36rpx; color: #999999; margin-right: 12rpx; }
.search-input { flex: 1; font-size: 28rpx; color: #333333; background: transparent; border: none; outline: none; }
.clear-icon { font-size: 32rpx; color: #cccccc; margin-left: 10rpx; padding: 4rpx; }
.cancel-btn { margin-left: 20rpx; font-size: 30rpx; color: #333333; }
.search-panel { padding: 30rpx 32rpx; box-sizing: border-box; }
.section-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 20rpx; margin-top: 20rpx; }
.section-title { font-size: 32rpx; font-weight: bold; color: #333333; }
.clear-history { font-size: 26rpx; color: #999999; }
.flex-wrap { display: flex; flex-wrap: wrap; gap: 20rpx; }
.tag { background: #f0f0f0; padding: 10rpx 28rpx; border-radius: 30rpx; font-size: 26rpx; color: #333333; }
.hot-tags-wrapper { background: #f0f0f0; border-radius: 30rpx; padding: 20rpx 24rpx; display: flex; flex-wrap: wrap; gap: 20rpx; }
.hot-tag { background: #e5e5e5; padding: 10rpx 28rpx; border-radius: 30rpx; font-size: 26rpx; color: #333333; }
.category-section { margin-top: 20rpx; }
.category-grid { display: flex; justify-content: space-around; padding: 10rpx 0; }
.category-item { display: flex; flex-direction: column; align-items: center; }
.circle-icon { width: 100rpx; height: 100rpx; background: #f0f0f0; border-radius: 50%; display: flex; justify-content: center; align-items: center; margin-bottom: 12rpx; }
.category-icon-text { font-size: 48rpx; }
.category-name { font-size: 28rpx; color: #333333; }
.more-btn-wrapper { display: flex; justify-content: center; margin-top: 40rpx; margin-bottom: 60rpx; }
.more-btn { background: #e5e5e5; padding: 12rpx 60rpx; border-radius: 40rpx; font-size: 28rpx; color: #444444; }
.result-list { flex: 1; padding: 0 30rpx; background-color: #ffffff; box-sizing: border-box; }
.result-item { padding: 24rpx 0; border-bottom: 1rpx solid #f0f0f0; }
.item-title { font-size: 30rpx; color: #333333; display: block; margin-bottom: 8rpx; }
.item-desc { font-size: 24rpx; color: #999999; display: block; }
.empty-tip { text-align: center; color: #999999; margin-top: 100rpx; font-size: 28rpx; }
</style>
