<template>
  <view class="container">
    <view class="back-btn" @click="goBack">
      <text class="back-icon">‹</text>
      <text class="back-text">返回</text>
    </view>
    <view class="header">
      <text class="title">选择您的身份</text>
      <text class="subtitle">请根据您的角色选择对应的身份</text>
    </view>

    <view class="content">
      <view class="option-card" @click="selectIdentity('parent')">
        <view class="card-icon">
          <text class="icon">👨‍👩‍👧‍👦</text>
        </view>
        <view class="card-content">
          <text class="card-title">学员家长</text>
          <text class="card-desc">查看孩子的学习进度和课程安排</text>
        </view>
      </view>

      <view class="option-card" @click="selectIdentity('coach')">
        <view class="card-icon">
          <text class="icon">👨‍🏫</text>
        </view>
        <view class="card-content">
          <text class="card-title">教练</text>
          <text class="card-desc">管理课程和学员学习情况</text>
        </view>
      </view>

      <view class="option-card" @click="selectIdentity('admin')">
        <view class="card-icon">
          <text class="icon">👨‍💼</text>
        </view>
        <view class="card-content">
          <text class="card-title">管理员</text>
          <text class="card-desc">系统管理和数据统计</text>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {

    }
  },

  onLoad() {
    console.log('身份选择页面加载')
  },

  methods: {
    goBack() {
      // 获取当前页面栈
      const pages = getCurrentPages()
      // 如果页面栈长度大于1，说明有上一页可以返回
      if (pages.length > 1) {
        uni.navigateBack({
          delta: 1
        })
      } else {
        // 否则跳转到登录页面
        uni.reLaunch({
          url: '/pages/login/login'
        })
      }
    },

    selectIdentity(type) {
      console.log('选择身份:', type)
      uni.setStorageSync('userRole', type)

      uni.showToast({
        title: '身份选择成功',
        icon: 'success'
      })

      setTimeout(() => {
        uni.reLaunch({
          url: '/pages/club/club'
        })
      }, 1500)
    }
  }
}
</script>

<style>
.container {
  min-height: 100vh;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  padding: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.back-btn {
  position: absolute;
  top: 40px;
  left: 20px;
  display: flex;
  align-items: center;
  padding: 8px 16px;
  background-color: rgba(255, 255, 255, 0.2);
  border-radius: 20px;
  z-index: 10;
}

.back-icon {
  font-size: 24px;
  color: #ffffff;
  margin-right: 5px;
}

.back-text {
  font-size: 16px;
  color: #ffffff;
}

.header {
  text-align: center;
  margin-bottom: 40px;
  width: 100%;
  padding-top: 60px;
}

.title {
  font-size: 32px;
  font-weight: bold;
  color: #ffffff;
  display: block;
  margin-bottom: 10px;
  width: 100%;
}

.subtitle {
  font-size: 16px;
  color: rgba(255, 255, 255, 0.8);
  display: block;
}

.content {
  width: 100%;
  max-width: 600px;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.option-card {
  background-color: #ffffff;
  border-radius: 16px;
  padding: 20px;
  display: flex;
  align-items: center;
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
}

.option-card:active {
  transform: scale(0.98);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.card-icon {
  width: 60px;
  height: 60px;
  background-color: #f5f7fa;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-right: 20px;
}

.icon {
  font-size: 30px;
}

.card-content {
  flex: 1;
}

.card-title {
  font-size: 20px;
  font-weight: bold;
  color: #333333;
  display: block;
  margin-bottom: 5px;
}

.card-desc {
  font-size: 14px;
  color: #666666;
  display: block;
}
</style>
