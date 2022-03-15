<template>
	<view>
		<page-head :title="title"></page-head>
		<view class="whos-on-1st-module">
			<uni-forms ref="baseForm">
				<uni-forms-item label="显示屏内容">
					<uni-data-picker 
						id="display-text-picker"
						:localdata="displayTexts" 
						placeholder="请选择屏幕显示文本"
						:value="displayText"
						@change="onDisplayTextChanged">
					</uni-data-picker>
				</uni-forms-item>
			</uni-forms>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: '他叫什么',
				displayTexts: [
					{ text: '是',   value: 21 },
					{ text: '一',   value: 12 },
					{ text: '显示', value: 32 },
					{ text: '好',   value: 12 },
					{ text: '说',   value: 32 },
					{ text: '没有', value: 21 },
					// ---
					{ text: '(空格)',  value: 31 },
					{ text: '空',      value: 22 },
					{ text: '没',      value: 32 },
					{ text: '灯',      value: 21 },
					{ text: '等',      value: 32 },
					{ text: '瞪',      value: 22 },
					// ---
					{ text: '动',   value: 22 },
					{ text: '东',   value: 31 },
					{ text: '当',   value: 31 },
					{ text: '稍等', value: 32 },
					{ text: '你',   value: 22 },
					{ text: '泥',   value: 32 },
					// ---
					{ text: '您',     value: 22 },
					{ text: '你的',   value: 22 },
					{ text: '您的',   value: 11 },
					{ text: '他们',   value: 32 },
					{ text: '它们的', value: 31 },
					{ text: '她们的', value: 22 },
					// ---
					{ text: '他们的', value: 21 },
					{ text: '戏',     value: 32 },
					{ text: '西',     value: 12 },
					{ text: '喜',     value: 32 },
				],
				
				// ---
				displayText: undefined,
			}
		},
		methods: {
			onDisplayTextChanged(e) {
				console.log(e)
			}
		}
	}
</script>

<style>
	.whos-on-1st-module {
		padding-left: 16px;
		padding-right: 16px;
	}
</style>
