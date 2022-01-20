<template>
	<view>
		<view class="mask" :hidden="ishide"></view>
		<view class="maskhint" :hidden="ishide">{{ maskhint }}</view>
		<!-- 计时 -->
		<view class="time">{{ game_time }}</view>
		<view class="score">分数：{{ game_score }}</view>
		<canvas
			:style="'width:' + width + 'px; height:' + height + 'px;'"
			canvas-id="firstCanvas"
			id="firstCanvas"
			disable-scroll="true"
			@touchstart="touchstart"
			@touchmove="touchmove"
			@touchend="touchend"
		></canvas>
	</view>
</template>

<script>
	var add_time;
export default {
	data() {
		return {
			width: '',
			height: '',
			imgHeight: 235,
			imgWidth: 322,
			imgsrc: '../../static/money//yby.png',
			imggroup: ['../../static/money//wsy.png', '../../static/money//yby.png', '../../static/money//esy.png'],
			positionx: 0,
			positiony: 0,
			canx: 0,
			cany: 0,
			ctx: '',
			dataimg: '',
			imgmodel: '',
			cy: '',
			game_time: '0.0',
			game_score: 0,
			ishide: false,
			maskhint:'点击虚拟纸币向上拉，速度越快分数越高' 
		};
	},
	onLoad() {
		let that = this;
		uni.getSystemInfo({
			success: function(res) {
				console.log(res);
				that.width = res.windowWidth;
				that.height = res.windowHeight;
			}
		});
	},
	onReady: function(e) {
		let that=this;
		var context = uni.createCanvasContext('firstCanvas');

		context.drawImage('../../static/money//gaibg3.png', 0, 0, this.width, this.height);
		context.drawImage(this.imggroup[0], 130, 212, 150, 290);
		context.drawImage(this.imggroup[1], 125, 207, 150, 290);
		context.drawImage(this.imggroup[2], 120, 200, 150, 290);
		context.drawImage(this.imgsrc, 110, 180, 150, 290);
		context.draw();

		// 倒计时
		setTimeout(function() {
			that.ishide = true;
			add_time = setInterval(function() {
				let gt = that.game_time;
				gt = Number(gt) + 0.1;
				that.game_time = gt.toFixed(1);
				if(gt>=15.0){
					that.ishide=false;
					that.maskhint='游戏结束';
					clearInterval(add_time);
					that.game_time='0.0';
				}
			}, 100);
		}, 2000);
	},
	methods: {
		canvasIdErrorCallback: function(e) {
			console.error(e.detail.errMsg);
		},
		touchstart(e) {
			let psix = e.changedTouches[0].x;
			let psiy = e.changedTouches[0].y;
			this.canx = psix;
			this.cany = psiy;
		},
		touchmove(e) {
			var context = uni.createCanvasContext('firstCanvas');
			let onepsix = this.canx;
			let onepsiy = this.cany;
			let twopsix = e.changedTouches[0].x;
			let twopsiy = e.changedTouches[0].y;

			//绘制背景图
			context.drawImage('../../static/money//gaibg3.png', 0, 0, this.width, this.height);
			context.drawImage(this.imggroup[0], 130, 212, 150, 290);
			context.drawImage(this.imggroup[1], 125, 207, 150, 290);
			context.drawImage(this.imggroup[2], 120, 200, 150, 290);

			context.drawImage(this.imgsrc, 110, twopsiy - 200, 150, 290);
			context.draw();
			this.cy = twopsiy - 200;
		},
		touchend(e) {
			let src = this.imggroup[0];
			var context = uni.createCanvasContext('firstCanvas');
			let onepsix = this.canx;
			let onepsiy = this.cany;
			let twopsix = e.changedTouches[0].x;
			let twopsiy = e.changedTouches[0].y;
			this.positionx = this.positionx + twopsix - onepsix;
			this.positiony = this.positiony + twopsiy - onepsiy;
			let cy = this.cy;
			let group = this.imggroup;
			if (cy <= -100) {
				// 数钱成功
				// console.log('成功');
				this.game_score=this.game_score+1;
				this.shuffle(group);
				this.imggroup = group;
				context.drawImage('../../static/money//gaibg3.png', 0, 0, this.width, this.height);
				context.drawImage(group[0], 130, 212, 150, 290);
				context.drawImage(group[1], 125, 207, 150, 290);
				context.drawImage(group[2], 120, 200, 150, 290);
				context.drawImage(this.imgsrc, 110, 180, 150, 290);
				context.draw();
			} else {
				// 数钱失败
				// console.log('失败');
				context.drawImage('../../static/money//gaibg3.png', 0, 0, this.width, this.height);
				context.drawImage(group[0], 130, 212, 150, 290);
				context.drawImage(group[1], 125, 207, 150, 290);
				context.drawImage(group[2], 120, 200, 150, 290);
				context.drawImage(this.imgsrc, 110, 180, 150, 290);
				context.draw();
			}
		},
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
	top: 1%;
	left: 45%;
	z-index: 9999;
}
.score{
	font-size: 45rpx;
	color: #fff;
	position: fixed;
	top: 1%;
	left: 3%;
	z-index: 100;
}
</style>
