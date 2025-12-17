<template>
  <view class="login-container">
    <!-- 导航栏 -->
    <view class="navbar">
      <view class="nav-back" @click="handleBack" v-if="showBack">
        <text class="icon-text">←</text>
      </view>
      <text class="nav-title">用户登录</text>
    </view>

    <!-- 背景装饰 -->
    <view class="bg-circle bg-circle-1"></view>
    <view class="bg-circle bg-circle-2"></view>
    <view class="bg-circle bg-circle-3"></view>
    
    <!-- 主内容区域 -->
    <view class="content">
      <!-- 圆形头像区域 -->
      <view class="avatar-section">
        <view class="avatar-wrapper">
          <image 
            :src="userInfo.avatarUrl || '/static/images/default-avatar.png'" 
            class="avatar-image"
            mode="aspectFill"
            @error="handleImageError"
          />
          <view v-if="!userInfo.avatarUrl" class="avatar-placeholder">
          </view>
        </view>
        <text class="welcome-text">欢迎使用</text>
        <text class="app-name">我的小程序</text>
        <text class="user-tip" v-if="userInfo.nickName">您好，{{ userInfo.nickName }}</text>
      </view>
      
      <!-- 登录方式选择 -->
      <view class="login-methods" v-if="!showPhoneLogin">
        <!-- 微信一键登录 -->
        <button 
          class="login-button wechat-login" 
          :loading="wechatLoading"
          @getuserinfo="handleWechatLogin"
          open-type="getUserInfo"
        >
          <view class="button-content">
            <text class="button-text">{{ wechatLoading ? '登录中...' : '微信一键登录' }}</text>
          </view>
        </button>

        <!-- 手机号登录 -->
        <button 
          class="login-button phone-login" 
          @click="showPhoneLogin = true"
        >
          <view class="button-content">
            <text class="button-text">手机号登录</text>
          </view>
        </button>

        <!-- 游客登录 -->
        <view class="visitor-section">
          <button class="visitor-login-button" @click="handleVisitorLogin">
            <text class="visitor-text">游客登录</text>
          </button>
        </view>
      </view>

      <!-- 手机号登录表单 -->
      <view class="phone-login-form" v-else>
        <view class="form-header">
          <text class="form-title">手机号登录</text>
          <text class="form-subtitle">请输入您的手机号码</text>
        </view>

        <view class="input-group">
          <view class="input-item">
            <text class="input-label">手机号</text>
            <input 
              v-model="phoneNumber" 
              class="input-field" 
              type="number" 
              placeholder="请输入手机号码"
              maxlength="11"
            />
            <view class="input-clear" @click="phoneNumber = ''" v-if="phoneNumber">
              <text class="icon-text">×</text>
            </view>
          </view>

          <view class="input-item">
            <text class="input-label">验证码</text>
            <input 
              v-model="smsCode" 
              class="input-field" 
              type="number" 
              placeholder="请输入验证码"
              maxlength="6"
            />
            <button 
              class="sms-button" 
              :disabled="smsCountdown > 0 || !isPhoneValid"
              @click="handleSendSms"
            >
              {{ smsButtonText }}
            </button>
          </view>
        </view>

        <button 
          class="confirm-button" 
          :disabled="!isFormValid"
          :loading="phoneLoading"
          @click="handlePhoneLogin"
        >
          <text class="confirm-text">{{ phoneLoading ? '登录中...' : '登录' }}</text>
        </button>

        <view class="back-login" @click="showPhoneLogin = false">
          <text class="icon-text">←</text>
          <text class="back-text">返回其他登录方式</text>
        </view>
      </view>
    </view>

    <!-- 协议条款 -->
    <view class="agreement-section">
      <view class="agreement-checkbox" @click="toggleAgreement">
        <view class="checkbox-icon" :class="{ checked: agreementChecked }">
          <text class="icon-text" v-if="agreementChecked">✓</text>
        </view>
        <text class="agreement-text">我已阅读并同意</text>
      </view>
      <view class="agreement-links">
        <text class="agreement-link" @click="showAgreement('user')">《用户协议》</text>
        <text class="agreement-link" @click="showAgreement('privacy')">《隐私政策》</text>
      </view>
    </view>

    <!-- 自定义弹窗组件 -->
    <view v-if="showVisitorDialog" class="custom-dialog-mask">
      <view class="custom-dialog">
        <view class="dialog-header">
          <text class="dialog-title">游客模式</text>
        </view>
        <view class="dialog-content">
          <text class="dialog-message">游客模式下部分功能将受限，是否继续？</text>
        </view>
        <view class="dialog-actions">
          <button class="dialog-button cancel" @click="handleVisitorClose">取消</button>
          <button class="dialog-button confirm" @click="handleVisitorConfirm">确定</button>
        </view>
      </view>
    </view>

    <!-- 协议详情弹窗 -->
    <view v-if="showAgreementDialog" class="custom-dialog-mask">
      <view class="agreement-dialog">
        <view class="dialog-header">
          <text class="dialog-title">{{ agreementTitle }}</text>
          <text class="dialog-close" @click="closeAgreement">×</text>
        </view>
        <scroll-view class="dialog-content agreement-content" scroll-y>
          <text class="agreement-text-content">{{ agreementContent }}</text>
        </scroll-view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      // 用户信息
      userInfo: {
        avatarUrl: '',
        nickName: ''
      },
      
      // 应用信息
      appName: '我的小程序',
      
      // 登录状态
      wechatLoading: false,
      phoneLoading: false,
      
      // 手机号登录相关
      showPhoneLogin: false,
      phoneNumber: '',
      smsCode: '',
      smsCountdown: 0,
      
      // 协议相关
      agreementChecked: false,
      agreementTitle: '',
      agreementContent: '',
      showAgreementDialog: false,
      
      // 弹窗相关
      showVisitorDialog: false,
      
      // 其他
      showBack: false
    }
  },
  
  computed: {
    // 验证手机号格式
    isPhoneValid() {
      return /^1[3-9]\d{9}$/.test(this.phoneNumber)
    },
    
    // 验证表单是否完整
    isFormValid() {
      return this.isPhoneValid && this.smsCode.length === 6 && this.agreementChecked
    },
    
    // 验证码按钮文本
    smsButtonText() {
      if (this.smsCountdown > 0) {
        return `${this.smsCountdown}秒后重新发送`
      }
      return '获取验证码'
    }
  },
  
  onLoad(options) {
    // 检查是否有返回页面
    if (options.from) {
      this.showBack = true
    }
    
    // 检查本地存储的用户信息
    this.checkLocalUserInfo()
    
    // 检查协议是否已同意
    this.checkAgreementStatus()
  },
  
  methods: {
    // 检查本地存储的用户信息
    checkLocalUserInfo() {
      try {
        const userInfo = uni.getStorageSync('userInfo')
        if (userInfo && userInfo.nickName) {
          this.userInfo = userInfo
        } else {
          // 确保userInfo结构正确
          this.userInfo = {
            avatarUrl: '',
            nickName: ''
          }
        }
      } catch (e) {
        console.error('获取用户信息失败:', e)
        this.userInfo = {
          avatarUrl: '',
          nickName: ''
        }
      }
    },
    
    // 检查协议状态
    checkAgreementStatus() {
      try {
        const agreed = uni.getStorageSync('agreementAgreed')
        this.agreementChecked = !!agreed
      } catch (e) {
        console.error('获取协议状态失败:', e)
      }
    },
    
    // 微信登录
    async handleWechatLogin(e) {
      if (!this.agreementChecked) {
        uni.showToast({
          title: '请先同意用户协议和隐私政策',
          icon: 'none'
        })
        return
      }
      
      if (e.detail.userInfo) {
        this.wechatLoading = true
        
        try {
          // 1. 获取微信登录code
          const loginRes = await new Promise((resolve, reject) => {
            uni.login({
              provider: 'weixin',
              success: resolve,
              fail: reject
            })
          })
          
          // 2. 获取用户信息
          const userInfo = e.detail.userInfo
          
          // 3. 调用后端登录接口
          const loginResult = await this.mockWechatLogin(loginRes.code, userInfo)
          
          if (loginResult.success) {
            await this.handleLoginSuccess(loginResult.data)
          } else {
            throw new Error(loginResult.message)
          }
          
        } catch (error) {
          console.error('微信登录失败:', error)
          uni.showToast({
            title: '登录失败，请重试',
            icon: 'none'
          })
        } finally {
          this.wechatLoading = false
        }
      } else {
        uni.showToast({
          title: '授权失败',
          icon: 'none'
        })
      }
    },
    
    // 模拟微信登录
    mockWechatLogin(code, userInfo) {
      return new Promise((resolve) => {
        setTimeout(() => {
          // 模拟登录成功
          resolve({
            success: true,
            data: {
              token: 'mock_token_' + Date.now(),
              userInfo: {
                nickName: userInfo.nickName || '微信用户',
                avatarUrl: userInfo.avatarUrl || '/static/images/default-avatar.png'
              },
              expiresIn: 7200
            },
            message: '登录成功'
          })
        }, 1500)
      })
    },
    
    // 发送短信验证码
    async handleSendSms() {
      if (!this.isPhoneValid) {
        uni.showToast({
          title: '请输入正确的手机号码',
          icon: 'none'
        })
        return
      }
      
      try {
        // 调用发送短信接口
        // await this.sendSmsApi(this.phoneNumber)
        
        // 开始倒计时
        this.startSmsCountdown()
        
        uni.showToast({
          title: '验证码已发送',
          icon: 'success'
        })
      } catch (error) {
        uni.showToast({
          title: '发送失败，请重试',
          icon: 'none'
        })
      }
    },
    
    // 开始短信倒计时
    startSmsCountdown() {
      this.smsCountdown = 60
      const timer = setInterval(() => {
        this.smsCountdown--
        if (this.smsCountdown <= 0) {
          clearInterval(timer)
        }
      }, 1000)
    },
    
    // 手机号登录
    async handlePhoneLogin() {
      if (!this.isFormValid) {
        uni.showToast({
          title: '请填写完整信息',
          icon: 'none'
        })
        return
      }
      
      this.phoneLoading = true
      
      try {
        // 调用手机号登录接口
        const loginResult = await this.mockPhoneLogin(this.phoneNumber, this.smsCode)
        
        if (loginResult.success) {
          await this.handleLoginSuccess(loginResult.data)
        } else {
          throw new Error(loginResult.message)
        }
      } catch (error) {
        uni.showToast({
          title: error.message || '登录失败',
          icon: 'none'
        })
      } finally {
        this.phoneLoading = false
      }
    },
    
    // 模拟手机号登录
    mockPhoneLogin(phone, code) {
      return new Promise((resolve) => {
        setTimeout(() => {
          // 模拟登录成功
          resolve({
            success: true,
            data: {
              token: 'mock_phone_token_' + Date.now(),
              userInfo: {
                nickName: '用户_' + phone.slice(-4),
                avatarUrl: '/static/images/default-avatar.png'
              },
              expiresIn: 7200
            },
            message: '登录成功'
          })
        }, 1500)
      })
    },
    
    // 处理登录成功
    async handleLoginSuccess(loginData) {
      // 保存token和用户信息
      try {
        uni.setStorageSync('token', loginData.token)
        uni.setStorageSync('userInfo', loginData.userInfo)
        uni.setStorageSync('loginTime', Date.now())
        uni.setStorageSync('agreementAgreed', true)
        
        // 确保userInfo结构正确
        this.userInfo = {
          nickName: loginData.userInfo.nickName || '',
          avatarUrl: loginData.userInfo.avatarUrl || '/static/images/default-avatar.png'
        }
        
        this.agreementChecked = true
        
        uni.showToast({
          title: '登录成功',
          icon: 'success'
        })
        
        // 延迟跳转
        setTimeout(() => {
          this.redirectToTargetPage()
        }, 1500)
        
      } catch (error) {
        console.error('保存登录信息失败:', error)
      }
    },
    
    // 跳转到目标页面
    redirectToTargetPage() {
      // 清除之前存储的身份信息
      try {
        uni.removeStorageSync('userRole')
        console.log('已清除之前存储的身份信息')
      } catch (e) {
        console.error('清除身份信息失败:', e)
      }

      // 每次登录后都跳转到身份选择页面
      console.log('跳转到身份选择页面')
      uni.navigateTo({
        url: '/pages/identity/identity'
      })
    },
    
    // 游客登录
    handleVisitorLogin() {
      this.showVisitorDialog = true
    },
    
    // 游客模式确认
    handleVisitorConfirm() {
      // 设置游客模式标识
      uni.setStorageSync('visitorMode', true)
      this.showVisitorDialog = false
      
      uni.showToast({
        title: '已进入游客模式',
        icon: 'success'
      })
      
      setTimeout(() => {
        this.redirectToTargetPage()
      }, 1000)
    },
    
    // 游客模式取消
    handleVisitorClose() {
      this.showVisitorDialog = false
    },
    
    // 获取手机号
    handleGetPhone(e) {
      if (e.detail.code) {
        // 使用code获取手机号
        console.log('获取手机号code:', e.detail.code)
        // 调用后端接口解密手机号
      } else {
        uni.showToast({
          title: '获取手机号失败',
          icon: 'none'
        })
      }
    },
    
    // 切换协议同意状态
    toggleAgreement() {
      this.agreementChecked = !this.agreementChecked
      if (this.agreementChecked) {
        uni.setStorageSync('agreementAgreed', true)
      }
    },
    
    // 显示协议详情
    showAgreement(type) {
      if (type === 'user') {
        this.agreementTitle = '用户协议'
        this.agreementContent = this.getUserAgreementContent()
      } else {
        this.agreementTitle = '隐私政策'
        this.agreementContent = this.getPrivacyAgreementContent()
      }
      this.showAgreementDialog = true
    },
    
    // 关闭协议弹窗
    closeAgreement() {
      this.showAgreementDialog = false
    },
    
    // 用户协议内容
    getUserAgreementContent() {
      return `用户协议

欢迎使用本小程序！请您仔细阅读以下条款：

1. 服务条款
   我们为您提供...的服务

2. 用户权利与义务
   您有权...同时您需要遵守...

3. 使用规范
   请勿进行以下行为...

4. 免责声明
   在法律法规允许的范围内...

5. 其他条款
   本协议的解释权归...所有

感谢您的使用！`
    },
    
    // 隐私政策内容
    getPrivacyAgreementContent() {
      return `隐私政策

我们高度重视您的隐私保护：

1. 信息收集
   我们仅收集必要的用户信息...

2. 信息使用
   收集的信息将用于...

3. 信息共享
   我们不会与第三方共享...

4. 信息安全
   我们采取严格的安全措施...

5. 您的权利
   您有权查询、修改、删除...

如有疑问，请联系我们。`
    },
    
    // 返回上一页
    handleBack() {
      uni.navigateBack()
    },
    
    // 图片加载失败处理
    handleImageError() {
      this.userInfo.avatarUrl = '/static/images/default-avatar.png'
    }
  }
}
</script>

<style scoped>
.login-container {
  position: relative;
  width: 100%;
  height: 100vh;
  min-height: 100vh;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  overflow: hidden;
}

/* 导航栏样式 */
.navbar {
  position: relative;
  height: 88rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 0 30rpx;
  z-index: 100;
}

.nav-back {
  position: absolute;
  left: 30rpx;
  width: 60rpx;
  height: 60rpx;
  display: flex;
  align-items: center;
  justify-content: center;
}

.icon-text {
  font-size: 32rpx;
}

.nav-title {
  font-size: 36rpx;
  color: #fff;
  font-weight: bold;
}

/* 背景装饰圆圈 */
.bg-circle {
  position: absolute;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.1);
}

.bg-circle-1 {
  width: 300rpx;
  height: 300rpx;
  top: -150rpx;
  right: -150rpx;
}

.bg-circle-2 {
  width: 200rpx;
  height: 200rpx;
  bottom: 100rpx;
  left: -100rpx;
}

.bg-circle-3 {
  width: 150rpx;
  height: 150rpx;
  bottom: -75rpx;
  right: 20%;
}

.content {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  z-index: 10;
  padding: 0 40rpx;
  box-sizing: border-box;
}

/* 头像区域样式 */
.avatar-section {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 80rpx;
  width: 100%;
}

.avatar-wrapper {
  width: 160rpx;
  height: 160rpx;
  border-radius: 50%;
  overflow: hidden;
  border: 4rpx solid rgba(255, 255, 255, 0.3);
  box-shadow: 0 10rpx 30rpx rgba(0, 0, 0, 0.2);
  margin-bottom: 30rpx;
  position: relative;
}

.avatar-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  position: relative;
  z-index: 1;
}

.avatar-placeholder {
  width: 100%;
  height: 100%;
  background-color: #cccccc;
  border-radius: 50%;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 0;
}

.welcome-text {
  font-size: 32rpx;
  color: rgba(255, 255, 255, 0.9);
  margin-bottom: 8rpx;
  text-align: center;
}

.app-name {
  font-size: 40rpx;
  color: #fff;
  font-weight: bold;
  text-align: center;
  margin-bottom: 10rpx;
}

.user-tip {
  font-size: 26rpx;
  color: rgba(255, 255, 255, 0.8);
  text-align: center;
}

/* 登录按钮样式 */
.login-methods {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.login-button {
  width: 100%;
  height: 90rpx;
  border-radius: 50rpx;
  display: flex;
  justify-content: center;
  align-items: center;
  border: none;
  margin-bottom: 25rpx;
  box-shadow: 0 6rpx 20rpx rgba(0, 0, 0, 0.15);
}

.login-button::after {
  border: none;
}

.button-content {
  display: flex;
  align-items: center;
  justify-content: center;
}

.button-content .icon-text {
  margin-right: 15rpx;
  font-size: 36rpx;
}

.button-text {
  font-size: 32rpx;
  font-weight: 500;
}

.wechat-login {
  background: #07C160;
}

.wechat-login .button-text {
  color: #fff;
}

.phone-login {
  background: #fff;
  border: 2rpx solid #667eea;
}

.phone-login .button-text {
  color: #667eea;
}

/* 其他登录方式 */
.other-methods {
  margin-top: 50rpx;
  text-align: center;
  width: 100%;
}

.other-text {
  font-size: 26rpx;
  color: rgba(255, 255, 255, 0.7);
  margin-bottom: 20rpx;
  display: block;
}

.method-icons {
  display: flex;
  justify-content: center;
  gap: 40rpx;
}

.icon-button {
  width: 80rpx;
  height: 80rpx;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.2);
  display: flex;
  align-items: center;
  justify-content: center;
  border: none;
}

.icon-button::after {
  border: none;
}

.visitor-section {
  display: flex;
  justify-content: center;
  margin-top: 60rpx;
  width: 100%;
}

.visitor-login-button {
  width: 240rpx;
  height: 90rpx;
  border-radius: 45rpx;
  background: linear-gradient(135deg, rgba(102, 126, 234, 0.8), rgba(118, 75, 162, 0.8));
  display: flex;
  align-items: center;
  justify-content: center;
  border: none;
  padding: 0 20rpx;
  box-sizing: border-box;
  box-shadow: 0 8rpx 20rpx rgba(0, 0, 0, 0.15);
  transition: all 0.3s ease;
}

.visitor-login-button:active {
  transform: scale(0.98);
  box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.15);
}

.visitor-login-button::after {
  border: none;
}

.visitor-text {
  font-size: 32rpx;
  color: #fff;
  font-weight: 600;
  letter-spacing: 2rpx;
  text-shadow: 0 2rpx 4rpx rgba(0, 0, 0, 0.2);
}

/* 手机号登录表单 */
.phone-login-form {
  width: 100%;
}

.form-header {
  text-align: center;
  margin-bottom: 50rpx;
}

.form-title {
  font-size: 36rpx;
  color: #fff;
  font-weight: bold;
  display: block;
  margin-bottom: 12rpx;
}

.form-subtitle {
  font-size: 26rpx;
  color: rgba(255, 255, 255, 0.8);
  display: block;
}

.input-group {
  width: 100%;
  margin-bottom: 50rpx;
}

.input-item {
  position: relative;
  margin-bottom: 30rpx;
  background: rgba(255, 255, 255, 0.9);
  border-radius: 25rpx;
  padding: 0 30rpx;
  height: 80rpx;
  display: flex;
  align-items: center;
}

.input-label {
  font-size: 28rpx;
  color: #333;
  width: 140rpx;
}

.input-field {
  flex: 1;
  font-size: 28rpx;
  color: #333;
  height: 100%;
}

.input-clear {
  width: 40rpx;
  height: 40rpx;
  border-radius: 50%;
  background: #f0f0f0;
  display: flex;
  align-items: center;
  justify-content: center;
}

.sms-button {
  background: #667eea;
  color: #fff;
  border: none;
  border-radius: 20rpx;
  padding: 8rpx 16rpx;
  font-size: 24rpx;
  min-width: 160rpx;
}

.sms-button[disabled] {
  background: #ccc;
  color: #999;
}

.sms-button::after {
  border: none;
}

.confirm-button {
  width: 100%;
  height: 90rpx;
  background: #fff;
  border-radius: 50rpx;
  display: flex;
  justify-content: center;
  align-items: center;
  border: none;
  margin-bottom: 30rpx;
  box-shadow: 0 6rpx 20rpx rgba(0, 0, 0, 0.15);
}

.confirm-button[disabled] {
  background: #ccc;
}

.confirm-button::after {
  border: none;
}

.confirm-text {
  font-size: 32rpx;
  color: #667eea;
  font-weight: 500;
}

.back-login {
  display: flex;
  align-items: center;
  justify-content: center;
  color: #667eea;
  padding: 20rpx;
}

.back-text {
  font-size: 28rpx;
  margin-left: 10rpx;
}

/* 协议条款 */
.agreement-section {
  position: absolute;
  bottom: 40rpx;
  left: 0;
  right: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 0 40rpx;
}

.agreement-checkbox {
  display: flex;
  align-items: center;
  margin-bottom: 15rpx;
}

.checkbox-icon {
  width: 30rpx;
  height: 30rpx;
  border: 2rpx solid rgba(255, 255, 255, 0.7);
  border-radius: 6rpx;
  margin-right: 12rpx;
  display: flex;
  align-items: center;
  justify-content: center;
}

.checkbox-icon.checked {
  background: #667eea;
  border-color: #667eea;
}

.checkbox-icon .icon-text {
  font-size: 20rpx;
  color: #fff;
}

.agreement-text {
  font-size: 24rpx;
  color: rgba(255, 255, 255, 0.7);
}

.agreement-links {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
}

.agreement-link {
  font-size: 24rpx;
  color: #fff;
  text-decoration: underline;
  margin: 0 8rpx;
}

/* 自定义弹窗 */
.custom-dialog-mask {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 9999;
  padding: 0 40rpx;
}

.custom-dialog {
  background: #fff;
  border-radius: 20rpx;
  width: 100%;
  max-width: 600rpx;
  overflow: hidden;
}

.agreement-dialog {
  background: #fff;
  border-radius: 20rpx;
  width: 100%;
  max-width: 600rpx;
  height: 70vh;
  overflow: hidden;
  display: flex;
  flex-direction: column;
}

.dialog-header {
  padding: 30rpx 40rpx;
  border-bottom: 1rpx solid #f0f0f0;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.dialog-title {
  font-size: 32rpx;
  font-weight: bold;
  color: #333;
}

.dialog-close {
  font-size: 36rpx;
  color: #999;
}

.dialog-content {
  padding: 30rpx 40rpx;
  flex: 1;
}

.dialog-message {
  font-size: 28rpx;
  color: #666;
  text-align: center;
}

.agreement-content {
  height: 100%;
}

.agreement-text-content {
  font-size: 26rpx;
  color: #333;
  line-height: 1.6;
  white-space: pre-line;
}

.dialog-actions {
  display: flex;
  border-top: 1rpx solid #f0f0f0;
}

.dialog-button {
  flex: 1;
  height: 90rpx;
  border: none;
  background: none;
  font-size: 30rpx;
}

.dialog-button::after {
  border: none;
}

.dialog-button.cancel {
  color: #666;
  border-right: 1rpx solid #f0f0f0;
}

.dialog-button.confirm {
  color: #667eea;
  font-weight: 500;
}
</style>