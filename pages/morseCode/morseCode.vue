<template>
	<view>
        <page-head :title="title"></page-head>
		<view class="morse-code-module">
			<uni-forms ref="baseForm">
				<uni-forms-item label="摩斯电码">
					<input type="text" :value="morseCodes" 
							@input="onMorseCodesChanged" 
							placeholder="请输入摩斯电码"/>
				</uni-forms-item>
				<view class="uni-flex uni-row buttons">
					<view class="flex-item input-button">
						<button @click="inputCode($event, '.')">.</button>
					</view>
					<view class="flex-item input-button">
						<button @click="inputCode($event, '-')">-</button>
					</view>
					<view class="flex-item input-button">
						<button @click="inputCode($event, ',')">,</button>
					</view>
					<view class="flex-item input-button">
						<button @click="inputCode($event, '?')" disabled>?</button>
					</view>
					<view class="flex-item input-button">
						<button @click="inputCode($event, '*')" disabled>*</button>
					</view>
				</view>
				<uni-forms-item label="原文">
					<text>{{decoded}}</text>
				</uni-forms-item>
				<uni-forms-item label="备选">
				</uni-forms-item>
				<uni-table stripe emptyText="暂无更多数据">
					<uni-tr>
						<uni-th align="center">单词</uni-th>
						<uni-th align="center">频率（MHz）</uni-th>
					</uni-tr>
					<uni-tr v-for="result in results">
						<uni-td align="center">{{result.word}}</uni-td>
						<uni-td align="center">{{result.freq}}</uni-td>
					</uni-tr>
				</uni-table>
			</uni-forms>
		</view>
	</view>
</template>

<script>
	export default {
		onLoad() {
			// console.log("加载完成");
			// this.generateDictMorseCodeToChar();
		},
		data() {
			return {
				title: '摩斯电码',
				dictCharToMorseCode: {
					'0': '-----',
					'1': '.----',
					'2': '..---',
					'3': '...--',
					'4': '....-',
					'5': '.....',
					'6': '-....',
					'7': '--...',
					'8': '---..',
					'9': '----.',
					// ---
					'A': '.-',
					'B': '-...',
					'C': '-.-.',
					'D': '-..',
					'E': '.',
					'F': '..-.',
					'G': '--.',
					// ---
					'H': '....',
					'I': '..',
					'J': '.---',
					'K': '-.-',
					'L': '.-..',
					'M': '--',
					'N': '-.',
					// ---
					'O': '---',
					'P': '.--.',
					'Q': '--.-',
					'R': '.-.',
					'S': '...',
					'T': '-',
					// ---
					'U': '..-',
					'V': '...-',
					'W': '.--',
					'X': '-..-',
					'Y': '-.--',
					'Z': '--..',
				},
				dictMorseCodeToChar: {
					"-----": "0",
					".----": "1",
					"..---": "2",
					"...--": "3",
					"....-": "4",
					".....": "5",
					"-....": "6",
					"--...": "7",
					"---..": "8",
					"----.": "9",
					".-": "A",
					"-...": "B",
					"-.-.": "C",
					"-..": "D",
					".": "E",
					"..-.": "F",
					"--.": "G",
					"....": "H",
					"..": "I",
					".---": "J",
					"-.-": "K",
					".-..": "L",
					"--": "M",
					"-.": "N",
					"---": "O",
					".--.": "P",
					"--.-": "Q",
					".-.": "R",
					"...": "S",
					"-": "T",
					"..-": "U",
					"...-": "V",
					".--": "W",
					"-..-": "X",
					"-.--": "Y",
					"--..": "Z"
				},
				words: {
					'shell':  '3.505',
					'halls':  '3.515',
					'slick':  '3.522',
					'trick':  '3.532',
					// ---
					'boxes':  '3.535',
					'leaks':  '3.542',
					'strobe': '3.545',
					'bistro': '3.552',
					// ---
					'flick':  '3.555',
					'bombs':  '3.565',
					'break':  '3.572',
					'brick':  '3.575',
					// ---
					'steak':  '3.582',
					'sting':  '3.592',
					'vector': '3.595',
					'beats':  '3.600',
				},
				morseCodes: '',
				decoded: '',
				results: undefined
			}
		},
		methods: {
			inputCode(e, c) {
				this.morseCodes += c;
				this.decode();
			},
			decode() {
				let morseCodeList = this.morseCodes.split(',');
				this.decoded = '';
				for (let idx = 0; idx < morseCodeList.length; idx++) {
					let morseCode = morseCodeList[idx].trim();
					if (morseCode == '') {
						continue;
					}
					if (!this.dictMorseCodeToChar.hasOwnProperty(morseCode)) {
						console.log("无法解析的摩斯电码：" + morseCode);
						this.decoded += '?';
						continue;
					}
					let ch = this.dictMorseCodeToChar[morseCode];
					this.decoded += ch;
				}
				let lower = this.decoded.toLowerCase();
				this.results = [];
				for (let word in this.words) {
					if (word.indexOf(lower) < 0) {
						continue;
					}
					let freq = this.words[word];
					let result = {
						word: word,
						freq: freq
					};
					this.results.push(result);
				}
			},
			onMorseCodesChanged(e) {
				this.morseCodes = e.detail.value;
				this.decode();
			}
		}
	}
</script>

<style>
	.morse-code-module {
		padding-left: 16px;
		padding-right: 16px;
	}
	.flex-item.input-button {
		width: 20%;
		height: 120rpx;
		text-align: center;
		line-height: 120rpx;
		font-size: x-large;
	}
</style>
