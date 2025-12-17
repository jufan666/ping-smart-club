<template>
  <view class="container">
    <view class="header">个人中心</view>

    <view class="profile-section">
      <view class="avatar-wrapper">
        <image 
          :src="userInfo.avatarUrl || '/static/images/default-avatar.png'" 
          class="avatar"
          mode="aspectFill"
        ></image>
      </view>
      <text class="username">{{userInfo.nickName || '未登录'}}</text>
      <text class="user-role">系统管理员</text>
    </view>

    <view class="stats-row">
      <view class="stat-item">
        <text class="stat-value">156</text>
        <text class="stat-label">管理用户</text>
      </view>
      <view class="stat-item">
        <text class="stat-value">42</text>
        <text class="stat-label">管理教练</text>
      </view>
      <view class="stat-item">
        <text class="stat-value">89</text>
        <text class="stat-label">管理课程</text>
      </view>
    </view>

    <view class="menu-list">
      <view class="menu-item" @click="navigateTo('/pages/admin/logs')">
        <text class="menu-title">系统日志</text>
        <text class="menu-arrow">></text>
      </view>
      <view class="menu-item" @click="navigateTo('/pages/admin/backup')">
        <text class="menu-title">数据备份</text>
        <text class="menu-arrow">></text>
      </view>
      <view class="menu-item" @click="navigateTo('/pages/admin/security')">
        <text class="menu-title">安全设置</text>
        <text class="menu-arrow">></text>
      </view>
      <view class="menu-item" @click="navigateTo('/pages/admin/announcement')">
        <text class="menu-title">公告管理</text>
        <text class="menu-arrow">></text>
      </view>
      <view class="menu-item" @click="navigateTo('/pages/admin/settings')">
        <text class="menu-title">系统设置</text>
        <text class="menu-arrow">></text>
      </view>
    </view>

    <button class="logout-btn" @click="logout">退出登录</button>

    <!-- 使用自定义tabbar组件 -->
    <custom-tabbar 
      :user-role="'admin'" 
      :current="2" 
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
      userInfo: {}
    };
  },
  onLoad() {
    this.getUserInfo();
  },
  methods: {
    getUserInfo() {
      try {
        const userInfo = uni.getStorageSync('userInfo');
        if (userInfo) {
          this.userInfo = userInfo;
        }
      } catch (e) {
        console.error('获取用户信息失败:', e);
      }
    },
    navigateTo(url) {
      uni.showToast({
        title: '功能开发中',
        icon: 'none'
      });
    },
    logout() {
      uni.showModal({
        title: '提示',
        content: '确定要退出登录吗？',
        success: (res) => {
          if (res.confirm) {
            try {
              uni.removeStorageSync('token');
              uni.removeStorageSync('userInfo');
              uni.removeStorageSync('userRole');

              uni.reLaunch({
                url: '/pages/login/login'
              });
            } catch (e) {
              console.error('退出登录失败:', e);
            }
          }
        }
      });
    },
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

.profile-section {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 30px;
}

.avatar-wrapper {
  width: 120rpx;
  height: 120rpx;
  border-radius: 60rpx;
  overflow: hidden;
  margin-bottom: 15px;
  border: 2px solid #eee;
}

.avatar {
  width: 100%;
  height: 100%;
}

.username {
  font-size: 20px;
  font-weight: bold;
  margin-bottom: 5px;
}

.user-role {
  font-size: 14px;
  color: #666;
  background-color: #f0f0f0;
  padding: 4px 12px;
  border-radius: 12px;
}

.stats-row {
  display: flex;
  justify-content: space-around;
  margin-bottom: 30px;
  background-color: #f8f8f8;
  padding: 20px 0;
  border-radius: 8px;
}

.stat-item {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.stat-value {
  font-size: 24px;
  font-weight: bold;
  color: #007AFF;
  margin-bottom: 5px;
}

.stat-label {
  font-size: 14px;
  color: #666;
}

.menu-list {
  background-color: #ffffff;
  border-radius: 8px;
  overflow: hidden;
  margin-bottom: 30px;
}

.menu-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px 20px;
  border-bottom: 1px solid #f0f0f0;

  &:last-child {
    border-bottom: none;
  }
}

.menu-title {
  font-size: 16px;
}

.menu-arrow {
  color: #999;
  font-size: 18px;
}

.logout-btn {
  background-color: #ff4d4f;
  color: white;
  border: none;
  padding: 12px 0;
  border-radius: 8px;
  font-size: 16px;
  width: 100%;
}
</style>
