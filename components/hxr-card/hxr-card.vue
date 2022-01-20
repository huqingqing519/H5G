<template>
	<view class="card">
		<block v-for="(item, index) in card" :key="index">
			<view
				:data-index="index"
				:data-item="item"
				@click="openCard"
				:class="'project item' + index + ' ' + (isMove ? 'ani' : '') + ' ' + (item.status == 1 ? 'flip' : item.status == 3 ? 'passof' : '')"
			>
				<image class="front" src="../../static/card/card.png"></image>
				<view class="back"><image :src="item.img"></image></view>
			</view>
		</block>
	</view>
</template>

<script>
function runAsync(time) {
	return new Promise(function(resolve, reject) {
		const timer = setTimeout(function() {
			resolve();
			clearTimeout(timer);
		}, time);
	});
}

export default {
	data() {
		return {
			move: '',
			isMove: true
		};
	},

	components: {},
	props: {
		// 九宫格卡片
		card: {
			type: Array,
			default: new Array()
		}
	},
	onLoad() {},
	methods: {
		start(callback) {
			const { card, mode } = this;
			let that = this;
			setTimeout(function() {
				that.isMove = false;
			}, 100)
			runAsync(100).then(() => {
				// 结束后回调
				if (typeof callback === 'function') {
					callback();
				}
			});
		},

		// 点击打开单个卡片，开奖
		openCard(event) {
			const { item, index } = event.currentTarget.dataset; // 触发父组件方法

			this.$emit('open', {
				detail: {
					item,
					index
				}
			});
		}
	}
};
</script>
<style>
.card {
	position: relative;
	overflow: hidden;
	margin: 30rpx auto 0;
	width: 700rpx;
	height: 100vh;
}
.card .project {
	position: absolute;
	float: left;
	width: 200rpx;
	height: 260rpx;
	display: -ms-flexbox;
	display: flex;
	-ms-flex-pack: center;
	justify-content: center;
	-ms-flex-align: center;
	align-items: center;
	transition: all 1s cubic-bezier(0.68, -0.22, 0.265, 1.22);
}
.card .project image {
	width: 200rpx;
	height: 260rpx;
}
.card .project .back,
.card .project .front {
	-webkit-backface-visibility: hidden;
	backface-visibility: hidden;
	transform-style: preserve-3d;
	transition: all 1s cubic-bezier(0.68, -0.22, 0.265, 1.22);
	position: absolute;
	left: 10rpx;
	top: 10rpx;
	width: 200rpx;
	height: 260rpx;
}
.card .project .front {
	transform: rotateY(0);
}
.card .project .back {
	transform: rotateY(180deg);
}
.card .project.flip .back {
	transform: rotateY(0);
}
.card .project.flip .front {
	transform: rotateY(180deg);
}
.card .ani {
	top: 340rpx !important;
	left: 250rpx !important;
}
.card .item0 {
	top: 0;
	left: 0;
}
.card .item1 {
	top: 0;
	left: 240rpx;
}
.card .item2 {
	top: 0;
	left: 480rpx;
}
.card .item3 {
	top: 290rpx;
	left: 0;
}
.card .item4 {
	top: 290rpx;
	left: 240rpx;
}
.card .item5 {
	top: 290rpx;
	left: 480rpx;
}
.card .item6 {
	top: 580rpx;
	left: 0;
}
.card .item7 {
	top: 580rpx;
	left: 240rpx;
}
.card .item8 {
	top: 580rpx;
	left: 480rpx;
}
.card .item9 {
	top: 870rpx;
	left: 0;
}
.card .item10 {
	top: 870rpx;
	left: 240rpx;
}
.card .item11 {
	top: 870rpx;
	left: 480rpx;
}
/* 消失 */
.passof .front {
	display: none;
}
.passof .back {
	display: none;
}
</style>
