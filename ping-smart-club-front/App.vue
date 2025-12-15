<script>
	export default {
		data() {
			return {
				// 系统信息缓存
				systemInfo: null
			}
		},
		onLaunch: function() {
			console.log('App Launch')

			// 捕获全局错误
			uni.onError((error) => {
				console.error('全局错误:', error)
			})

			// 捕获未处理的Promise错误
			uni.onUnhandledRejection(({reason}) => {
				console.error('未处理的Promise错误:', reason)
			})
			
			// 获取系统信息
			this.getSystemInfo()
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		},
		methods: {
			// 获取系统信息，兼容新旧API
			getSystemInfo() {
				try {
					// 优先使用新API
					if (uni.getWindowInfo && uni.getDeviceInfo && uni.getAppBaseInfo) {
						const windowInfo = uni.getWindowInfo()
						const deviceInfo = uni.getDeviceInfo()
						const appBaseInfo = uni.getAppBaseInfo()
						
						// 合并信息
						this.systemInfo = {
							...windowInfo,
							...deviceInfo,
							...appBaseInfo
						}
						
						console.log('使用新API获取系统信息成功:', this.systemInfo)
					} else {
						// 回退到旧API
						this.systemInfo = uni.getSystemInfoSync()
						console.log('使用旧API获取系统信息成功:', this.systemInfo)
					}
				} catch (error) {
					console.error('获取系统信息失败:', error)
					// 设置默认值，防止后续调用出错
					this.systemInfo = {
						brand: 'unknown',
						model: 'unknown',
						screenWidth: 375,
						screenHeight: 667,
						SDKVersion: '2.0.0',
						system: 'unknown',
						platform: 'unknown',
						windowWidth: 375,
						windowHeight: 667
					}
				}
			}
		}
	}
</script>

<style>
	/*每个页面公共css */
</style>
