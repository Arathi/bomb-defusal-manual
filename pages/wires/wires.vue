<template>
	<view>
        <page-head :title="title"></page-head>
		<view class="wires-module">
			<uni-forms ref="baseForm">
				<uni-forms-item label="线路数量">
					<slider :value="amount" @change="sliderChange" min="3" max="6" show-value />
				</uni-forms-item>
				<uni-forms-item label="序列号尾数" v-if="amount > 3">
					<uni-data-checkbox 
						v-model="seqOddEven" 
						:localdata="seqLastTypes">
					</uni-data-checkbox>
				</uni-forms-item>
				<uni-forms-item label="第一根线">
					<uni-data-picker 
						id="wire-color-picker-1"
						:localdata="colorNames" 
						placeholder="请选择颜色"
						:value="colors[0]"
						:style="wireColorPickerStyle[0]"
						@change="wireColorPickerChange($event, 0)">
					</uni-data-picker>
				</uni-forms-item>
				<uni-forms-item label="第二根线">
					<uni-data-picker 
						id="wire-color-picker-2"
						:localdata="colorNames" 
						placeholder="请选择颜色"
						:value="colors[1]"
						@change="wireColorPickerChange($event, 1)">
					</uni-data-picker>
				</uni-forms-item>
				<uni-forms-item label="第三根线">
					<uni-data-picker 
						id="wire-color-picker-3"
						:localdata="colorNames" 
						placeholder="请选择颜色"
						:value="colors[2]"
						@change="wireColorPickerChange($event, 2)">
					</uni-data-picker>
				</uni-forms-item>
				<uni-forms-item label="第四根线" v-if="amount >= 4">
					<uni-data-picker 
						id="wire-color-picker-4"
						:localdata="colorNames" 
						placeholder="请选择颜色"
						:value="colors[3]"
						@change="wireColorPickerChange($event, 3)">
					</uni-data-picker>
				</uni-forms-item>
				<uni-forms-item label="第五根线" v-if="amount >= 5">
					<uni-data-picker 
						id="wire-color-picker-5"
						:localdata="colorNames" 
						placeholder="请选择颜色"
						:value="colors[4]"
						@change="wireColorPickerChange($event, 4)">
					</uni-data-picker>
				</uni-forms-item>
				<uni-forms-item label="第六根线" v-if="amount >= 6">
					<uni-data-picker 
						id="wire-color-picker-6"
						:localdata="colorNames" 
						placeholder="请选择颜色"
						:value="colors[5]"
						@change="wireColorPickerChange($event, 5)">
					</uni-data-picker>
				</uni-forms-item>
				<uni-forms-item class="buttons">
					<button type="primary" size="mini" @click="calc">计算</button>
					<button type="warn" size="mini" @click="reset">重置</button>
				</uni-forms-item>
				<uni-forms-item label="操作步骤">
					<view>{{output}}</view>
				</uni-forms-item>
			</uni-forms>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
                title: '线路模块',
				colorNames: [
					{ text: '红', value: 'red' },
					{ text: '绿', value: 'green' },
					{ text: '蓝', value: 'blue' },
					{ text: '白', value: 'white' },
					{ text: '黄', value: 'yellow' },
					{ text: '黑', value: 'black' },
					{ text: '其他', value: 'other' },
				],
				seqLastTypes: [
					{ text: '字母', value: 0 },
					{ text: '奇数', value: 1 },
					{ text: '偶数', value: 2 },
				],
				amount: 3,
				seqOddEven: -1,
				colors: [
					undefined, undefined, undefined, 
					undefined, undefined, undefined
				],
				wireColorPickerStyle: [
					undefined, undefined, undefined, 
					undefined, undefined, undefined
				],
				output: ''
			}
		},
		methods: {
			sliderChange(e) {
				this.amount = e.detail.value;
				console.log("当前线路数量：" + this.amount);
			},
			wireColorPickerChange(e, wireIndex) {
				let color = e.detail.value[0];
				this.colors[wireIndex] = color.value;
				console.log("线路" + (wireIndex+1) + "颜色发生变动：" + color.text + "(" + color.value + ")");
				if (color.value != undefined && color.value != 'other') {
					let style = 'background-color: ' + '#000' + ";";
					// this.wireColorPickerStyle[wireIndex] = style;
				}
			},
			calc() {
				// 序列号奇偶性检查
				if (this.amount > 3 && (this.seqOddEven < 0 || this.seqOddEven > 2) ) {
					uni.showToast({
						icon: "error",
						title: "参数缺失",
						duration: 1500
					});
					this.output = "请选择序列号尾数奇偶性";
					return;
				}
				
				// 线路颜色统计
				let redCounter = 0;
				let blueCounter = 0;
				let yellowCounter = 0;
				let blackCounter = 0;
				let whiteCounter = 0;
				for (let idx = 0; idx < this.amount; idx++) {
					let color = this.colors[idx];
					if (color == undefined) {
						uni.showToast({
							icon: "error",
							title: "参数缺失",
							duration: 1500
						});
						this.output = "请选择第" + (idx+1) + "根线的颜色";
						return;
					}
					if (color == 'red') {
						redCounter++;
					}
					if (color == 'blue') {
						blueCounter++;
					}
					if (color == 'yellow') {
						yellowCounter++;
					}
					if (color == 'black') {
						blackCounter++;
					}
					if (color == 'white') {
						whiteCounter++;
					}
				}
				
				let lastLineIndex = this.amount - 1;
				if (this.amount == 3) {  // 3根线
					// 如果没有红线，则剪断第二根线
					if (redCounter == 0) {
						this.output = '【3-1】剪断第二根线';
					}
					// 否则，当最后一根线为白线时，剪断最后一根线
					else if (this.colors[lastLineIndex] == 'white') {
						this.output = '【3-2】剪断最后一根线';
					}
					// 否则，当有不止一根蓝线的时候，剪断最后一根蓝线
					else if (blueCounter >= 2) {
						this.output = '【3-3】剪断最后一根蓝线';
					}
					// 否则，剪断最后一根线
					else {
						this.output = '【3-4】剪断最后一根线';
					}
				}
				else if (this.amount == 4) {  // 4根线
					// 如果有不止一根红线且序列号末位为奇数，则剪断最后一根红线
					if (redCounter >= 2 && this.seqOddEven == 1) {
						this.output = '【4-1】剪断最后一根红线';
					}
					// 否则，当没有红线且最后一根线是黄线时，剪断第一根线
					else if (redCounter == 0 && this.colors[lastLineIndex] == 'yellow') {
						this.output = '【4-2】剪断第一根线';
					}
					// 否则，当有且仅有一根蓝线时，剪断第一根线
					else if (blueCounter == 1) {
						this.output = '【4-3】剪断第一根线';
					}
					// 否则，当有不止一根黄线时，剪断最后一根线
					else if (yellowCounter >= 2) {
						this.output = '【4-4】剪断最后一根线';
					}
					// 否则，剪断第二根线
					else {
						this.output = '【4-5】剪断第二根线';
					}
				}
				else if (this.amount == 5) {  // 5根线
					// 如果最后一根线是黑线且序列号末位为奇数，则剪断第四根线
					if (this.colors[lastLineIndex] == 'black' && this.seqOddEven == 1) {
						this.output = '剪断第四根线';
					}
					// 否则，当有且仅有一根红线，且黄线不止一根时，剪断第一根线
					else if (redCounter == 1 && yellowCounter >= 2) {
						this.output = '剪断第一根线';
					}
					// 否则，当没有黑线时，剪断第二根线
					else if (blackCounter == 0) {
						this.output = '剪断第二根线';
					}
					// 否则，剪断第一根线
					else {
						this.output = '剪断第一根线';
					}
				}
				else if (this.amount == 6) {  // 6根线
					// 如果没有黄线且序列号末位为奇数，则剪断第三根线
					if (yellowCounter == 0 && this.seqOddEven == 1) {
						this.output = '剪断第三根线';
					}
					// 否则，当有且仅有一根黄线，且白线不止一根时，剪断第四根线
					else if (yellowCounter == 1 && whiteCounter >= 2) {
						this.output = '剪断第四根线';
					}
					// 否则，当没有红线时，剪断最后一根线
					else if (redCounter == 0) {
						this.output = '剪断最后一根线';
					}
					// 否则，剪断第四根线
					else {
						this.output = '剪断第四根线';
					}
				}
				else {
					uni.showToast({
						icon: "error",
						title: "参数异常",
						duration: 1500
					});
					this.output = '线路数量有异常'
				}
			},
			reset() {
				this.amount = 3;
				this.seqOddEven = -1;
				this.colors = [
					undefined, undefined, undefined, 
					undefined, undefined, undefined
				];
				this.wireColorPickerStyle = [
					undefined, undefined, undefined, 
					undefined, undefined, undefined
				];
				this.output = '';
			}
		}
	}
</script>

<style>
	.wires-module {
		padding-left: 16px;
		padding-right: 16px;
	}
	.buttons button {
		margin-right: 16px;
	}
	.buttons {
		display: flex;
		flex-direction: row;
		align-items: center;
	}
</style>
