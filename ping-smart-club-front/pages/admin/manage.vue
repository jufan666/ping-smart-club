<template>
  <view class="container">
    <view class="header">系统管理</view>

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

      <!-- 用户管理 -->
      <view v-if="currentTab === 0" class="manage-content">
        <view class="search-bar">
          <input 
            class="search-input" 
            placeholder="搜索用户" 
            v-model="searchKeyword"
          />
          <button class="search-btn">搜索</button>
        </view>

        <view class="user-list">
          <view class="user-item" v-for="(user, index) in filteredUsers" :key="index">
            <view class="user-info">
              <image class="user-avatar" :src="user.avatar" mode="aspectFill"></image>
              <view class="user-details">
                <text class="user-name">{{user.name}}</text>
                <text class="user-role">{{user.role}}</text>
              </view>
            </view>
            <view class="user-actions">
              <button class="action-btn" @click="editUser(index)">编辑</button>
              <button class="action-btn delete" @click="deleteUser(index)">删除</button>
            </view>
          </view>
        </view>
      </view>

      <!-- 课程管理 -->
      <view v-if="currentTab === 1" class="manage-content">
        <button class="add-btn" @click="addCourse">+ 添加课程</button>

        <view class="course-list">
          <view class="course-item" v-for="(course, index) in courses" :key="index">
            <view class="course-info">
              <text class="course-name">{{course.name}}</text>
              <text class="course-coach">教练: {{course.coach}}</text>
              <text class="course-time">时间: {{course.time}}</text>
            </view>
            <view class="course-actions">
              <button class="action-btn" @click="editCourse(index)">编辑</button>
              <button class="action-btn delete" @click="deleteCourse(index)">删除</button>
            </view>
          </view>
        </view>
      </view>

      <!-- 系统设置 -->
      <view v-if="currentTab === 2" class="manage-content">
        <view class="settings-group">
          <text class="group-title">基础设置</text>
          <view class="setting-item">
            <text class="setting-label">系统名称</text>
            <input class="setting-input" v-model="settings.systemName" />
          </view>
          <view class="setting-item">
            <text class="setting-label">系统版本</text>
            <input class="setting-input" v-model="settings.version" />
          </view>
        </view>

        <view class="settings-group">
          <text class="group-title">通知设置</text>
          <view class="setting-item">
            <text class="setting-label">新用户注册通知</text>
            <switch :checked="settings.newUserNotification" @change="toggleSetting('newUserNotification')" />
          </view>
          <view class="setting-item">
            <text class="setting-label">课程预约通知</text>
            <switch :checked="settings.courseBookingNotification" @change="toggleSetting('courseBookingNotification')" />
          </view>
        </view>

        <button class="save-btn" @click="saveSettings">保存设置</button>
      </view>
    </view>

    <!-- 使用自定义tabbar组件 -->
    <custom-tabbar 
      :user-role="'admin'" 
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
      tabs: ['用户管理', '课程管理', '系统设置'],
      searchKeyword: '',
      users: [
        {
          name: '张三',
          role: '教练',
          avatar: '/static/images/default-avatar.png'
        },
        {
          name: '李四',
          role: '学员家长',
          avatar: '/static/images/default-avatar.png'
        },
        {
          name: '王五',
          role: '管理员',
          avatar: '/static/images/default-avatar.png'
        }
      ],
      courses: [
        {
          name: '乒乓球基础班',
          coach: '张教练',
          time: '周三 16:00-17:00'
        },
        {
          name: '乒乓球进阶班',
          coach: '李教练',
          time: '周五 18:00-19:00'
        }
      ],
      settings: {
        systemName: '乒乓球培训系统',
        version: '2.1.0',
        newUserNotification: true,
        courseBookingNotification: true
      }
    };
  },
  computed: {
    filteredUsers() {
      if (!this.searchKeyword) return this.users;
      return this.users.filter(user => 
        user.name.includes(this.searchKeyword) || 
        user.role.includes(this.searchKeyword)
      );
    }
  },
  methods: {
    switchTab(index) {
      this.currentTab = index;
    },
    editUser(index) {
      uni.showToast({
        title: '编辑用户功能开发中',
        icon: 'none'
      });
    },
    deleteUser(index) {
      uni.showModal({
        title: '提示',
        content: '确定要删除该用户吗？',
        success: (res) => {
          if (res.confirm) {
            this.users.splice(index, 1);
            uni.showToast({
              title: '删除成功',
              icon: 'success'
            });
          }
        }
      });
    },
    addCourse() {
      uni.showToast({
        title: '添加课程功能开发中',
        icon: 'none'
      });
    },
    editCourse(index) {
      uni.showToast({
        title: '编辑课程功能开发中',
        icon: 'none'
      });
    },
    deleteCourse(index) {
      uni.showModal({
        title: '提示',
        content: '确定要删除该课程吗？',
        success: (res) => {
          if (res.confirm) {
            this.courses.splice(index, 1);
            uni.showToast({
              title: '删除成功',
              icon: 'success'
            });
          }
        }
      });
    },
    toggleSetting(key) {
      this.settings[key] = !this.settings[key];
    },
    saveSettings() {
      uni.showToast({
        title: '设置已保存',
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

.manage-content {
  margin-top: 20px;
}

.search-bar {
  display: flex;
  margin-bottom: 20px;
}

.search-input {
  flex: 1;
  height: 40px;
  padding: 0 15px;
  border: 1px solid #ddd;
  border-radius: 4px 0 0 4px;
  font-size: 14px;
}

.search-btn {
  height: 40px;
  padding: 0 15px;
  background-color: #007AFF;
  color: white;
  border: none;
  border-radius: 0 4px 4px 0;
  font-size: 14px;
}

.user-list, .course-list {
  margin-top: 20px;
}

.user-item, .course-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px;
  background-color: #f8f8f8;
  border-radius: 8px;
  margin-bottom: 15px;
}

.user-info {
  display: flex;
  align-items: center;
}

.user-avatar {
  width: 50px;
  height: 50px;
  border-radius: 25px;
  margin-right: 15px;
}

.user-details {
  display: flex;
  flex-direction: column;
}

.user-name {
  font-size: 16px;
  font-weight: bold;
  margin-bottom: 5px;
}

.user-role {
  font-size: 14px;
  color: #666;
}

.course-info {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.course-name {
  font-size: 16px;
  font-weight: bold;
  margin-bottom: 5px;
}

.course-coach, .course-time {
  font-size: 14px;
  color: #666;
  margin-bottom: 3px;
}

.user-actions, .course-actions {
  display: flex;
}

.action-btn {
  margin-left: 10px;
  padding: 6px 12px;
  background-color: #007AFF;
  color: white;
  border: none;
  border-radius: 4px;
  font-size: 14px;

  &.delete {
    background-color: #ff4d4f;
  }
}

.add-btn {
  width: 100%;
  padding: 12px 0;
  background-color: #007AFF;
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 16px;
  margin-bottom: 20px;
}

.settings-group {
  margin-bottom: 30px;
}

.group-title {
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 15px;
  display: block;
}

.setting-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px 0;
  border-bottom: 1px solid #f0f0f0;
}

.setting-label {
  font-size: 16px;
}

.setting-input {
  width: 200px;
  height: 36px;
  padding: 0 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
}

.save-btn {
  width: 100%;
  padding: 12px 0;
  background-color: #007AFF;
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 16px;
}
</style>
