<template>
	<view>
        <page-head :title="title"></page-head>
		<view class="morse-code-module">
			<uni-forms ref="baseForm">
			</uni-forms>
		</view>
	</view>
</template>

<script>
	export default {
		onLoad() {
			console.log("加载完成");
			this.generateDictMorseCodeToChar();
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
				dictMorseCodeToChar: undefined,
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
			}
		},
		methods: {
			generateDictMorseCodeToChar() {
				this.dictMorseCodeToChar = new Map();
				for (let char in this.dictCharToMorseCode) {
					let morseCode = this.dictCharToMorseCode[char];
					this.dictMorseCodeToChar.set(morseCode, char);
				}
				console.log('摩斯电码-字符 字典 生成完成');
			},
		}
	}
</script>

<style>

</style>
