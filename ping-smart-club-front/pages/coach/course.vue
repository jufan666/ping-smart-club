<template>
  <view class="container">
    <view class="header">课程管理</view>

    <view class="content">
      <view class="tab-header">
        <view 
          v-for="(tab, index) in tabs" 
          :key="index" 
          :class="['tab-item', {active: currentTab === index}]" 
          @click="switchTab(index)"
        >
          {{tab}}
        </view>
      </view>

      <!-- 全部课程 -->
      <view v-if="currentTab === 0" class="course-list">
        <view class="course-item" v-for="(item, index) in allCourses" :key="index">
          <view class="course-info">
            <text class="course-name">{{item.name}}</text>
            <text class="course-time">时间: {{item.time}}</text>
            <text class="course-students">学员: {{item.students}}人</text>
          </view>
          <view class="course-status">
            <text :class="['status-text', item.status]">{{getStatusText(item.status)}}</text>
          </view>
        </view>
      </view>

      <!-- 待上课 -->
      <view v-if="currentTab === 1" class="course-list">
        <view class="course-item" v-for="(item, index) in upcomingCourses" :key="index">
          <view class="course-info">
            <text class="course-name">{{item.name}}</text>
            <text class="course-time">时间: {{item.time}}</text>
            <text class="course-students">学员: {{item.students}}人</text>
          </view>
          <button class="action-btn" @click="startCourse(index)">开始上课</button>
        </view>
      </view>

      <!-- 已完成 -->
      <view v-if="currentTab === 2" class="course-list">
        <view class="course-item" v-for="(item, index) in completedCourses" :key="index">
          <view class="course-info">
            <text class="course-name">{{item.name}}</text>
            <text class="course-time">时间: {{item.time}}</text>
            <text class="course-students">学员: {{item.students}}人</text>
          </view>
          <button class="action-btn" @click="viewDetails(index)">查看详情</button>
        </view>
      </view>
    </view>

    <!-- 使用自定义tabbar组件 -->
    <custom-tabbar 
      :user-role="'coach'" 
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
      currentTab: 0,
      tabs: ['全部', '待上课', '已完成'],
      allCourses: [
        {
          name: "乒乓球基础班",
          time: "周三 16:00-17:00",
          students: 5,
          status: "upcoming"
        },
        {
          name: "乒乓球进阶班",
          time: "周五 18:00-19:00",
          students: 8,
          status: "completed"
        },
        {
          name: "一对一私教",
          time: "周六 10:00-11:00",
          students: 1,
          status: "upcoming"
        }
      ]
    };
  },
  computed: {
    upcomingCourses() {
      return this.allCourses.filter(course => course.status === 'upcoming');
    },
    completedCourses() {
      return this.allCourses.filter(course => course.status === 'completed');
    }
  },
  methods: {
    switchTab(index) {
      this.currentTab = index;
    },
    getStatusText(status) {
      switch(status) {
        case 'upcoming':
          return '待上课';
        case 'completed':
          return '已完成';
        default:
          return '未知';
      }
    },
    startCourse(index) {
      uni.showToast({
        title: '课程已开始',
        icon: 'success'
      });
    },
    viewDetails(index) {
      uni.showToast({
        title: '查看课程详情',
        icon: 'none'
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

.tab-header {
  display: flex;
  margin-bottom: 20px;
  border-bottom: 1px solid #eee;
}

.tab-item {
  flex: 1;
  text-align: center;
  padding: 15px 0;
  font-size: 16px;
  color: #666;
  position: relative;

  &.active {
    color: #007AFF;
    font-weight: bold;

    &:after {
      content: '';
      position: absolute;
      bottom: 0;
      left: 25%;
      width: 50%;
      height: 3px;
      background-color: #007AFF;
    }
  }
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
  flex: 1;
  display: flex;
  flex-direction: column;
}

.course-name {
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 5px;
}

.course-time, .course-students {
  font-size: 14px;
  color: #666;
  margin-bottom: 3px;
}

.course-status {
  width: 100px;
  text-align: center;
}

.status-text {
  padding: 5px 10px;
  border-radius: 4px;
  font-size: 14px;

  &.upcoming {
    background-color: #e6f7ff;
    color: #1890ff;
  }

  &.completed {
    background-color: #f6ffed;
    color: #52c41a;
  }
}

.action-btn {
  background-color: #007AFF;
  color: white;
  border: none;
  padding: 8px 15px;
  border-radius: 4px;
  font-size: 14px;
}
</style>
