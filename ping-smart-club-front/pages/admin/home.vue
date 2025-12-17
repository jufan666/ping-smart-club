<template>
  <view class="container">
    <view class="header">管理首页</view>

    <view class="content">
      <text class="welcome">欢迎，管理员！</text>
      <text class="desc">这里可以进行系统管理和数据统计</text>

      <view class="stats-grid">
        <view class="stat-card">
          <text class="stat-value">156</text>
          <text class="stat-label">用户总数</text>
        </view>
        <view class="stat-card">
          <text class="stat-value">42</text>
          <text class="stat-label">教练总数</text>
        </view>
        <view class="stat-card">
          <text class="stat-value">89</text>
          <text class="stat-label">学员总数</text>
        </view>
        <view class="stat-card">
          <text class="stat-value">25</text>
          <text class="stat-label">今日活跃</text>
        </view>
      </view>

      <view class="chart-section">
        <text class="section-title">用户增长趋势</text>
        <view class="chart-placeholder">
          <text class="chart-text">图表区域（可集成图表库）</text>
        </view>
      </view>

      <view class="recent-activities">
        <text class="section-title">最近活动</text>
        <view class="activity-item">
          <view class="activity-info">
            <text class="activity-title">新增教练</text>
            <text class="activity-desc">张教练已通过审核</text>
          </view>
          <text class="activity-time">2小时前</text>
        </view>
        <view class="activity-item">
          <view class="activity-info">
            <text class="activity-title">系统更新</text>
            <text class="activity-desc">版本2.1.0已发布</text>
          </view>
          <text class="activity-time">1天前</text>
        </view>
      </view>
    </view>

    <!-- 使用自定义tabbar组件 -->
    <custom-tabbar 
      :user-role="'admin'" 
      :current="0" 
      @change="onTabChange"
    />
  </view>
</template>

<script>
import customTabbar from '@/pages/components/custom-tabbar.vue';

export default {
  components: {
    customTabbar
  },
  onLoad() {
    // 页面加载时跳转到俱乐部宣传页面
    uni.redirectTo({
      url: '/pages/club/club?role=admin'
    });
  },
  methods: {
    onTabChange(index) {
      console.log('当前选中的tab索引:', index);
    }
  }
};
</script>

<style lang="scss" scoped>
.container {
  padding: 20px;
  padding-bottom: 120rpx; /* 留出底部tabbar的空间 */
}

.header {
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 30px;
  text-align: center;
}

.content {
  width: 100%;
}

.welcome {
  font-size: 22px;
  font-weight: bold;
  margin-bottom: 10px;
  display: block;
}

.desc {
  font-size: 16px;
  color: #666;
  margin-bottom: 30px;
  display: block;
}

.stats-grid {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  margin-bottom: 30px;
}

.stat-card {
  width: 48%;
  background-color: #fff;
  border-radius: 8px;
  padding: 20px;
  margin-bottom: 15px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
  display: flex;
  flex-direction: column;
  align-items: center;
}

.stat-value {
  font-size: 32px;
  font-weight: bold;
  color: #007AFF;
  margin-bottom: 5px;
}

.stat-label {
  font-size: 14px;
  color: #666;
}

.chart-section {
  margin-bottom: 30px;
}

.section-title {
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 15px;
  display: block;
}

.chart-placeholder {
  height: 200px;
  background-color: #f8f8f8;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.chart-text {
  color: #999;
  font-size: 16px;
}

.recent-activities {
  margin-top: 20px;
}

.activity-item {
  display: flex;
  justify-content: space-between;
  padding: 15px;
  background-color: #f8f8f8;
  border-radius: 8px;
  margin-bottom: 10px;
}

.activity-info {
  flex: 1;
}

.activity-title {
  font-size: 16px;
  font-weight: bold;
  margin-bottom: 5px;
  display: block;
}

.activity-desc {
  font-size: 14px;
  color: #666;
  display: block;
}

.activity-time {
  font-size: 14px;
  color: #999;
}
</style>
