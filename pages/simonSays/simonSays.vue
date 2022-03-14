<template>
	<view>
        <page-head :title="title"></page-head>
		<view class="simon-says-module">
			<uni-forms ref="baseForm">
				<uni-forms-item label="序列号">
					<uni-segmented-control 
						:current="seqTypeIndex" 
						:values="seqTypes" 
						@clickItem="onSeqTypeChanged" />
				</uni-forms-item>
				<uni-forms-item label="失误次数">
					<slider :value="failAmount" @change="sliderChange" min="0" max="2" show-value />
				</uni-forms-item>
				<uni-forms-item id='color-buttons'>
					<view class="uni-flex uni-row">
						<view class="flex-item">
							<uni-button id='btn-blue' @click="colorBtnClicked('blue')">蓝</uni-button>
						</view>
					</view>
					<view class="uni-flex uni-row">
						<view class="flex-item">
							<uni-button id='btn-red' @click="colorBtnClicked('red')">红</uni-button>
						</view>
						<view class="flex-item">
							<uni-button id='btn-yellow' @click="colorBtnClicked('yellow')">黄</uni-button>
						</view>
					</view>
					<view class="uni-flex uni-row">
					</view>
					<view class="uni-flex uni-row">
						<view class="flex-item">
							<uni-button id='btn-green' @click="colorBtnClicked('green')">绿</uni-button>
						</view>
					</view>
				</uni-forms-item>
				<uni-forms-item>
					<uni-button id='btn-reset' @click="reset">重置</uni-button>
				</uni-forms-item>
				<uni-forms-item label="闪光顺序">
					<view class="uni-flex uni-row color-blocks">
						<view v-for="color in inputs" class="flex-item" :class="'color-block-' + color">
							{{colorNames[color]}}
						</view>
					</view>
				</uni-forms-item>
				<uni-forms-item label="按键顺序">
					<view class="uni-flex uni-row color-blocks">
						<view v-for="color in outputs" class="flex-item" :class="'color-block-' + color">
							{{colorNames[color]}}
						</view>
					</view>
				</uni-forms-item>
			</uni-forms>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: '四色方块',
				seqTypes: [
					'包含元音',
					'不包含元音'
				],
				colorNames: {
					blue: '蓝',
					red: '红',
					yellow: '黄',
					green: '绿'
				},
				failAmount: 0,
				seqTypeIndex: -1,
				inputs: [],
				outputs: []
			}
		},
		methods: {
			onSeqTypeChanged: function(e) {
				this.seqTypeIndex = e.currentIndex;
			},
			sliderChange: function(e) {
				this.failAmount = e.detail.value;
				this.inputs = [];
				this.reflushOutputs();
			},
			reset: function() {
				this.failAmount = 0;
				this.seqTypeIndex = -1;
				this.inputs = [];
				this.outputs = [];
			},
			reflushOutputs: function() {
				this.outputs = [];
				for (let idx = 0; idx < this.inputs.length; idx++) {
					let color = this.inputs[idx];
					if (this.seqTypeIndex == 0) {
						if (this.failAmount == 0) {
							if (color == 'red') this.outputs.push('blue');
							else if (color == 'blue') this.outputs.push('red');
							else if (color == 'green') this.outputs.push('yellow');
							else if (color == 'yellow') this.outputs.push('green');
						}
						else if (this.failAmount == 1) {
							if (color == 'red') this.outputs.push('yellow');
							else if (color == 'blue') this.outputs.push('green');
							else if (color == 'green') this.outputs.push('blue');
							else if (color == 'yellow') this.outputs.push('red');
						}
						else if (this.failAmount == 2) {
							if (color == 'red') this.outputs.push('green');
							else if (color == 'blue') this.outputs.push('red');
							else if (color == 'green') this.outputs.push('yellow');
							else if (color == 'yellow') this.outputs.push('blue');
						}
					}
					else if (this.seqTypeIndex == 1) {
						if (this.failAmount == 0) {
							if (color == 'red') this.outputs.push('blue');
							else if (color == 'blue') this.outputs.push('yellow');
							else if (color == 'green') this.outputs.push('green');
							else if (color == 'yellow') this.outputs.push('red');
						}
						else if (this.failAmount == 1) {
							if (color == 'red') this.outputs.push('red');
							else if (color == 'blue') this.outputs.push('blue');
							else if (color == 'green') this.outputs.push('yellow');
							else if (color == 'yellow') this.outputs.push('green');
						}
						else if (this.failAmount == 2) {
							if (color == 'red') this.outputs.push('yellow');
							else if (color == 'blue') this.outputs.push('green');
							else if (color == 'green') this.outputs.push('blue');
							else if (color == 'yellow') this.outputs.push('red');
						}
					}
					else {
						console.log("未选择序列号类型");
						uni.showToast({
							icon: 'none',
							title: '未选择序列号类型'
						});
					}
				}
			},
			colorBtnClicked: function(color) {
				if (this.seqTypeIndex != 0 && this.seqTypeIndex != 1) {
					uni.showToast({
						icon: 'none',
						title: '未选择序列号类型'
					});
					return;
				}
				this.inputs.push(color);
				this.reflushOutputs();
			},
		}
	}
</script>

<style>
	.simon-says-module {
		padding-left: 16px;
		padding-right: 16px;
	}
	
	#color-buttons .flex-item {
		height: 62px;
	}
	
	.flex-item {
		width: 100%;
		height: 150rpx;
		text-align: center;
		line-height: 150rpx;
	}
	
	#btn-blue {
		width: 45%;
		background-color: #007AFF;
	}
	#btn-red {
		width: 90%;
		background-color: #FF3333;
	}
	#btn-yellow {
		width: 90%;
		background-color: #ffff00;
	}
	#btn-green {
		width: 45%;
		background-color: #00ff00;
	}
	
	.color-blocks .flex-item {
		line-height: 32px;
		height: 32px;
	}
	.color-block-blue {
		background-color: #007AFF;
	}
	.color-block-red {
		background-color: #FF3333;
	}
	.color-block-yellow {
		background-color: #ffff00;
	}
	.color-block-green {
		background-color: #00ff00;
	}
</style>
