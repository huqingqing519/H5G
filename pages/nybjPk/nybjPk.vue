<template>
	<view>
		<view class="mask" :hidden="ishide"></view>
		<view class="maskhint" :hidden="ishide">{{ maskhint }}</view>
		<!-- 计时 -->
		<view class="time">{{ game_time }}</view>
		<view class="score">分数：{{ game_score }}</view>
		<!-- 背景 -->
		<image src="../../static/pk/bg.jpg" :style="'width:' + width + 'px; height:' + height + 'px;'" class="bg"></image>
		<!-- npc列表 -->
		<view class="npcView">
			<image class="npc-item" :class="'npc'+index+' '+[item.fault?'fault':'']" v-for="(item,index) in npclist" :key="index" :src="item.img" mode="widthFix"></image>
		</view>
		<!-- 背景npc -->
		<image class="ny" src="../../static/pk/ny.png" mode="widthFix"></image>
		<image class="bj" src="../../static/pk/bj.png" mode="widthFix"></image>
		<!-- 按钮 -->
		<image class="nbtn" src="../../static/pk/nbtn.png" @click="choose" data-fx="n" mode="widthFix"></image>
		<image class="bbtn" src="../../static/pk/bbtn.png" @click="choose" data-fx="b" mode="widthFix"></image>
	</view>
</template>

<script>
	var add_time;
	export default {
		data() {
			return {
				width:'',
				height:'',
				npclist:[
					{img:'../../static/pk/ny.png',fault:false},
					{img:'../../static/pk/bj.png',fault:false},
					{img:'../../static/pk/ny.png',fault:false},
					{img:'../../static/pk/bj.png',fault:false},
					{img:'../../static/pk/ny.png',fault:false},
					{img:'../../static/pk/bj.png',fault:false},
					{img:'../../static/pk/ny.png',fault:false},
					{img:'../../static/pk/bj.png',fault:false},
				],
				ishide:false,
				maskhint:'最前方为汤圆则点击左边按钮，最前方为饺子则点击右边按钮',
				game_time:'0.0',
				game_score:0,
			}
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
			let g=this.npclist;
			this.shuffle(g);
			this.npclist=g;
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
			// 选择
			choose(e){
				let fx=e.target.dataset.fx;
				let that=this;
				let answer;
				if(fx=='n'){
					answer='../../static/pk/ny.png'
				}else{
					answer='../../static/pk/bj.png'
				}
				let npclist=this.npclist;
				
				if(npclist[7].img==answer){  
					// 答案正确
					let num=Math.floor(Math.random()*(2-1+1)+1);
					console.log(num)
					if(num==1){
						answer='../../static/pk/ny.png'
					}else{
						answer='../../static/pk/bj.png'
					}
					npclist.splice(npclist.length-1,1);
					let a={img:answer,fault:false};
					npclist.unshift(a);
					this.game_score=this.game_score+1;
				}else{
					// 答案错误
					npclist[7].fault=true;
					setTimeout(function(){
						that.ishide=false;
						that.maskhint='游戏结束';
						clearInterval(add_time);
					},1500)
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
	}
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
.score{
	font-size: 45rpx;
	color: #fff;
	position: fixed;
	top: 1%;
	left: 3%;
	z-index: 100;
}
/* 游戏主体 */
.bg{
	position: fixed;
	top: 0;
	left: 0;
	z-index: -1;
}
.npcView{
	width: 200rpx;
	height: 700rpx;
	display: flex;
	flex-direction: column;
	align-items: center;
	justify-content: center;
	position: relative;
	margin: 0 auto;
	top: 300rpx;
}
.npc-item{
	position: absolute;
	transform: translateX(-50%,-50%);
}
.npc7{
	width: 260rpx;
	bottom: 0;
}
.npc6{
	width: 230rpx;
	bottom: 90rpx;
}
.npc5{
	width: 200rpx;
	bottom: 180rpx;
}
.npc4{
	width: 170rpx;
	bottom: 240rpx;
}
.npc3{
	width: 160rpx;
	bottom: 290rpx;
}
.npc2{
	width: 150rpx;
	bottom: 330rpx;
}
.npc1{
	width: 140rpx;
	bottom: 380rpx;
}
.npc0{
	width: 130rpx;
	bottom: 420rpx;
}
.fault{
	animation: shine 1.5s;
}
@keyframes shine{
	0%{
		background-color: red;
	}
	20%{
		background-color: rgba(0,0,0,0);
	}
	40%{
		background-color: red;
	}
	60%{
		background-color: rgba(0,0,0,0);
	}
	80%{
		background-color: red;
	}
	100%{
		background-color: rgba(0,0,0,0);
	}
}
/* 背景npc */
.ny,.bj{
	width: 180rpx;
	position: fixed;
	bottom: 35%;
}
.ny{
	left: 50rpx;
}
.bj{
	right: 50rpx;
}
/* 按钮 */
.nbtn,.bbtn{
	width: 200rpx;
	position: fixed;
	bottom: 10%;
}
.nbtn{
	left: 60rpx;
}
.bbtn{
	right: 60rpx;
}
</style>
