<template>
  <view class="container">
    <view class="header">管理员页面</view>

    <view class="content">
      <text>欢迎，管理员！</text>
      <text>这里可以进行系统管理、数据统计等操作</text>
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
    console.log('管理员页面加载')
  }
}
</script>

<style>
.container {
  padding: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.header {
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 30px;
}

.content {
  width: 100%;
  display: flex;
  flex-direction: column;
  gap: 20px;
}
</style>
