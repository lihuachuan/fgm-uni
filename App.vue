<script>
	export default {
		onLaunch: function() {
			console.log('App Launch')
			this.$u.get('https://fgm.ltd/fgm-api/home/bannerAndNoticeInfo', {}, {
				device: 2,
				version:7
			}).then(res => {
				console.log(res)
			});
		},
		onShow: function() {
			// console.log('App Show')
		},
		onHide: function() {
			// console.log('App Hide')
		}
	}
</script>

<style lang="scss">
	@import "uview-ui/index.scss";

	/*每个页面公共css */
	.status_bar {
		height: var(--status-bar-height);
		width: 100%;
	}

	body {
		width: 100%;
		height: 100%;
		background-color: #081724
	}
</style>
