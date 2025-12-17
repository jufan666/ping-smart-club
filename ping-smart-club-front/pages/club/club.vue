<template>
  <view class="container">
    <!-- 俱乐部横幅 -->
    <view class="banner">
      <image class="banner-image" src="/static/logo.png" mode="aspectFit"></image>
      <view class="banner-text">
        <text class="club-name">三众乒乓球俱乐部</text>
        <text class="club-slogan">专业培训，成就未来冠军</text>
      </view>
    </view>

    <!-- 俱乐部简介 -->
    <view class="section">
      <view class="section-title">
        <text class="title-text">俱乐部简介</text>
      </view>
      <view class="intro-content">
        <text class="intro-text">三众乒乓球俱乐部成立于1990年，是一家专注于青少年乒乓球培训的专业机构。我们拥有国际标准训练场地、先进的教学设备和经验丰富的教练团队，致力于为学员提供最专业的乒乓球培训服务。</text>
      </view>
    </view>

    <!-- 师资团队 -->
    <view class="section">
      <view class="section-title">
        <text class="title-text">师资团队</text>
      </view>
      <view class="team-grid">
        <view class="coach-card" v-for="(coach, index) in coaches" :key="index">
          <image class="coach-avatar" :src="coach.avatar" mode="aspectFill"></image>
          <view class="coach-info">
            <text class="coach-name">{{ coach.name }}</text>
            <text class="coach-title">{{ coach.title }}</text>
            <text class="coach-desc">{{ coach.description }}</text>
          </view>
        </view>
      </view>
    </view>

    <!-- 俱乐部历史 -->
    <view class="section">
      <view class="section-title">
        <text class="title-text">俱乐部历史</text>
      </view>
      <view class="timeline">
        <view class="timeline-item" v-for="(event, index) in history" :key="index">
          <view class="timeline-dot"></view>
          <view class="timeline-content">
            <text class="timeline-year">{{ event.year }}</text>
            <text class="timeline-text">{{ event.event }}</text>
          </view>
        </view>
      </view>
    </view>

    <!-- 俱乐部荣誉 -->
    <view class="section">
      <view class="section-title">
        <text class="title-text">俱乐部荣誉</text>
      </view>
      <view class="honor-grid">
        <view class="honor-card" v-for="(honor, index) in honors" :key="index">
          <view class="honor-icon">
            <text class="iconfont icon-trophy">{{ honor.icon }}</text>
          </view>
          <text class="honor-title">{{ honor.title }}</text>
          <text class="honor-year">{{ honor.year }}</text>
        </view>
      </view>
    </view>

    <!-- 优秀学员 -->
    <view class="section">
      <view class="section-title">
        <text class="title-text">优秀学员</text>
      </view>
      <view class="students-grid">
        <view class="student-card" v-for="(student, index) in students" :key="index">
          <image class="student-avatar" :src="student.avatar" mode="aspectFill"></image>
          <view class="student-info">
            <text class="student-name">{{ student.name }}</text>
            <text class="student-achievement">{{ student.achievement }}</text>
          </view>
        </view>
      </view>
    </view>

    <!-- 联系我们 -->
    <view class="section contact-section">
      <view class="section-title">
        <text class="title-text">联系我们</text>
      </view>
      <view class="contact-info">
        <view class="contact-item">
          <text class="contact-label">地址：</text>
          <text class="contact-value">北京市朝阳区体育中心路88号</text>
        </view>
        <view class="contact-item">
          <text class="contact-label">电话：</text>
          <text class="contact-value">010-12345678</text>
        </view>
        <view class="contact-item">
          <text class="contact-label">邮箱：</text>
          <text class="contact-value">info@pingpongclub.com</text>
        </view>
        <view class="contact-item">
          <text class="contact-label">营业时间：</text>
          <text class="contact-value">周一至周五 9:00-21:00，周末 8:00-22:00</text>
        </view>
      </view>
    </view>

    <!-- 使用自定义tabbar组件 -->
    <custom-tabbar
      :user-role="userRole"
      :current="0"
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
      userRole: '', // 将在onLoad中设置
      coaches: [
        {
          name: '安丰达',
          title: '总教练',
          description: '前国家队选手，拥有15年教学经验',
          avatar: '/static/images/coach_an.png'
        },
        {
          name: '鞠梵',
          title: '高级教练',
          description: '省队退役选手，擅长青少年启蒙训练',
          avatar: '/static/images/coach_ju.png'
        },
        {
          name: '许轩源',
          title: '中级教练',
          description: '体育大学乒乓球专业毕业，教学经验丰富',
          avatar: '/static/images/default-avatar.png'
        }
      ],
      history: [
        {
          year: '2010年',
          event: '智慧乒乓球俱乐部正式成立'
        },
        {
          year: '2012年',
          event: '首次参加市级比赛获得团体冠军'
        },
        {
          year: '2015年',
          event: '扩建训练场地，新增10张国际标准球台'
        },
        {
          year: '2018年',
          event: '被评为市级青少年乒乓球培训示范基地'
        },
        {
          year: '2021年',
          event: '学员首次获得全国青少年乒乓球比赛冠军'
        }
      ],
      honors: [
        {
          icon: '🏆',
          title: '市级青少年乒乓球培训示范基地',
          year: '2018年'
        },
        {
          icon: '🥇',
          title: '省级优秀乒乓球俱乐部',
          year: '2019年'
        },
        {
          icon: '🏅',
          title: '全国青少年乒乓球比赛优秀组织奖',
          year: '2020年'
        },
        {
          icon: '🎖️',
          title: '体育产业创新示范单位',
          year: '2022年'
        }
      ],
      students: [
        {
          name: '小明',
          achievement: '2022年全国青少年乒乓球比赛U12组冠军',
          avatar: '/static/images/default-avatar.png'
        },
        {
          name: '小红',
          achievement: '2021年省级青少年乒乓球比赛U10组亚军',
          avatar: '/static/images/default-avatar.png'
        },
        {
          name: '小华',
          achievement: '2023年市级青少年乒乓球比赛U14组冠军',
          avatar: '/static/images/default-avatar.png'
        },
        {
          name: '小丽',
          achievement: '2022年省级青少年乒乓球比赛U8组季军',
          avatar: '/static/images/default-avatar.png'
        }
      ]
    };
  },
  onLoad(options) {
    // 根据传入的角色参数设置用户角色
    try {
      if (options.role) {
        this.userRole = options.role;
        // 保存用户角色到本地存储
        uni.setStorageSync('userRole', options.role);
      } else {
        // 如果URL中没有角色参数，尝试从本地存储获取
        const savedRole = uni.getStorageSync('userRole');
        if (savedRole) {
          this.userRole = savedRole;
        } else {
          // 默认设置为家长角色
          this.userRole = 'parent';
          uni.setStorageSync('userRole', 'parent');
        }
      }
      
      console.log('当前用户角色:', this.userRole);
    } catch (error) {
      console.error('设置用户角色失败:', error);
      // 默认设置为家长角色
      this.userRole = 'parent';
    }
  },
  methods: {
    onTabChange(index) {
      console.log('当前选中的tab索引:', index);
    }
  }
};
</script>

<style lang="scss" scoped>
.container {
  padding-bottom: 120rpx; /* 留出底部tabbar的空间 */
}

/* 横幅部分 */
.banner {
  position: relative;
  height: 300rpx;
  background-color: #f0f8ff;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.banner-image {
  width: 150rpx;
  height: 150rpx;
  margin-right: 30rpx;
}

.banner-text {
  display: flex;
  flex-direction: column;
}

.club-name {
  font-size: 40rpx;
  font-weight: bold;
  color: #333;
  margin-bottom: 10rpx;
}

.club-slogan {
  font-size: 28rpx;
  color: #666;
}

/* 通用部分样式 */
.section {
  padding: 40rpx 30rpx;
  background-color: #fff;
  margin-bottom: 20rpx;
}

.section-title {
  margin-bottom: 30rpx;
  position: relative;
  padding-left: 20rpx;
}

.title-text {
  font-size: 36rpx;
  font-weight: bold;
  color: #333;
}

.section-title::before {
  content: '';
  position: absolute;
  left: 0;
  top: 10rpx;
  height: 36rpx;
  width: 8rpx;
  background-color: #007AFF;
  border-radius: 4rpx;
}

/* 俱乐部简介 */
.intro-content {
  line-height: 1.6;
}

.intro-text {
  font-size: 30rpx;
  color: #666;
}

/* 师资团队 */
.team-grid {
  display: flex;
  flex-direction: column;
  gap: 30rpx;
}

.coach-card {
  display: flex;
  background-color: #f8f8f8;
  border-radius: 16rpx;
  padding: 30rpx;
  box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.05);
}

.coach-avatar {
  width: 120rpx;
  height: 120rpx;
  border-radius: 60rpx;
  margin-right: 30rpx;
}

.coach-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.coach-name {
  font-size: 32rpx;
  font-weight: bold;
  color: #333;
  margin-bottom: 10rpx;
}

.coach-title {
  font-size: 28rpx;
  color: #007AFF;
  margin-bottom: 10rpx;
}

.coach-desc {
  font-size: 26rpx;
  color: #666;
}

/* 俱乐部历史 */
.timeline {
  position: relative;
  padding-left: 30rpx;
}

.timeline::before {
  content: '';
  position: absolute;
  left: 15rpx;
  top: 0;
  height: 100%;
  width: 4rpx;
  background-color: #e0e0e0;
}

.timeline-item {
  position: relative;
  padding-bottom: 40rpx;
}

.timeline-dot {
  position: absolute;
  left: -22rpx;
  top: 10rpx;
  width: 20rpx;
  height: 20rpx;
  border-radius: 10rpx;
  background-color: #007AFF;
  border: 4rpx solid #fff;
  box-shadow: 0 0 0 4rpx #e0e0e0;
}

.timeline-content {
  padding-left: 20rpx;
}

.timeline-year {
  font-size: 30rpx;
  font-weight: bold;
  color: #007AFF;
  margin-bottom: 10rpx;
  display: block;
}

.timeline-text {
  font-size: 28rpx;
  color: #333;
  line-height: 1.5;
}

/* 俱乐部荣誉 */
.honor-grid {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}

.honor-card {
  width: 48%;
  background-color: #f8f8f8;
  border-radius: 16rpx;
  padding: 30rpx;
  margin-bottom: 30rpx;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.05);
}

.honor-icon {
  font-size: 60rpx;
  margin-bottom: 20rpx;
}

.honor-title {
  font-size: 28rpx;
  font-weight: bold;
  color: #333;
  margin-bottom: 10rpx;
}

.honor-year {
  font-size: 24rpx;
  color: #666;
}

/* 优秀学员 */
.students-grid {
  display: flex;
  flex-direction: column;
  gap: 30rpx;
}

.student-card {
  display: flex;
  background-color: #f8f8f8;
  border-radius: 16rpx;
  padding: 30rpx;
  box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.05);
}

.student-avatar {
  width: 100rpx;
  height: 100rpx;
  border-radius: 50rpx;
  margin-right: 30rpx;
}

.student-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.student-name {
  font-size: 32rpx;
  font-weight: bold;
  color: #333;
  margin-bottom: 10rpx;
}

.student-achievement {
  font-size: 26rpx;
  color: #666;
  line-height: 1.5;
}

/* 联系我们 */
.contact-section {
  margin-bottom: 40rpx;
}

.contact-info {
  line-height: 1.6;
}

.contact-item {
  margin-bottom: 20rpx;
  display: flex;
}

.contact-label {
  font-size: 30rpx;
  color: #333;
  width: 160rpx;
}

.contact-value {
  font-size: 30rpx;
  color: #666;
  flex: 1;
}
</style>
