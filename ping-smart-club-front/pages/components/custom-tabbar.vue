<template>
	<view class="custom-tabbar" v-if="tabList.length > 0">
		<view 
			v-for="(item, index) in tabList" 
			:key="index" 
			class="tabbar-item" 
			@click="switchTab(item, index)"
		>
			<text 
				class="tabbar-icon" 
				:style="{color: currentTab === index ? selectedColor : color}"
			>{{getIcon(item.text)}}</text>
			<text 
				class="tabbar-text" 
				:style="{color: currentTab === index ? selectedColor : color}"
			>{{item.text}}</text>
		</view>
	</view>
</template>

<script>
export default {
	name: 'custom-tabbar',
	props: {
		// 用户身份：'parent'家长, 'coach'教练, 'admin'管理员
		userRole: {
			type: String,
			default: 'parent'
		},
		// 当前选中的tab索引
		current: {
			type: Number,
			default: 0
		},
		// 未选中时的颜色
		color: {
			type: String,
			default: '#999999'
		},
		// 选中时的颜色
		selectedColor: {
			type: String,
			default: '#007AFF'
		}
	},
	data() {
		return {
			currentTab: 0,
			// 家长tabbar配置
			parentTabList: [
				{
					pagePath: "/pages/club/club",
					text: "首页",
					iconPath: "/static/images/home.png",
					selectedIconPath: "/static/images/home-active.png"
				},
				{
					pagePath: "/pages/parent/booking",
					text: "约课",
					iconPath: "/static/images/booking.png",
					selectedIconPath: "/static/images/booking-active.png"
				},
				{
					pagePath: "/pages/parent/my",
					text: "我的",
					iconPath: "/static/images/my.png",
					selectedIconPath: "/static/images/my-active.png"
				}
			],
			// 教练tabbar配置
			coachTabList: [
				{
					pagePath: "/pages/club/club",
					text: "主页",
					iconPath: "/static/images/home.png",
					selectedIconPath: "/static/images/home-active.png"
				},
				{
					pagePath: "/pages/coach/course",
					text: "课程",
					iconPath: "/static/images/course.png",
					selectedIconPath: "/static/images/course-active.png"
				},
				{
					pagePath: "/pages/coach/my",
					text: "我的",
					iconPath: "/static/images/my.png",
					selectedIconPath: "/static/images/my-active.png"
				}
			],
			// 管理员tabbar配置
			adminTabList: [
				{
					pagePath: "/pages/club/club",
					text: "首页",
					iconPath: "/static/images/home.png",
					selectedIconPath: "/static/images/home-active.png"
				},
				{
					pagePath: "/pages/admin/manage",
					text: "管理",
					iconPath: "/static/images/manage.png",
					selectedIconPath: "/static/images/manage-active.png"
				},
				{
					pagePath: "/pages/admin/my",
					text: "我的",
					iconPath: "/static/images/my.png",
					selectedIconPath: "/static/images/my-active.png"
				}
			]
		};
	},
	computed: {
		// 根据用户角色获取对应的tabbar列表
		tabList() {
			switch(this.userRole) {
				case 'coach':
					return this.coachTabList;
				case 'admin':
					return this.adminTabList;
				case 'parent':
				default:
					return this.parentTabList;
			}
		}
	},
	watch: {
		// 监听current变化
		current(newVal) {
			this.currentTab = newVal;
		}
	},
	mounted() {
		this.currentTab = this.current;
	},
	methods: {
		// 获取图标
		getIcon(text) {
			const icons = {
				"首页": "🏠",
				"主页": "🏠",
				"约课": "📅",
				"课程": "📚",
				"管理": "⚙️",
				"我的": "👤"
			};
			return icons[text] || "📄";
		},
		// 切换tab
		switchTab(item, index) {
			this.currentTab = index;
			// 发出事件，通知父组件
			this.$emit('change', index);
			
			// 跳转页面
			try {
				// 使用switchTab跳转
				uni.switchTab({
					url: item.pagePath,
					success: () => {
						console.log('跳转成功:', item.pagePath);
					},
					fail: (err) => {
						console.error('跳转失败:', err);
						// 如果switchTab失败，尝试使用reLaunch
						uni.reLaunch({
							url: item.pagePath
						});
					}
				});
			} catch (error) {
				console.error('页面跳转错误:', error);
				// 最后的备选方案
				uni.redirectTo({
					url: item.pagePath
				});
			}
		}
	}
};
</script>

<style lang="scss" scoped>
.custom-tabbar {
	position: fixed;
	bottom: 0;
	left: 0;
	right: 0;
	height: 100rpx;
	background-color: #ffffff;
	display: flex;
	padding-bottom: env(safe-area-inset-bottom);
	box-shadow: 0 -1rpx 6rpx rgba(0, 0, 0, 0.1);
	z-index: 999;
}

.tabbar-item {
	flex: 1;
	display: flex;
	flex-direction: column;
	align-items: center;
	justify-content: center;
	padding: 10rpx 0;
}

.tabbar-icon {
	font-size: 48rpx;
	margin-bottom: 6rpx;
	line-height: 1;
}

.tabbar-text {
	font-size: 24rpx;
	line-height: 1;
}
</style>
