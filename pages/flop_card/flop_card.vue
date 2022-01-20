<template>
	<view class="container">
		<view class="mask" :hidden="ishide"></view>
		<view class="maskhint" :hidden="ishide">{{maskhint}}</view>
		<!-- 计时 -->
		<view class="time">{{ game_time }}</view>
		<view class="maskbtn" :hidden="ishidebtn" @tap="start">我知道了</view>
		<view class="grid-card"><mycard id="mycard" :card="card" @open="openCard"></mycard></view>
	</view>
</template>

<script>
import mycard from '../../components/hxr-card/hxr-card.vue';
var add_time;
export default {
	data() {
		return {
			// 九宫格数据
			card: [
				{
					id: 1,
					prizeName: '10金币',
					img: '../../static/card/prize.png',
					status: 0 //   :0 反面 , 1 正面
				},
				{
					id: 2,
					prizeName: '10金币',
					img: '../../static/card/prize2.png',
					status: 0
				},
				{
					id: 3,
					prizeName: '100金币',
					img: '../../static/card/prize.png',
					status: 0
				},
				{
					id: 4,
					prizeName: '10金币',
					img: '../../static/card/prize2.png',
					status: 0
				},
				{
					id: 5,
					prizeName: '40金币',
					img: '../../static/card/prize.png',
					status: 0
				},
				{
					id: 6,
					prizeName: '20金币',
					img: '../../static/card/prize2.png',
					status: 0
				},
				{
					id: 7,
					prizeName: '50金币',
					img: '../../static/card/prize.png',
					status: 0
				},
				{
					id: 8,
					prizeName: '60金币',
					img: '../../static/card/prize2.png',
					status: 0
				},
				{
					id: 9,
					prizeName: '10金币',
					img: '../../static/card/prize.png',
					status: 0
				},
				{
					id: 10,
					prizeName: '20金币',
					img: '../../static/card/prize2.png',
					status: 0
				},
				{
					id: 11,
					prizeName: '50金币',
					img: '../../static/card/prize.png',
					status: 0
				},
				{
					id: 12,
					prizeName: '60金币',
					img: '../../static/card/prize2.png',
					status: 0
				}
			],
			ready: false, // 是否点击开始抽奖
			ishide:false,
			ishidebtn:false,
			allnum: 0,
			game_time: '0.0',
			maskhint:'点击牌背进行翻牌，翻到两张一样的牌即可消除，所有卡牌消除完毕后完成挑战。',
		};
	},

	components: {
		mycard
	},
	onShow: function(options) {
		let that=this;
		this.ready=false;
		this.ishide=false;
		this.allnum=0;
		// 每次都打乱顺序
		let list=this.card;
		this.shuffle(list);
		this.card=list;
	},

	methods: {
		shuffle(arr) {
			var len = arr.length;
			for (var i = 0; i < len - 1; i++) {
				var index = parseInt(Math.random() * (len - i));
				var temp = arr[index];
				arr[index] = arr[len - i - 1];
				arr[len - i - 1] = temp;
			}
			return arr;
		},
		start() {
			let that=this;
			this.ishide=true;
			this.ishidebtn=true;
			if (this.ready) {
				uni.showToast({
					title: `已经开启抽奖`,
					icon: 'none'
				});
				return;
			} // 触发组件开始方法
            add_time = setInterval(function() {
            	let gt = that.game_time;
            	gt = Number(gt) + 0.1;
            	that.game_time = gt.toFixed(1);
            }, 100);
			this.selectComponent('#mycard').start(() => {
				// 动画结束后可以点击
				this.ready = true;
			});
		},
		// 子组件触发，点击打开单个卡片奖品
		openCard(e) {
			const { item, index } = e.detail; // 动画没有结束，或已经点开

			if (!this.ready || item.status == 1) {
				return;
			}
			// 将数组中的单张牌改为翻开状态
			var obj = this.card[index];
			obj.status = 1;
			this.card[index] = obj;

			let fknum = 0;
			let allnum = 0;
			// 当翻开牌的数量大于一张时，遍历数组
			for (let item of this.card) {
				item.status == 1 ? (fknum = fknum + 1) : '';
				item.status == 3 ? (allnum = allnum + 1) : '';
			}
			let arr = [];
			// 有两张牌被打开的情况
			if (fknum > 1) {
				// 有两张牌同时打开时，把这两张牌的数据存到另一个数组里
				this.card.map(function(item, index) {
					if (item.status == 1) {
						item.index = index;
						arr.push(item);
					}
				});
				// 判断数组中的两个图片是否一致
				let a = arr[0];
				let b = arr[1];
				let that = this;
				if (a.img == b.img) {
					// 两张一样
					if (allnum == 10) {
						//这是最后一组消除成功
						setTimeout(function() {
							a.status = 3;
							b.status = 3;
							that.card[a.index] = a;
							that.card[b.index] = b;
							that.allnum = that.allnum + 2;
							clearInterval(add_time);
							that.ishide=false;
							that.maskhint='游戏结束,用时'+that.game_time+'秒';
						}, 1000);
					} else {
						//场上还有其他牌没有消除
						setTimeout(function() {
							a.status = 3;
							b.status = 3;
							that.card[a.index] = a;
							that.card[b.index] = b;
							that.allnum = that.allnum + 2;
						}, 1000);
					}
				} else {
					// 两张不一样
					setTimeout(function() {
						a.status = 0;
						b.status = 0;
						that.card[a.index] = a;
						that.card[b.index] = b;
					}, 1000);
				}
			}
		}
	}
};
</script>

<style>
page {
	background: #7bb0e1;
}

.container {
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
}

.grid-card {
	padding: 10px;
}

.sol-button {
	margin: 0 auto;
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
	font-size: 45rpx;
	color: #fff;
	z-index: 800;
	position: absolute;
	top: 30%;
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
.maskbtn{
	width: 50%;
	height: 70rpx;
	line-height: 70rpx;
	border-radius: 50rpx;
	background-color: rgba(170, 255, 255, 0.8);
	border: 1rpx solid rgb(170, 255, 255);
	text-align: center;
	font-size: 40rpx;
	color: #fff;
	z-index: 800;
	position: absolute;
	top: 50%;
}
</style>
