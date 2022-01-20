<template>
	<view>
		<view class="mask" :hidden="ishide"></view>
		<view class="maskhint" :hidden="ishide">{{ maskhint }}</view>
		<!-- 计时 -->
		<view class="time">{{ game_time }}</view>
		<view class="score">分数：{{ game_score }}</view>
		<!-- 背景 -->
		<image class="bg" src="../../static/milktea/bg.png" mode="widthFix"></image>
		<!-- 游戏主体 -->
		<view class="main-game" :style="'grid-template-columns: repeat(' + con + ',' + conn + '%); grid-template-rows: repeat(' + row + ',' + rown + '%);'">
			<view :class="[item.fault ? 'fail' : '']" v-for="(item, index) in games" :key="index">
				<image class="game-img" :src="item.img" mode="widthFix" @click="thisone" :data-item="JSON.stringify(item)" :data-index="index"></image>
			</view>
		</view>
	</view>
</template>

<script>
	var add_time;
export default {
	data() {
		return {
			ishide: false,
			maskhint: '点击页面中唯一一张跟其他饮料不一样的照片即可得分',
			game_time: '0.0',
			game_score: 0,
			games: [],//游戏主体遍历的数组
			// list保存了99张错误的果汁图片
			list: [
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false },
				{ img: '../../static/milktea/gj.png', fault: false }
			],
			row: 2,
			rown: 45,
			con: 2,
			conn: 45
		};
	},
	onLoad() {
		let that = this;
		// 初始四张图布局，截取list中的三张错误图片并且拿出一张奶茶的图片，打乱顺序存入游戏使用的数组中
		let g = [];
		let a = { img: '../../static/milktea/nc.png', fault: false };
		let arr = this.list.slice(0, 3);
		g = arr;
		g.push(a);
		this.shuffle(g);
		this.games = g;

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
				if (gt >= 15.0) {
					that.ishide = false;
					that.maskhint = '游戏结束';
					clearInterval(add_time);
					that.game_time = '0.0';
				}
			}, 100);
		}, 2000);
	},
	methods: {
		// 图片点击方法
		thisone(e) {
			let that = this;
			let row = Number(this.row);
			let list = this.list;
			console.log(list);
			let item = JSON.parse(e.target.dataset.item);
			let index = e.target.dataset.index;
			let g = [];
			let arr = [];
			let a = { img: '../../static/milktea/nc.png', fault: false };
			if (item.img == '../../static/milktea/nc.png') {
				// 点对了，加分，增加难度，代码逻辑与初始布局逻辑一样，最大到10*10的布局
				this.game_score+=1;
				switch (row) {
					case 2:
						this.row = 3;
						this.con = 3;
						this.rown = 30;
						this.conn = 30;
						arr = this.list.slice(0, 8);
						break;
					case 3:
						this.row = 4;
						this.con = 4;
						this.rown = 22;
						this.conn = 22;
						arr = this.list.slice(0, 15);
						break;
					case 4:
						this.row = 5;
						this.con = 5;
						this.rown = 17;
						this.conn = 17;
						arr = this.list.slice(0, 24);
						break;
					case 5:
						this.row = 6;
						this.con = 6;
						this.rown = 15;
						this.conn = 15;
						arr = this.list.slice(0, 35);
						break;
					case 6:
						this.row = 7;
						this.con = 7;
						this.rown = 13;
						this.conn = 13;
						arr = this.list.slice(0, 48);
						break;
					case 7:
						this.row = 8;
						this.con = 8;
						this.rown = 11;
						this.conn = 11;
						arr = this.list.slice(0, 63);
						break;
					case 8:
						this.row = 9;
						this.con = 9;
						this.rown = 10;
						this.conn = 10;
						arr = this.list.slice(0, 80);
						break;
					default:
						this.row = 10;
						this.con = 10;
						this.rown = 9;
						this.conn = 9;
						arr = this.list.slice(0,99);
						break;
				}
				g = arr;
				g.push(a);
				console.log(g)
				this.shuffle(g);
				this.games = g;
			} else {
				// 点错了，游戏结束
				console.log('fail');
				this.games[index].fault = true;
				setTimeout(function(){
					that.ishide = false;
					that.maskhint = '游戏结束';
					clearInterval(add_time);
				},1000)
			}
		},
		// 打乱数组
		shuffle(arr) {
			var len = arr.length;
			for (var i = 0; i < len - 1; i++) {
				var index = parseInt(Math.random() * (len - i));
				var temp = arr[index];
				arr[index] = arr[len - i - 1];
				arr[len - i - 1] = temp;
			}
			return arr;
		}
	}
};
</script>

<style>
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
.bg {
	width: 100%;
	position: fixed;
	top: 0;
	left: 0;
	z-index: -1;
}
/* 游戏主体 */
.main-game {
	position: fixed;
	top: 20%;
	left: 3%;
	transform: translateX(-50%, -50%);
	width: 700rpx;
	height: 700rpx;
	border-radius: 20rpx;
	background-color: #fff;
	display: grid;
	place-content: center center;
	grid-auto-flow: column;
}
.game-img {
	width: 100%;
}
.fail {
	animation: shine 1s;
	z-index: 999;
	background-color: red;
	border-radius: 10rpx;
}
.fail image {
	opacity: 0.6;
	filter: alpha(opacity=60);
}
@keyframes shine {
	0% {
		background-color: rgba(0, 0, 0, 0);
	}
	20% {
		background-color: red;
	}
	40% {
		background-color: rgba(0, 0, 0, 0);
	}
	60% {
		background-color: red;
	}
	80% {
		background-color: rgba(0, 0, 0, 0);
	}
	100% {
		background-color: red;
	}
}
</style>
