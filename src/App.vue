<template>
	<div class="container">
    	<h1>Search Emoji</h1>

		<input
		type="search"
		placeholder="Type to search emoji..."
		class="searchInput"
		:value="searchInput"
		@input="searchEmoji"
		 /> <!--入力があるたびに「searchEmoji」を実行/入力された値を「searchInput」とする　？？？？ -->

		<div class="emojiLength">
			<div class="emojiWrapper">
				<div
					class="emojiItem"
					v-for="emoji in showEmojis"
					:key="emoji.emoji"
				>
				{{ emoji.emoji }}&nbsp;&nbsp;
				<span class="emoji-alias">
					:{{ emoji.aliases[0] }}:
				</span>
			</div>
			</div>
		</div>
	</div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const originEmojis = ref('') //「originEmojis」を空とする
const showEmojis = ref([]) //「showEmojis」を空の配列とする
const searchInput = ref('') //「searchInput」を空とする

const searchEmoji = (e) => { //「e」を引数として、以下の内容を「searchEmoji」に格納
	const value = e.target.value //「value」に検索ボックスに入力された値を格納
	searchInput.value = value //「searchInput.value」をvalue（＝検索ボックスに入力された値）とする？？？？searchInputは入力された値なのでは？？？？

	const filtered = value.length > 1 //「value」の数が1より多い場合（検索ボックスに複数単語が入力された場合）、
	? originEmojis.value.filter((emoji) => emoji.aliases[0].includes(value)) //emoji（originEmojis内のオブジェクト）のaliasesに「value（検索ボックスに入力された値）」が含まれているオブジェクトを「filtered」とする
	: [...originEmojis.value] //「value」の数が1以下の場合、originEmojisの値（文字列）を「filtered」とする

	showEmojis.value = filtered //showEmojisの値を「filtered」とする　？？？？これが検索機能の役割？？？？
}

onMounted(async () => {
	const res = await fetch('https://raw.githubusercontent.com/github/gemoji/master/db/emoji.json') //emoji.jsonを取得して「res」とする
	const json = await res.json() //「res」をJSON形式にしたものを「json」とする　？このjsonはどこで使われる？？？？
	showEmojis.value = json //showEmojisの値を「json」とする　？？？？
	originEmojis.value = json //originEmojisの値を「json」とする　？？？？
})
</script>

<style lang="scss">
body {
	font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica,
	  Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
	background: #20232a;
	color: #fff;
  }
  
  .container {
	width: 90%;
	margin: 0 auto;
  }
  
  .searchInput {
	padding: 10px 12px;
	width: 90%;
	min-width: 300px;
	border-radius: 8px;
	border: 1px solid #a49b9b;
	margin: 1em 0;
	font-size: 16px;
	outline: 0;
  }
  
  .emojiLength {
	margin-bottom: 1em;
	font-size: 16px;
	font-weight: bold;
  }
  
  .emojiWrapper {
	display: flex;
	justify-content: flex-start;
	align-items: center;
	flex-wrap: wrap;
	text-align: left;
  }
  
  .emojiItem {
	width: 100%;
	padding-bottom: 10px;
	cursor: pointer;
  }
  
  .emojiAlias:hover {
	zoom: 1.1;
  }
  
  .isSelected {
	color: #79d70f;
	zoom: 0.9 !important;
  }
  
  @media screen and (min-width: 600px) {
	.emojiItem {
	  width: 50%;
	}
  }
  
  @media screen and (min-width: 1000px) {
	.emojiItem {
	  width: 30%;
	}
  }
  
  @media screen and (min-width: 1200px) {
	.emojiItem {
	  width: 25%;
	}
  }
</style>