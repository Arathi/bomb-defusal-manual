<template>
	<view>
        <page-head :title="title"></page-head>
		<view class="buttons-module">
			<uni-steps id="steps" :options="stepNames" :active="step-1" />
			<uni-forms ref="baseForm">
				<uni-forms-item label="按钮文本">
					<uni-segmented-control 
						:current="btnTextIndex" 
						:values="btnTexts" 
						@clickItem="onBtnTextChanged" />
				</uni-forms-item>
				<uni-forms-item label="按钮颜色">
					<uni-data-picker 
						:localdata="btnColors" 
						placeholder="请选择颜色"
						:value="btnColor"
						@change="onBtnColorPickerChanged">
					</uni-data-picker>
				</uni-forms-item>
				<uni-forms-item label="电池数量" v-show="showCellAmount">
					<uni-data-picker 
						:localdata="cellAmountDescs" 
						placeholder="请选择电池数量"
						:value="cellAmount"
						@change="onCellAmountChanged">
					</uni-data-picker>
				</uni-forms-item>
				<uni-forms-item id="labels" label="标签指示灯" v-show="showLabels">
					<!--
					<uni-data-checkbox 
						mode="tag" 
						multiple 
						v-model="labels" 
						:localdata="labelNames"/>
					-->
					<uni-tag 
						class="tag"
						text="CAR"
						size="normal"
						type="primary"
						:disabled="labelDatas.car.disabled"
						:inverted="!labelDatas.car.selected"
						@click="onLabelClicked($event,'car')"
					/>
					<uni-tag
						class="tag"
						text="FRK"
						size="normal"
						type="primary"
						:disabled="labelDatas.frk.disabled"
						:inverted="!labelDatas.frk.selected"
						@click="onLabelClicked($event,'frk')"
					/>
				</uni-forms-item>
				<uni-forms-item label="灯条颜色" v-show="showLightColor">
					<uni-data-picker 
						:localdata="lightColors" 
						placeholder="请选择灯条颜色"
						:value="lightColor"
						@change="onLightColorChanged">
					</uni-data-picker>
				</uni-forms-item>
				<uni-forms-item class="buttons">
					<button type="primary" 
							size="mini" 
							:disabled="btnNextDisabled"
							@click="onNext">
						下一步
					</button>
					<button type="warn" size="mini" @click="onReset">重&nbsp&nbsp;&nbsp;&nbsp;置</button>
					<button type="default" size="mini" @click="onUnlockAll" hidden>全部解锁</button>
				</uni-forms-item>
				<uni-section title="操作步骤" type="line">
					<uni-list>
						<uni-list-item v-for="action in actions" :title="action" />
					</uni-list>
				</uni-section>
			</uni-forms>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: '按钮模块',
				stepNames: [
					{ title: '开始' },
					{ title: '电池' },
					{ title: 'CAR' },
					{ title: 'FRK' },
					{ title: '按黄' },
					{ title: '点红' },
					{ title: '按住' },
					{ title: '结束' }
				],
				btnTexts: ['中止(1)', '引爆(2)', '按住(6)', '其他'],
				btnColors: [
					{ text: '蓝(1)', value: 'blue' },
					{ text: '白(3)', value: 'white' },
					{ text: '黄(5)', value: 'yellow' },
					{ text: '红(6)', value: 'red' },
					{ text: '绿', value: 'green' },
					{ text: '黑', value: 'black' },
					{ text: '其他', value: 'other' },
				],
				cellAmountDescs: [
					{ text: '没有', value: 0 },
					{ text: '一枚', value: 1 },
					{ text: '两枚(2)', value: 2 },
					{ text: '三枚及以上(2,4)', value: 3 },
				],
				labelNames: [
					{ text: 'CAR(3)', value: 'car' },
					{ text: 'FRK(4)', value: 'frk' },
				],
				lightColors: [
					{ text: '蓝', value: 'blue' },
					{ text: '白', value: 'white' },
					{ text: '黄', value: 'yellow' },
					{ text: '其他', value: 'other' },
				],
				activeColor: '#666666',
				btnTextIndex: -1,
				btnColor: undefined,
				cellAmount: -1,
				labels: [],
				lightColor: undefined,
				actions: ['1. 第一步：检查按钮文本与颜色'],
				step: 1,
				showCellAmount: false,
				showLightColor: false,
				showLabels: false,
				btnNextDisabled: true,
				labelDatas: {
					car: { disabled: true, selected: false },
					frk: { disabled: true, selected: false }
				},
			}
		},
		methods: {
			onBtnTextChanged(e) {
				this.btnTextIndex = e.currentIndex;
				if (this.step == 1) {
					this.step1();
				}
			},
			onBtnColorPickerChanged(e) {
				let picked = e.detail.value[0];
				this.btnColor = picked.value;
				if (this.btnColor == 'blue') {
					this.activeColor = '#0000FF';
				}
				if (this.btnColor == 'white') {
					this.activeColor = '#FFFFFF';
				}
				if (this.btnColor == 'yellow') {
					this.activeColor = '#FFFF00';
				}
				if (this.btnColor == 'red') {
					this.activeColor = '#FF0000';
				}
				
				if (this.step == 1) {
					this.step1();
				}
			},
			onCellAmountChanged(e) {
				let picked = e.detail.value[0];
				this.cellAmount = picked.value;
				
				if (this.step == 2) {
					this.step2(this.cellAmount, this.btnTextIndex);
				}
			},
			onLightColorChanged(e) {
				let picked = e.detail.value[0];
				this.lightColor = picked.value;
				this.onStepHold(this.lightColor);
			},
			onLabelClicked(e, label) {
				// let labelData = this.labelDatas[label];
				// console.log("点击标签" + label);
				this.labelDatas[label].selected = !this.labelDatas[label].selected;
				if (this.labelDatas[label].selected) {
					if (label == 'car') {
						this.step3(this.btnColor, true);
					}
					if (label == 'frk') {
						this.step4(this.cellAmount, true);
					}
				}
			},
			onNext() {
				if (this.step == 1) {
					this.step1();
					return;
				}
				if (this.step == 2) {
					this.step2();
					return;
				}
				if (this.step == 3) {
					this.step3();
					return;
				}
				if (this.step == 4) {
					this.step4();
					return;
				}
				if (this.step == 5) {
					this.step5();
					return;
				}
				if (this.step == 6) {
					this.step6();
					return;
				}
			},
			onReset() {
				this.btnTextIndex = -1;
				this.btnColor = undefined;
				this.cellAmount = -1;
				this.lightColor = undefined;
				this.actions = ['1. 第一步：检查按钮文本与颜色'];
				this.step = 1;
				this.showCellAmount = false;
				this.showLightColor = false;
				this.showLabels = false;
				this.labelDatas.car.disabled = true;
				this.labelDatas.frk.disabled = true;
				this.labelDatas.car.selected = false;
				this.labelDatas.frk.selected = false;
				this.btnNextDisabled = true;
			},
			onUnlockAll() {
				this.showCellAmount = true;
				this.showLightColor = true;
				this.showLabels = true;
				this.labelDatas.car.disabled = false;
				this.labelDatas.frk.disabled = false;
				this.btnNextDisabled = false;
			},
			// 1. 如果是写有“中止”的蓝色按钮，按住按钮
			step1() {
				if (this.btnTextIndex < 0 || this.btnTextIndex > 3) {
					this.addAction("请选择按钮文本");
					return false;
				}
				
				if (this.btnColor == undefined) {
					this.addAction("请选择按钮颜色");
					return false;
				}
				
				// 中止 - 蓝 - 随便选
				if (this.btnTextIndex == 0 && this.btnColor == 'blue') {
					this.onStepHold(undefined);
				}
				else {
					this.step = 2;
					this.addAction("第二步：检查电池数量");
					this.showCellAmount = true;
					this.step2();
				}
				return true;
			},
			// 2. 如果炸弹上有不止1个电池，同时按钮上写着“引爆”，按下按钮并立即松开
			step2() {
				if (this.cellAmount < 0) {
					this.addAction("请选择电池数量");
					return false;
				}
				
				// 引爆 - 随便 - 两枚
				if (this.cellAmount >= 2 && this.btnTextIndex == 1) {
					this.onBtnClick();
					return true;
				}
				else {
					this.step = 3;
					this.addAction("第三步：检查是否有CAR标签");
					// this.showLabels = true;
					// this.labelDatas.car.disabled = false;
					this.step3();
				}
				return true;
			},
			// 3. 如果按钮是白色的，同时炸弹上有个写着CAR的指示灯亮，按住按钮
			step3() {
				// if (color == undefined) {
				// 	this.addAction("请检查按钮颜色");
				// 	return;
				// }
				if (this.showLabels == false) {
					this.showLabels = true;
					this.btnNextDisabled = false;
				}
				if (this.labelDatas.car.disabled == true) {
					this.labelDatas.car.disabled = false;
					return;
				}
				
				// 引爆 - 白色 - 一枚 - CAR - 任意
				let car = this.labelDatas.car.selected;
				if (this.btnColor == 'white' && car === true) {
					this.onStepHold(undefined);
				}
				else {
					this.step = 4;
					this.addAction("第四步：检查是否有FRK标签");
					this.step4();
				}
			},
			// 4. 如果炸弹上有不止2个电池，也有写着FRK的指示灯亮，按下按钮并立即松开
			step4() {
				// if (this.showLabels == false) {
				// 	this.showLabels = true;
				// 	return;
				// }
				if (this.labelDatas.frk.disabled == true) {
					this.labelDatas.frk.disabled = false;
					return;
				}
				
				// 按住 - 白色 - 三枚 - 下一步 - FRK
				let frk = this.labelDatas.frk.selected;
				if (this.cellAmount >= 3 && frk === true) {
					this.onBtnClick();
					return;
				}
				this.step5();
			},
			// 5. 如果按钮是黄色的，按住按钮
			step5() {
				this.addAction('第五步：黄色按钮');
				// 按住 - 黄色 - 三枚 - 下一步 - 下一步 - 任意
				if (this.btnColor == 'yellow') {
					this.onStepHold(undefined);
					return;
				}
				this.step6();
			},
			// 6. 如果是写有“按住”的红色按钮，按下按钮并立即松开
			// 7. 如果不满足上述任一情况，按住按钮
			step6() {
				this.addAction('第六步：红色“按住”');
				// 按住 - 红色 - 三枚 - 下一步 - 下一步
				if (this.btnTextIndex == 2 && this.btnColor == 'red') {
					this.onBtnClick();
				}
				// 中止 - 红色 - 三枚 - 下一步 - 下一步 - 任意
				else {
					this.addAction('第七步：其他情况');
					this.onStepHold(undefined);
				}
			},
			onBtnClick() {
				this.addAction('按下按钮并立即松开');
				this.step = 8;
				this.btnNextDisabled = true;
			},
			onStepHold(light) {
				this.btnNextDisabled = true;
				
				this.step = 7;
				
				if (light == undefined) {
					this.addAction("按住按钮，检查光条颜色");
					this.lightColor = undefined;
					this.showLightColor = true;
					return;
				}
				
				// 蓝色光条： 在计时器任意数位显示4时松开
				if (light == 'blue') {
					this.addAction('在计时器任意数位显示4时松开');
				}
				// 白色光条： 在计时器任意数位显示1时松开
				if (light == 'white') {
					this.addAction('在计时器任意数位显示1时松开');
				}
				// 黄色光条： 在计时器任意数位显示5时松开
				if (light == 'yellow') {
					this.addAction('在计时器任意数位显示5时松开');
				}
				// 其他颜色光条： 在计时器任意数位显示1时松开
				if (light == 'other') {
					this.addAction('在计时器任意数位显示1时松开');
				}
				
				this.step = 8;
			},
			addAction(action) {
				let index = this.actions.length + 1;
				this.actions.unshift(index+ ". " + action);
			}
		}
	}
</script>

<style>
	#steps {
		padding-bottom: 16px;
	}
	.buttons-module {
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
	#labels .tag {
		margin-right: 8px;
	}
</style>
