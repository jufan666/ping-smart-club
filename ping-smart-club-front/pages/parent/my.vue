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
    </view>

    <view class="menu-list">
      <view class="menu-item" @click="navigateTo('/pages/parent/children')">
        <text class="menu-title">我的孩子</text>
        <text class="menu-arrow">></text>
      </view>
      <view class="menu-item" @click="navigateTo('/pages/parent/records')">
        <text class="menu-title">学习记录</text>
        <text class="menu-arrow">></text>
      </view>
      <view class="menu-item" @click="navigateTo('/pages/parent/appointments')">
        <text class="menu-title">预约记录</text>
        <text class="menu-arrow">></text>
      </view>
      <view class="menu-item" @click="navigateTo('/pages/parent/settings')">
        <text class="menu-title">设置</text>
        <text class="menu-arrow">></text>
      </view>
    </view>

    <button class="logout-btn" @click="logout">退出登录</button>

    <!-- 使用自定义tabbar组件 -->
    <custom-tabbar 
      :user-role="'parent'" 
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
  margin-bottom: 40px;
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
  font-size: 18px;
  font-weight: bold;
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
