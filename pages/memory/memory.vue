<template>
	<view>
        <page-head :title="title"></page-head>
		<view class="memory-module">
			<uni-steps id="steps" :options="stepNames" :active="step-1" />
			<view class="uni-flex uni-row">
				<view class="flex-item uni-bg-blue memory-display">{{displayNumber}}</view>
			</view>
			<view class="uni-flex uni-row">
				<view class="flex-item memory-button" 
					:class="clickIndex == 1 ? 'uni-bg-red' : 'uni-bg-green'">
					{{btnNumber[0]}}
				</view>
				<view class="flex-item memory-button" 
					:class="clickIndex == 2 ? 'uni-bg-red' : 'uni-bg-green'">
					{{btnNumber[1]}}
				</view>
				<view class="flex-item memory-button" 
					:class="clickIndex == 3 ? 'uni-bg-red' : 'uni-bg-green'">
					{{btnNumber[2]}}
				</view>
				<view class="flex-item memory-button" 
					:class="clickIndex == 4 ? 'uni-bg-red' : 'uni-bg-green'">
					{{btnNumber[3]}}
				</view>
			</view>
			<view class="uni-flex uni-row buttons">
				<view class="flex-item number-button">
					<button @click="onNumberButtonClicked($event, 1)" 
						:disabled="btnNumberDisabled[0]">
						1
					</button>
				</view>
				<view class="flex-item number-button">
					<button @click="onNumberButtonClicked($event, 2)" 
						:disabled="btnNumberDisabled[1]">
						2
					</button>
				</view>
				<view class="flex-item number-button">
					<button @click="onNumberButtonClicked($event, 3)" 
						:disabled="btnNumberDisabled[2]">
						3
					</button>
				</view>
				<view class="flex-item number-button">
					<button @click="onNumberButtonClicked($event, 4)" 
						:disabled="btnNumberDisabled[3]">
						4
					</button>
				</view>
			</view>
			<view class="uni-flex uni-row buttons">
				<view class="flex-item command-button">
					<button @click="clear">清空</button>
				</view>
				<view class="flex-item command-button">
					<button type="primary" @click="nextStep" :disabled="nextStepDisabled">下一阶段</button>
				</view>
				<view class="flex-item command-button">
					<button type="warn" @clear="reset">重置</button>
				</view>
			</view>
			
			<uni-grid :column="6" :showBorder="true" :square="false">
				<uni-grid-item class="uni-bg-blue">
					<text class="text log-table-cell">阶段</text>
				</uni-grid-item>
				<uni-grid-item class="uni-bg-blue">
					<text class="text log-table-cell">显示</text>
				</uni-grid-item>
				<uni-grid-item class="uni-bg-blue">
					<text class="text log-table-cell">按钮1</text>
				</uni-grid-item>
				<uni-grid-item class="uni-bg-blue">
					<text class="text log-table-cell">按钮2</text>
				</uni-grid-item>
				<uni-grid-item class="uni-bg-blue">
					<text class="text log-table-cell">按钮3</text>
				</uni-grid-item>
				<uni-grid-item class="uni-bg-blue">
					<text class="text log-table-cell">按钮4</text>
				</uni-grid-item>
			</uni-grid>
			
			<uni-grid :column="6" 
					:showBorder="true" 
					:square="false" 
					v-for="log in logs">
				<uni-grid-item>
					<text class="text log-table-cell">{{log.step}}</text>
				</uni-grid-item>
				<uni-grid-item>
					<text class="text log-table-cell">{{log.display}}</text>
				</uni-grid-item>
				<uni-grid-item :class="log.click == 1 ? 'uni-bg-red' : ''">
					<text class="text log-table-cell">{{log.btn[0]}}</text>
				</uni-grid-item>
				<uni-grid-item :class="log.click == 2 ? 'uni-bg-red' : ''">
					<text class="text log-table-cell">{{log.btn[1]}}</text>
				</uni-grid-item>
				<uni-grid-item :class="log.click == 3 ? 'uni-bg-red' : ''">
					<text class="text log-table-cell">{{log.btn[2]}}</text>
				</uni-grid-item>
				<uni-grid-item :class="log.click == 4 ? 'uni-bg-red' : ''">
					<text class="text log-table-cell">{{log.btn[3]}}</text>
				</uni-grid-item>
			</uni-grid>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: "记忆模块",
				stepNames: [
					{ title: '阶段1' },
					{ title: '阶段2' },
					{ title: '阶段3' },
					{ title: '阶段4' },
					{ title: '阶段5' },
				],
				// ---
				step: 0,
				displayNumber: undefined,
				btnNumber: [undefined, undefined, undefined, undefined],
				btnNumberDisabled: [false, false, false, false],
				nextStepDisabled: true,
				clickIndex: 0,
				logs: [],
				result: ''
			}
		},
		methods: {
			onNumberButtonClicked: function(e, number) {
				console.log("按下数字按钮" + number);
				if (this.displayNumber == undefined) {
					this.displayNumber = number;
				}
				else {
					for (let i = 0; i < 4; i++) {
						if (this.btnNumber[i] == undefined) {
							this.btnNumber[i] = number;
							this.btnNumberDisabled[number - 1] = true;
							break;
						}
					}
					if (this.btnNumber[3] != undefined) {
						this.check();
					}
				}
				this.$forceUpdate();
			},
			findBtnByValue(value) {
				for (let index = 0; index < 4; index++) {
					if (this.btnNumber[index] == value) {
						return index + 1;
					}
				}
				console.warn("未找到写有" + value + "的按钮");
				return 0;
			},
			check() {
				console.log("开始检测");
				
				if (this.step == 0) {
					if (this.displayNumber == 1) {
						console.log("按下第二个位置的按钮");
						this.clickIndex = 2;
					}
					if (this.displayNumber == 2) {
						console.log("按下第二个位置的按钮");
						this.clickIndex = 2;
					}
					if (this.displayNumber == 3) {
						console.log("按下第三个位置的按钮");
						this.clickIndex = 3;
					}
					if (this.displayNumber == 4) {
						console.log("按下第四个位置的按钮");
						this.clickIndex = 4;
					}
				}
				
				if (this.step == 1) {
					if (this.displayNumber == 1) {
						console.log("按下数字为“4”的按钮");
						this.clickIndex = this.findBtnByValue(4);
					}
					if (this.displayNumber == 2) {
						console.log("按下和阶段 1 中你所按下的按钮位置相同的按钮");
						this.clickIndex = this.logs[0].click;
					}
					if (this.displayNumber == 3) {
						console.log("按下第一个位置的按钮");
						this.clickIndex = 1;
					}
					if (this.displayNumber == 4) {
						console.log("按下和阶段 1 中你所按下的按钮位置相同的按钮");
						this.clickIndex = this.logs[0].click;
					}
				}
				
				if (this.step == 2) {
					if (this.displayNumber == 1) {
						console.log("按下和阶段 2 中你所按下的按钮数字相同的按钮");
						let step2value = this.logs[1].btn[this.logs[1].click - 1];
						this.clickIndex = this.findBtnByValue(step2value);
					}
					if (this.displayNumber == 2) {
						console.log("按下和阶段 1 中你所按下的按钮位置相同的按钮");
						let step1value = this.logs[0].btn[this.logs[0].click - 1];
						this.clickIndex = this.findBtnByValue(step1value);
					}
					if (this.displayNumber == 3) {
						console.log("按下第三个位置的按钮");
						this.clickIndex = 3;
					}
					if (this.displayNumber == 4) {
						console.log("按下数字为“4”的按钮");
						this.clickIndex = this.findBtnByValue(4);
					}
				}
				
				if (this.step == 3) {
					if (this.displayNumber == 1) {
						console.log("按下和阶段 1 中你所按下的按钮位置相同的按钮");
						let step1value = this.logs[0].btn[this.logs[0].click - 1];
						this.clickIndex = this.findBtnByValue(step1value);
					}
					if (this.displayNumber == 2) {
						console.log("按下第一个位置的按钮");
						this.clickIndex = 1;
					}
					if (this.displayNumber == 3) {
						console.log("按下和阶段 2 中你所按下的按钮数字相同的按钮");
						let step2value = this.logs[1].btn[this.logs[1].click - 1];
						this.clickIndex = this.findBtnByValue(step2value);
					}
					if (this.displayNumber == 4) {
						console.log("按下和阶段 2 中你所按下的按钮数字相同的按钮");
						let step2value = this.logs[1].btn[this.logs[1].click - 1];
						this.clickIndex = this.findBtnByValue(step2value);
					}
				}
				
				if (this.step == 4) {
					if (this.displayNumber == 1) {
						console.log("按下和阶段 1 中你所按下的按钮位置相同的按钮");
						let step1value = this.logs[0].btn[this.logs[0].click - 1];
						this.clickIndex = this.findBtnByValue(step1value);
					}
					if (this.displayNumber == 2) {
						console.log("按下和阶段 2 中你所按下的按钮数字相同的按钮");
						let step2value = this.logs[1].btn[this.logs[1].click - 1];
						this.clickIndex = this.findBtnByValue(step2value);
					}
					if (this.displayNumber == 3) {
						console.log("按下和阶段 4 中你所按下的按钮数字相同的按钮");
						let step4value = this.logs[3].btn[this.logs[3].click - 1];
						this.clickIndex = this.findBtnByValue(step4value);
					}
					if (this.displayNumber == 4) {
						console.log("按下和阶段 3 中你所按下的按钮数字相同的按钮");
						let step3value = this.logs[2].btn[this.logs[2].click - 1];
						this.clickIndex = this.findBtnByValue(step3value);
					}
				}
				
				this.nextStepDisabled = false;
			},
			nextStep() {
				this.step++;
				let log = {
					step: this.step,
					display: this.displayNumber,
					btn: [
						this.btnNumber[0],
						this.btnNumber[1],
						this.btnNumber[2],
						this.btnNumber[3]
					],
					click: this.clickIndex
				};
				this.logs.push(log);
				this.clear();
			},
			clear() {
				this.displayNumber = undefined;
				this.btnNumber = [undefined, undefined, undefined, undefined];
				this.btnNumberDisabled = [false, false, false, false];
				this.clickIndex = 0;
				this.nextStepDisabled = true;
			},
			reset() {
				this.clear();
				this.step = 0;
			}
		}
	}
</script>

<style>
	#steps {
		padding-bottom: 16px;
	}
	
	.memory-module {
		padding-left: 16px;
		padding-right: 16px;
	}
	
	.flex-item.memory-display {
		width: 100%;
		height: 240rpx;
		text-align: center;
		line-height: 240rpx;
		font-size: xx-large;
		background-color: #000000;
	}
	
	.flex-item.memory-button {
		width: 25%;
		height: 120rpx;
		text-align: center;
		line-height: 120rpx;
		font-size: x-large;
		border: 5rpx solid #F5F5F5;
	}
	
	.flex-item.number-button {
		width: 25%;
		height: 120rpx;
		text-align: center;
		line-height: 120rpx;
		font-size: x-large;
	}
	
	.flex-item.command-button {
		width: 33.33%;
		height: 120rpx;
		text-align: center;
		line-height: 120rpx;
		font-size: x-large;
	}
	
	.log-table-cell {
		text-align: center;
	}
	
	.buttons {
		padding-top: 10px;
	}

	.flex-item-V {
		width: 100%;
		height: 150rpx;
		text-align: center;
		line-height: 150rpx;
	}
</style>
