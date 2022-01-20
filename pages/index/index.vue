<template>
	<view class="content">
		<view class="title">h5小游戏集锦</view>
		<view class="main">
			<view class="item" @click="toNext" data-no="0">数钱游戏</view>
			<view class="item" @click="toNext" data-no="1">翻牌抢货</view>
			<view class="item" @click="toNext" data-no="2">南圆北饺大PK</view>
			<view class="item" @click="toNext" data-no="3">奶茶在哪里</view>
			<view class="item" @click="toNext" data-no="4">救救钱包君</view>
			<view class="item" @click="toNext" data-no="5">红包雨</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			title: 'Hello'
		};
	},
	onLoad() {},
	methods: {
		toNext(e) {
			let no = Number(e.target.dataset.no);
			switch (no) {
				case 0:
					uni.navigateTo({
						url: '../money/money'
					});
					break;
				case 1:
					uni.navigateTo({
						url: '../flop_card/flop_card'
					});
					break;
				case 2:
					uni.navigateTo({
						url: '../nybjPk/nybjPk'
					});
					break;
				case 3:
					uni.navigateTo({
						url: '../milk_tea/milk_tea'
					});
					break;
				case 4:
					uni.navigateTo({
						url: '../wallet/wallet'
					});
					break;
				default:
					uni.showToast({
						title: '还未完成，敬请期待...',
						icon: 'none'
					});
					break;
			}
		}
	}
};
</script>

<style>
page {
	font-family: 方正兰亭超细黑简体;
}
.title {
	margin: 20rpx auto 30rpx;
	text-align: center;
	font-size: 50rpx;
	color: #1a1a1a;
}
.main {
	width: 100%;
	font-size: 38rpx;
	text-align: center;
}
.item {
	width: 85%;
	height: 80rpx;
	line-height: 80rpx;
	margin: 0 auto;
	border-bottom: 1rpx solid #efefef;
}
</style>
