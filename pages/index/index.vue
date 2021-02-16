<template>
	<view class="content">
		<view class="status_bar divBgColor">
			<!-- 这里是状态栏 -->
		</view>
		<view class="title divBgColor"> FGM Coin </view>
		<view class="wrap divBgColor">
			<u-swiper :list="imageList" height="300"></u-swiper>
		</view>

		<u-row gutter="8" class="noticeView divBgColor">
			<u-col span="2" class="rj_notice">
				<u-image width="80" height="40" src="@/static/images/rj_notice.png"></u-image>
			</u-col>
			<u-col span="9">
				<u-notice-bar type="none" mode="vertical" volume-size="0" color="white" :list="getNoticeList"
				 @click="clickNotice"></u-notice-bar>
			</u-col>
			<u-col span="1">
				<u-image width="30" height="20" src="@/static/images/rj_notice_menu.png"></u-image>
			</u-col>
		</u-row>

		<u-row gutter="16" class="divBgColor top3Coin">
			<u-col span="4" v-for="(value, key, i) in currencyList" v-if="i < 3">
				<view class="coin-title" >
					{{value.baseCurrency.toUpperCase()}}/{{value.quoteCurrency.toUpperCase()}}
					<view class="up-data-tag">{{formatNumber(value.rate, 2)}}</view>
				</view>
				<view class="coin-price">{{formatNumber(value.price, 4)}}</view>
			</u-col>
			
		</u-row>
		
		<view class="registerView" @click="goLogin()">
			<u-image class="registerImg" mode="widthFix" src="@/static/images/rj_home_register.png"></u-image>
		</view>
		
		<u-row gutter="16" class="divBgColor nagivateDiv">
			<u-col span="3" text-align="center">
				<img class="nagivateImg" src="@/static/images/ic_home_store.png"></img>
				<view class="nagivateTitle">Wallet</view>
			</u-col>
			<u-col span="3"  text-align="center">
				<img class="nagivateImg" src="@/static/images/ic_home_game.png"></img>
				<view class="nagivateTitle">Mapping</view>
			</u-col>
			<u-col span="3" text-align="center">
				<img class="nagivateImg" src="@/static/images/ic_home_scan.png"></img>
				<view class="nagivateTitle">Mine shop</view>
			</u-col>
			<u-col span="3"  text-align="center">
				<img class="nagivateImg" src="@/static/images/ic_home_trade_center.png"></img>
				<view class="nagivateTitle">Trading center</view>
			</u-col>
			
		</u-row>
		
		<view class="divBgColor coinDiv">
			<view style="height: 1px;"></view>
			<view class="title">Quotation</view>
			<u-row gutter="16" class="subTitle" >
				<u-col span="6">Name</u-col>
				<u-col span="6" text-align="right">Quote change</u-col>
			</u-row>
			<view v-for="(value, key, i) in currencyList">
				<u-row gutter="16" >
					<u-col span="4">
						<view class="coinTitlePre">{{value.baseCurrency.toUpperCase()}}</view>
						<view class="coinTitleSuf">/{{value.quoteCurrency.toUpperCase()}}</view>
					</u-col>
					<u-col span="4" class="coinPrice" text-align="center">
						$ {{formatNumber(value.price, 4)}}
					</u-col>
					<u-col span="4"  text-align="center"`>
						<view class="coinRate " v-bind:class="value.rate > 0? 'upRate' : 'downRate'">
						{{formatNumber(value.rate, 2)}} %
						</view>
					</u-col>
				</u-row>
				<u-line color="#213349" margin="30rpx" />
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'Hello',
				imageList: [],
				noticeList: [],
				currencyList:[]
			}
		},
		onLoad() {
			this.$u.get('https://fgm.ltd/fgm-api/home/bannerAndNoticeInfo', {}, {
				device: 2,
				version: 8
			}).then(res => {

				if (res.code === 0) {
					for (var i = 0; i < res.data.bannerList.length; i++) {
						var banner = res.data.bannerList[i];
						this.imageList.push({
							"image": banner.imageHref,
							"title": banner.title
						});
					}
					for (var i = 0; i < res.data.noticeList.length; i++) {
						var notice = res.data.noticeList[i];
						this.noticeList.push({
							"title": notice.title,
							"content": notice.content
						});
					}

				}
			});
			
			this.$u.get('https://fgm.ltd/fgm-api/market/marketList/1/20', {}, {
				device: 2,
				version: 8
			}).then(res => {
				console.log(res.data);
				if (res.code === 0) {
					this.currencyList = res.data;
			
				}
			});
		},
		computed: {
			getNoticeList: function() {
				var notices = [];
				for (var i = 0; i < this.noticeList.length; i++) {
					notices.push(this.noticeList[i].title);
				}
				return notices;
			}
		},
		methods: {
			formatNumber: function(number, decimals, dec_point, thousands_sep) {
			        
				number = (number + '').replace(/[^0-9+-Ee.]/g, '');
				var n = !isFinite(+number) ? 0 : +number,
		
					prec = !isFinite(+decimals) ? 0 : Math.abs(decimals),
					sep = (typeof thousands_sep === 'undefined') ? ',' : thousands_sep,
					dec = (typeof dec_point === 'undefined') ? '.' : dec_point,
					s = '',
					toFixedFix = function (n, prec) {
						var k = Math.pow(10, prec);
						return '' + Math.round(n * k) / k;
					};
				s = (prec ? toFixedFix(n, prec) : '' + Math.floor(n)).split('.');
				var re = /(-?\d+)(\d{3})/;
				while (re.test(s[0])) {
					s[0] = s[0].replace(re, "$1" + sep + "$2");
				}
		
				if ((s[1] || '').length < prec) {
					s[1] = s[1] || '';
					s[1] += new Array(prec - s[1].length + 1).join('0');
				}
				return s.join(dec);
			},
			clickNotice: function(index) {
				var content = this.noticeList[index].content;
				var title = this.noticeList[index].title;
				uni.navigateTo({
					url: './noticeDetail',
					success: function(res) {
						// 通过eventChannel向被打开页面传送数据
						uni.setStorageSync('indexNoticeContent', {
							title: title,
							content: content
						});
					}
				})
			}, 
			goLogin: function() {
				uni.navigateTo({
					url:'../my/login',
					success:function(res) {
						
					}
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.content {
		padding: 10rpx;
		.upRate{
			color:#03AA90;
			background-color: #0E4A4D;
		}
		.downRate{
			color:#D14B64;
			background-color: #4C2D3F;
		}
		.divBgColor{
			background-color: #131F2F;
		}
		.title {
			color: white;
			font-weight: bold;
			text-align: center;
			justify-content: center;
		}

		.wrap {
			padding: 40rpx;
		}

		.noticeView {
			padding: 0 20rpx;

			.rj_notice {
				width: 0 80rpx;
			}
		}
		.coin-title{
			display: flex;
			margin-top: 15px;
			color: white;
			font-size: 5px;
		}
		.up-data-tag {
			background-color: $u-type-error;
			color: #FFFFFF;
			display: flex;
			align-items: center;
			padding: 4rpx 14rpx;
			border-radius: 50rpx;
			line-height: 1;
			margin-left: 8rpx ;
		}
		
		.coin-price{
			color:white;
			font-size: 16px;
			margin-bottom: 20rpx;
		}
		.registerView{
			.registerImg{
				margin-top: 10px;
			}
		}
		
		.top3Coin {
			border-radius:0 0 10px 10px;
		}
		
		.nagivateDiv{
			border-radius:10px;
			margin-top: 8px;

			.nagivateImg{
				color: white;
				height: 60rpx;
				margin-top: 5px;
			}
			.nagivateTitle{
				color: white;
				margin: 2px 0 10px 0;
				font-size: 2rpx;
				white-space: nowrap;
			}
		}
		.coinDiv{
			border-radius:10px;
			margin-top: 8px;
			.title{
				color: #2583FF;
				font-size: 40rpx;
				text-align: left;
				margin: 10px;	
			}
			.subTitle{
				margin: 10rpx;
				color: #83A1C5;
				font-size: 20rpx;
			}
			
			.coinTitlePre{
				display:inline;
				color:white;
				margin-left: 10rpx;
				font-size: 30rpx;
				font-weight: bold;
			}
			.coinTitleSuf{
				display:inline;
				color: #83A1C5;
				font-size: 20rpx;
			}
			.coinPrice{color: white;}
			.coinRate{
				padding: 4rpx 14rpx;
				border-radius: 10rpx;
				line-height: 1.5;
				width: 160rpx;
				margin-left: 50rpx;
			}
		}
		
	}
</style>
