<script>
	export default {
		onLaunch: function() {
			uni.showShareMenu({
				menus: ["shareAppMessage", "shareTimeline"],
			});
			const times = uni.getStorageSync('times');
			if(!times && times !== 0) {
				uni.setStorageSync('times', 1);
			}
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style>
	/*每个页面公共css */
	page {
		height: 100%;
		background: #f5f5f5;
	}
</style>
