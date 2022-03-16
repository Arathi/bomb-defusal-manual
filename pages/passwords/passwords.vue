<template>
	<view>
		<page-head :title="title"></page-head>
		<view class="passwords-module">
			<uni-forms ref="baseForm">
				<uni-forms-item label="第一个字母">
					<input type="text" :value="chars[0]" 
							@input="charsChanged($event, 0)" />
				</uni-forms-item>
				<uni-forms-item label="第二个字母">
					<input type="text" :value="chars[1]" 
							@input="charsChanged($event, 1)" />
				</uni-forms-item>
				<uni-forms-item label="第三个字母">
					<input type="text" :value="chars[2]" 
							@input="charsChanged($event, 2)" />
				</uni-forms-item>
				<uni-forms-item label="第四个字母">
					<input type="text" :value="chars[3]" 
							@input="charsChanged($event, 3)" />
				</uni-forms-item>
				<uni-forms-item label="第五个字母">
					<input type="text" :value="chars[4]" 
							@input="charsChanged($event, 4)" />
				</uni-forms-item>
				<uni-forms-item label="备选单词">
					<text>{{pattern.length <= 0 ? '' : '模式：'}}{{pattern}}</text>
				</uni-forms-item>
				
				<uni-grid :column="4" :showBorder="false" :square="false">
					<uni-grid-item v-for="word in filteredWords">
						<text class="text">{{word}}</text>
					</uni-grid-item>
				</uni-grid>
			</uni-forms>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: "密码模块",
				words: [
					"about", "after", "again", "below", "could",
					"every", "first", "found", "great", "house",
					"large", "learn", "never", "other", "place",
					"plant", "point", "right", "small", "sound",
					"spell", "still", "study", "their", "there",
					"these", "thing", "think", "three", "water",
					"where", "which", "world", "would", "write"
				],
				chars: ['', '', '', '', ''],
				pattern: '',
				filteredWords: [],
			}
		},
		methods: {
			charsChanged(e, index) {
				this.chars[index] = e.detail.value.trim().toLowerCase();
				this.filter();
			},
			filter() {
				this.filteredWords = [];
				this.pattern = '';
				for (let i in this.chars) {
					let p = this.chars[i];
					if (p.length > 0) this.pattern += '[' + p + ']';
					else this.pattern += '[a-z]';
				}
				let re = new RegExp(this.pattern);
				for (let j in this.words) {
					let word = this.words[j];
					let m = re.exec(word);
					if (m != null) {
						this.filteredWords.push(word);
					}
				}
				this.$forceUpdate();
			}
		}
	}
</script>

<style>
	.passwords-module {
		padding-left: 16px;
		padding-right: 16px;
	}
</style>
