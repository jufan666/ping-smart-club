<template>
  <view class="container">
    <view class="header">约课页面</view>

    <view class="content">
      <text>这里可以查看可预约的课程</text>
      <text>选择合适的教练和时间进行预约</text>

      <view class="course-list">
        <view class="course-item" v-for="(item, index) in courseList" :key="index">
          <view class="course-info">
            <text class="course-name">{{item.name}}</text>
            <text class="course-coach">教练: {{item.coach}}</text>
            <text class="course-time">时间: {{item.time}}</text>
          </view>
          <button class="book-btn" @click="bookCourse(index)">预约</button>
        </view>
      </view>
    </view>

    <!-- 使用自定义tabbar组件 -->
    <custom-tabbar 
      :user-role="'parent'" 
      :current="1" 
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
      courseList: [
        {
          name: "乒乓球基础班",
          coach: "张教练",
          time: "周三 16:00-17:00"
        },
        {
          name: "乒乓球进阶班",
          coach: "李教练",
          time: "周五 18:00-19:00"
        },
        {
          name: "一对一私教",
          coach: "王教练",
          time: "预约制"
        }
      ]
    };
  },
  methods: {
    bookCourse(index) {
      uni.showToast({
        title: '预约成功',
        icon: 'success'
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

.content {
  width: 100%;
}

.course-list {
  margin-top: 20px;
}

.course-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px;
  background-color: #f8f8f8;
  border-radius: 8px;
  margin-bottom: 15px;
}

.course-info {
  display: flex;
  flex-direction: column;
}

.course-name {
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 5px;
}

.course-coach, .course-time {
  font-size: 14px;
  color: #666;
  margin-bottom: 3px;
}

.book-btn {
  background-color: #007AFF;
  color: white;
  border: none;
  padding: 8px 15px;
  border-radius: 4px;
  font-size: 14px;
}
</style>
