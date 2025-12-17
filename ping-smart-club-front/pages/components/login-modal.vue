<template>
  <view v-if="show" class="modal-mask" @tap="handleMaskClick">
    <view class="modal-container" @tap.stop>
      <!-- 关闭按钮 -->
      <view class="close-btn" @click="handleClose">
        <text class="iconfont icon-close"></text>
      </view>
      
      <!-- 弹窗内容 -->
      <view class="modal-content">
        <view class="modal-header">
          <text class="modal-title">微信授权登录</text>
          <text class="modal-subtitle">请授权以下信息以继续使用</text>
        </view>
        
        <view class="auth-list">
          <view class="auth-item">
            <text class="iconfont icon-user-circle"></text>
            <text class="auth-text">获取您的公开信息(昵称、头像等)</text>
          </view>
          <view class="auth-item">
            <text class="iconfont icon-lock"></text>
            <text class="auth-text">安全加密，保障您的隐私安全</text>
          </view>
        </view>
        
        <button 
          class="login-button" 
          :loading="loading"
          @click="handleLogin"
          open-type="getUserInfo"
        >
          <text class="login-button-text">{{ loading ? '授权中...' : '允许授权' }}</text>
        </button>
        
        <view class="agreement">
          <text class="agreement-text">登录即表示您已同意</text>
          <text class="agreement-link">《用户协议》</text>
          <text class="agreement-text">和</text>
          <text class="agreement-link">《隐私政策》</text>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  props: {
    show: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      loading: false
    }
  },
  methods: {
    // 处理遮罩层点击
    handleMaskClick() {
      this.$emit('close')
    },
    
    // 处理关闭按钮点击
    handleClose() {
      this.$emit('close')
    },
    
    // 处理登录授权
    handleLogin(e) {
      if (this.loading) return
      
      this.loading = true
      
      // 获取用户信息
      if (e.detail.userInfo) {
        // 用户允许授权
        const userInfo = e.detail.userInfo
        
        // 模拟登录过程
        this.mockLogin(userInfo)
      } else {
        // 用户拒绝授权
        uni.showToast({
          title: '授权失败',
          icon: 'none',
          duration: 2000
        })
        this.loading = false
      }
    },
    
    // 模拟登录过程
    mockLogin(userInfo) {
      // 在实际项目中，这里应该调用后端登录接口
      setTimeout(() => {
        // 保存用户信息到本地存储
        try {
          uni.setStorageSync('userInfo', userInfo)
        } catch (e) {
          console.error('保存用户信息失败:', e)
        }
        
        // 触发登录成功事件
        this.$emit('login-success', userInfo)
        this.loading = false
      }, 1500)
    }
  }
}
</script>

<style scoped>
.modal-mask {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: flex-end;
  z-index: 999;
}

.modal-container {
  width: 100%;
  background: #fff;
  border-top-left-radius: 30rpx;
  border-top-right-radius: 30rpx;
  padding: 40rpx;
  box-sizing: border-box;
  position: relative;
  animation: slideUp 0.3s ease-out;
}

@keyframes slideUp {
  from {
    transform: translateY(100%);
  }
  to {
    transform: translateY(0);
  }
}

.close-btn {
  position: absolute;
  top: 30rpx;
  right: 30rpx;
  width: 50rpx;
  height: 50rpx;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 10;
}

.close-btn .iconfont {
  font-size: 36rpx;
  color: #999;
}

.modal-content {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.modal-header {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 50rpx;
}

.modal-title {
  font-size: 40rpx;
  font-weight: bold;
  color: #333;
  margin-bottom: 15rpx;
}

.modal-subtitle {
  font-size: 28rpx;
  color: #666;
}

.auth-list {
  width: 100%;
  margin-bottom: 60rpx;
}

.auth-item {
  display: flex;
  align-items: center;
  padding: 25rpx 0;
  border-bottom: 1rpx solid #f5f5f5;
}

.auth-item:last-child {
  border-bottom: none;
}

.auth-item .iconfont {
  font-size: 36rpx;
  color: #667eea;
  margin-right: 20rpx;
}

.auth-text {
  font-size: 28rpx;
  color: #333;
}

.login-button {
  width: 100%;
  height: 90rpx;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 50rpx;
  display: flex;
  justify-content: center;
  align-items: center;
  border: none;
  margin-bottom: 40rpx;
}

.login-button::after {
  border: none;
}

.login-button-text {
  font-size: 32rpx;
  color: #fff;
  font-weight: 500;
}

.agreement {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.agreement-text {
  font-size: 24rpx;
  color: #999;
}

.agreement-link {
  font-size: 24rpx;
  color: #667eea;
}
</style>