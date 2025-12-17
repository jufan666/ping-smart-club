<template>
  <view class="container">
    <view class="header">学员家长页面</view>

    <view class="content">
      <text>欢迎，家长！</text>
      <text>这里可以查看学员课程、成绩等信息</text>
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
    console.log('家长页面加载');
    // 页面加载时跳转到俱乐部宣传页面
    uni.redirectTo({
      url: '/pages/club/club?role=parent'
    });
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
