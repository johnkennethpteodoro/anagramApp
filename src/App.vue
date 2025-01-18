<script setup>
import { ref } from "vue";

const isText1 = ref("");
const isText2 = ref("");
const resultMessage = ref("");

const handleAnagramWord = (t1, t2) => {
	const text1 = t1.toLowerCase().trim();
	const text2 = t2.toLowerCase().trim();

	if (!text1 || !text2) {
		resultMessage.value = "Both inputs must be filled.";
		return;
	}

	if (text1.length !== text2.length) {
		resultMessage.value = "The words are of different lengths and cannot be anagrams.";
		return false;
	}

	const isAnagram = text1.split("").sort().join("") === text2.split("").sort().join("");

	resultMessage.value = isAnagram ? "It's an anagram word!" : "It's not an anagram word.";
};

const clearInputs = () => {
	isText1.value = "";
	isText2.value = "";
	resultMessage.value = "";
};
</script>

<template>
	<div class="bg-zinc-900">
		<div class="flex justify-center h-screen w-full items-center">
			<div class="grid grid-cols-2 w-[1000px] gap-10">
				<div class="py-6 bg-zinc-950 rounded-xl">
					<h1 class="text-white mb-5 mx-6">Anagram Checker</h1>

					<div class="flex justify-between items-center mb-3 mx-6">
						<h1 class="text-zinc-400 text-[14px] font-medium">First Word</h1>
						<input
							v-model="isText1"
							type="text"
							class="w-[250px] border border-zinc-600 border-opacity-50 rounded-lg px-2.5 py-1.5 text-[14px] focus:border focus:border-zinc-700 focus:outline-none bg-zinc-800 text-white"
							placeholder="Enter a word"
						/>
					</div>
					<div class="flex justify-between items-center mb-9 mx-6">
						<h1 class="text-zinc-400 text-[14px] font-medium">Second Word</h1>
						<input
							v-model="isText2"
							type="text"
							class="w-[250px] border border-zinc-600 border-opacity-50 rounded-lg px-2.5 py-1.5 text-[14px] focus:border focus:border-zinc-700 focus:outline-none bg-zinc-800 text-white"
							placeholder="Enter a word"
						/>
					</div>

					<div class="border-b-[1px] border-zinc-800 bg-zinc-800 border-opacity-10"></div>

					<div class="w-full flex justify-end gap-2">
						<button
							@click="clearInputs"
							class="text-white mt-7 bg-zinc-800 px-3 py-1.5 text-[12px] font-semibold rounded-lg"
						>
							Clear
						</button>
						<button
							@click="handleAnagramWord(isText1, isText2)"
							class="text-zinc-700 mt-7 bg-white px-3 py-1.5 text-[12px] font-semibold rounded-lg mr-6"
						>
							Check
						</button>
					</div>
				</div>
				<div class="bg-white rounded-xl p-6">
					<h1 class="text-black mb-5 font-semibold">Results</h1>
					<h1
						:class="{
							'text-green-500': resultMessage === 'It\'s an anagram word!',
							'text-red-500': resultMessage === 'It\'s not an anagram word.',
							'text-[30px]': true,
							'font-bold': true,
						}"
					>
						{{ resultMessage }}
					</h1>
				</div>
			</div>
		</div>
	</div>
</template>

<style scoped>
/* * {
	border: 1px solid red;
} */
</style>
