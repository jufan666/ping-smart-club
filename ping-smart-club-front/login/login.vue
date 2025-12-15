<template>
  <view class="container">
    <view class="header">
      <text class="title">{{pageTitle}}</text>
      <text class="subtitle">{{pageSubtitle}}</text>
    </view>
    
    <view class="content">
      <!-- 根据不同角色显示不同内容 -->
      <view v-if="userRole === 'parent'" class="role-content">
        <text class="welcome">欢迎，家长！</text>
        <text class="desc">这里可以查看孩子的学习情况和课程安排</text>
        
        <view class="stats">
          <view class="stat-item">
            <text class="stat-value">12</text>
            <text class="stat-label">已完成课程</text>
          </view>
          <view class="stat-item">
            <text class="stat-value">3</text>
            <text class="stat-label">待上课程</text>
          </view>
        </view>
        
        <view class="recent-activities">
          <text class="section-title">最近活动</text>
          <view class="activity-item">
            <text class="activity-title">乒乓球课程</text>
            <text class="activity-time">2023-11-20 16:00</text>
          </view>
        </view>
      </view>
      
      <view v-else-if="userRole === 'coach'" class="role-content">
        <text class="welcome">欢迎，教练！</text>
        <text class="desc">这里可以管理课程和查看学员情况</text>
        
        <view class="stats">
          <view class="stat-item">
            <text class="stat-value">25</text>
            <text class="stat-label">学员总数</text>
          </view>
          <view class="stat-item">
            <text class="stat-value">8</text>
            <text class="stat-label">今日课程</text>
          </view>
        </view>
      </view>
      
      <view v-else-if="userRole === 'admin'" class="role-content">
        <text class="welcome">欢迎，管理员！</text>
        <text class="desc">这里可以进行系统管理和数据统计</text>
        
        <view class="stats">
          <view class="stat-item">
            <text class="stat-value">156</text>
            <text class="stat-label">用户总数</text>
          </view>
          <view class="stat-item">
            <text class="stat-value">42</text>
            <text class="stat-label">教练总数</text>
          </view>
        </view>
      </view>
      
      <view v-else class="login-prompt">
        <text class="prompt-text">请先登录并选择身份</text>
        <button class="login-btn" @click="goToLogin">去登录</button>
      </view>
    </view>
    
    <!-- 使用自定义tabbar组件 -->
    <custom-tabbar 
      v-if="userRole" 
      :user-role="userRole" 
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
  data() {
    return {
      userRole: '',
      userInfo: {}
    };
  },
  computed: {
    pageTitle() {
      switch(this.userRole) {
        case 'coach':
          return '教练主页';
        case 'admin':
          return '管理首页';
        case 'parent':
        default:
          return '学员家长首页';
      }
    },
    pageSubtitle() {
      switch(this.userRole) {
        case 'coach':
          return '管理您的课程和学员';
        case 'admin':
          return '系统管理和数据分析';
        case 'parent':
        default:
          return '查看孩子的学习进度';
      }
    }
  },
  onLoad() {
    this.checkUserRole();
  },
  onShow() {
    this.checkUserRole();
  },
  methods: {
    checkUserRole() {
      try {
        const userRole = uni.getStorageSync('userRole');
        const userInfo = uni.getStorageSync('userInfo');
        
        if (userRole) {
          this.userRole = userRole;
        }
        
        if (userInfo) {
          this.userInfo = userInfo;
        }
      } catch (e) {
        console.error('获取用户信息失败:', e);
      }
    },
    goToLogin() {
      uni.navigateTo({
        url: '/pages/login/login'
      });
    },
    onTabChange(index) {
      console.log('当前选中的tab索引:', index);
    }
  }
}
</script>

<style lang="scss" scoped>
.container {
  padding: 20px;
  padding-bottom: 120rpx; /* 留出底部tabbar的空间 */
}

.header {
  margin-bottom: 30px;
  text-align: center;
}

.title {
  font-size: 28px;
  font-weight: bold;
  display: block;
  margin-bottom: 10px;
}

.subtitle {
  font-size: 16px;
  color: #666;
  display: block;
}

.content {
  width: 100%;
}

.role-content {
  display: flex;
  flex-direction: column;
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

.recent-activities {
  margin-top: 20px;
}

.section-title {
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 15px;
  display: block;
}

.activity-item {
  background-color: #f8f8f8;
  padding: 15px;
  border-radius: 8px;
  margin-bottom: 10px;
  display: flex;
  justify-content: space-between;
}

.activity-title {
  font-size: 16px;
}

.activity-time {
  font-size: 14px;
  color: #666;
}

.login-prompt {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 300px;
}

.prompt-text {
  font-size: 18px;
  color: #666;
  margin-bottom: 20px;
}

.login-btn {
  background-color: #007AFF;
  color: white;
  border: none;
  padding: 12px 30px;
  border-radius: 8px;
  font-size: 16px;
}
</style>