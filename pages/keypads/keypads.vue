<template>
	<view>
        <page-head :title="title"></page-head>
		<!-- style="width: 432px; height: 504px;" -->
		<canvas
			canvas-id="keypads"
			@touchstart="touchStart"
			@touchmove="touchMove"
			@touchend="touchEnd"
			@touchcancel="touchCancel"
		/>
	</view>
</template>

<script>
	export default {
		onReady(e) {
			this.canvasContext = uni.createCanvasContext('keypads')
			// let imgWidth = uni.upx2px(432)
			// let imgHeight = uni.upx2px(504)
			let imgSrc = '/static/image/keypads/keypads.png'
			uni.downloadFile({
				url: imgSrc,
				success(res) {
					// context.drawImage(res.tempFilePath, 0, 0, imgWidth, imgHeight)
					// context.draw();
					// uni.data.keypadsRes = res;
					this.canvasRefresh(res)
				}
			})
		},
		onShow() {
			uni.getSystemInfo({
				success: function(res) {
					// console.log("屏幕宽度：" + res.screenWidth);
					// console.log("屏幕高度：" + res.screenHeight);
					// console.log("窗口宽度：" + res.windowWidth);
					// console.log("窗口高度：" + res.windowHeight);
					this.screenWidth = res.screenWidth;
					this.screenHeight = res.screenHeight;
				}
			})
		},
		canvasRefresh(res) {
			let width = uni.upx2px(432)
			let height = uni.upx2px(504)
			canvasContext.drawImage(keypadsRes.tempFilePath, 0, 0, width, height)
			canvasContext.draw();
		},
		data() {
			return {
				title: '键盘模块',
				canvasContext: undefined,
				keypadsRes: undefined,
				screenWidth: 0,
				screenHeight: 0
			}
		},
		methods: {
			touchStart: function(e) {
				let touchPoint = e.changedTouches[0]
				console.log("点击开始：" + touchPoint.x + "," + touchPoint.y)
			},
			touchMove: function(e) {
				let touchPoint = e.changedTouches[0]
				console.log("正在拖动：" + touchPoint.x + "," + touchPoint.y)
			},
			touchEnd: function(e) {
				let touchPoint = e.changedTouches[0]
				console.log("点击结束：" + touchPoint.x + "," + touchPoint.y)
			},
			touchCancel: function(e) {
				console.log("点击取消")
			},
		}
	}
</script>

<style>
	canvas {
		margin-left: 16px;
		margin-top: 16px;
	}
</style>
