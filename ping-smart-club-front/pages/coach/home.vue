<template>
  <view class="container">
    <view class="header">教练主页</view>

    <view class="content">
      <text class="welcome">欢迎，教练！</text>
      <text class="desc">这里可以查看您的课程和学员信息</text>

      <view class="stats">
        <view class="stat-item">
          <text class="stat-value">25</text>
          <text class="stat-label">学员总数</text>
        </view>
        <view class="stat-item">
          <text class="stat-value">8</text>
          <text class="stat-label">今日课程</text>
        </view>
        <view class="stat-item">
          <text class="stat-value">95%</text>
          <text class="stat-label">出勤率</text>
        </view>
      </view>

      <view class="schedule">
        <text class="section-title">今日课程安排</text>
        <view class="course-item">
          <view class="course-time">09:00 - 10:00</view>
          <view class="course-info">
            <text class="course-name">乒乓球基础班</text>
            <text class="course-students">学员: 5人</text>
          </view>
        </view>
        <view class="course-item">
          <view class="course-time">14:00 - 15:00</view>
          <view class="course-info">
            <text class="course-name">乒乓球进阶班</text>
            <text class="course-students">学员: 8人</text>
          </view>
        </view>
      </view>
    </view>

    <!-- 使用自定义tabbar组件 -->
    <custom-tabbar 
      :user-role="'coach'" 
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
      url: '/pages/club/club?role=coach'
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

.stats {
  display: flex;
  justify-content: space-around;
  margin-bottom: 30px;
}

.stat-item {
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

.schedule {
  margin-top: 20px;
}

.section-title {
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 15px;
  display: block;
}

.course-item {
  display: flex;
  background-color: #f8f8f8;
  padding: 15px;
  border-radius: 8px;
  margin-bottom: 15px;
}

.course-time {
  width: 120rpx;
  font-size: 16px;
  font-weight: bold;
  color: #007AFF;
  margin-right: 15px;
}

.course-info {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.course-name {
  font-size: 16px;
  font-weight: bold;
  margin-bottom: 5px;
}

.course-students {
  font-size: 14px;
  color: #666;
}
</style>
