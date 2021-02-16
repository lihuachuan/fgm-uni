<template>
	<view  >
		<view v-html="title" class="noticeTitle"></view>
		<view v-html="content" class="noticeContent"></view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title:'',
				content: ''
			}
		},
		onLoad() {
			let data = uni.getStorageSync('indexNoticeContent');
			this.content = data.content;
			this.title = data.title;
		},
		methods: {

		}
	}
</script>

<style scoped>
	.noticeTitle{
		color: white;
		padding: 20rpx;
		font-size: 1.2em;
	}
	.noticeContent {
		color: white;
		padding: 20rpx;

	}

	.noticeContent >>> img {
		width: 100% !important;

	}
</style>
