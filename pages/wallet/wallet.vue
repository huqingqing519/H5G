<template>
	<view>
		<view class="mask" :hidden="ishide"></view>
		<view class="maskhint" :hidden="ishide">{{ maskhint }}</view>
		<!-- 计时 -->
		<view class="time">{{ game_time }}</view>
		<!-- 背景 -->
		<image src="../../static/wallet/bg.png" class="bg" mode="widthFix"></image>
		<!-- 进度条 -->
		<progress class="progress" activeColor="#ffaa00" :percent="percent" stroke-width="25" border-radius="10" />
		<view class="ptxt">{{ percent }}%</view>
		<!-- 钱包君 -->
		<image :src="npc_img" mode="widthFix" class="npc"></image>
		<!-- 按钮 -->
		<image class="btn" src="../../static/wallet/btn.png" mode="widthFix" @click="clickBtn"></image>
	</view>
</template>

<script>
var add_time;
var check;
export default {
	data() {
		return {
			ishide: false,
			maskhint: '不断点击“立马搬砖”按钮拯救钱包君，进度条蓄满后即可完成挑战！',
			game_time: '0.0',
			game_score: 0,
			width: '',
			height: '',
			percent: 0, //进度条进度
			npc_img: '../../static/wallet/s1.png'
		};
	},
	onLoad() {
		let that = this;
		uni.getSystemInfo({
			success: function(res) {
				// console.log(res);
				that.width = res.windowWidth;
				that.height = res.windowHeight;
			}
		});
		// 倒计时
		setTimeout(function() {
			that.ishide = true;
			add_time = setInterval(function() {
				let gt = that.game_time;
				gt = Number(gt) + 0.1;
				that.game_time = gt.toFixed(1);
				that.checkNpc();
			}, 100);
		}, 2000);

		check = setInterval(function() {
			if (that.percent > 0 && that.percent < 100) {
				that.percent -= 1;
			}
			if (that.percent < 0) {
				that.percent = 0;
			}
			if (that.percent == 100) {
				that.percent = 100;
			}
			that.checkNpc();
		}, 100);
	},
	methods: {
		// 点击按钮进行加分
		clickBtn() {
			this.percent += 3;
			this.checkNpc();
		},
		// 根据进度条进度切换npc图片
		checkNpc() {
			let p = Number(this.percent);
			if (p < 25) {
				this.npc_img = '../../static/wallet/s1.png';
			}
			if (p > 25 && p < 50) {
				this.npc_img = '../../static/wallet/s2.png';
			}
			if (p > 50 && p < 75) {
				this.npc_img = '../../static/wallet/s3.png';
			}
			if (p > 75) {
				this.npc_img = '../../static/wallet/s4.png';
				if (p == 100) {
					this.ishide = false;
					let a = this.game_time;
					this.maskhint = '游戏结束！用时' + a + '秒!';
					clearInterval(add_time);
					clearInterval(check);
				}
			}
		}
	}
};
</script>

<style>
page {
	font-family: '微软雅黑';
}
>>> .uni-progress-bar {
	padding: 5upx;
	border-radius: 20px;
}

>>> .uni-progress-inner-bar {
	border-radius: 15px;
}
/**蒙版*/
.mask {
	width: 100%;
	height: 100vh;
	position: fixed;
	top: 0;
	opacity: 1;
	background-color: rgba(0, 0, 0, 0.7);
	z-index: 700;
}
.maskhint {
	width: 80%;
	text-align: center;
	font-size: 50rpx;
	color: #fff;
	z-index: 800;
	position: absolute;
	top: 30%;
	left: 11%;
	animation: txt 2s infinite;
}
@keyframes txt {
	0% {
		transform: scale3d(0.9, 0.9, 0.9);
	}

	50% {
		transform: scale3d(1.1, 1.1, 1.1);
	}

	100% {
		transform: scale3d(0.9, 0.9, 0.9);
	}
}
.time {
	font-size: 50rpx;
	color: #fff;
	position: fixed;
	top: 5rpx;
	left: 45%;
	z-index: 100;
}
.score {
	font-size: 45rpx;
	color: #fff;
	position: fixed;
	top: 1%;
	left: 3%;
	z-index: 100;
}
/* 背景 */
.bg {
	width: 100%;
	position: fixed;
	top: 0;
	left: 0;
	z-index: -1;
}
/* 进度条 */
.progress {
	transform: rotate(270deg);
	border-radius: 40rpx;
	position: absolute;
	bottom: 37%;
	right: -33%;
	z-index: 99;
	width: 600rpx;
	height: 100rpx;
}
.ptxt {
	color: #000;
	font-size: 40rpx;
	position: fixed;
	bottom: 13%;
	right: 2%;
	z-index: 99;
}
/* 钱包君 */
.npc {
	width: 600rpx;
	position: fixed;
	top: 20%;
	left: 8%;
	transform: translateX(-50%, -50%);
}
/* 按钮 */
.btn {
	width: 400rpx;
	position: fixed;
	bottom: 9%;
	left: 23%;
}
</style>
