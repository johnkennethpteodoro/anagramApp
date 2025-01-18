<script setup>
import { ref } from "vue";

const isText1 = ref("");
const isText2 = ref("");
const resultMessage = ref("");
const resultDetails = ref(""); // Added for detailed result

const handleAnagramWord = (t1, t2) => {
	const text1 = t1.toLowerCase().trim();
	const text2 = t2.toLowerCase().trim();

	if (!text1 || !text2) {
		resultMessage.value = "Both inputs must be filled.";
		resultDetails.value = "";
		return;
	}

	// if (text1.length !== text2.length) {
	// 	resultMessage.value = "The words are of different lengths and cannot be anagrams.";
	// 	resultDetails.value = "";
	// 	return false;
	// }

	// Logic to check anagram and character count comparison
	const isAnagram = text1.split("").sort().join("") === text2.split("").sort().join("");

	if (isAnagram) {
		resultMessage.value = "A valid anagram :)";
		resultDetails.value = ""; // Clear the detailed result for anagram match
	} else {
		resultMessage.value = "Not a valid anagram :(";

		// Compare characters count and display missing/matching counts
		let sourceCount = {};
		let anagramCount = {};

		// Count characters in each word
		text1.split("").forEach((char) => (sourceCount[char] = (sourceCount[char] || 0) + 1));
		text2.split("").forEach((char) => (anagramCount[char] = (anagramCount[char] || 0) + 1));

		let matchCount = 0;
		let missingCount = 0;

		// Compare character counts
		for (let char in sourceCount) {
			if (anagramCount[char]) {
				matchCount += Math.min(sourceCount[char], anagramCount[char]);
			} else {
				missingCount += sourceCount[char];
			}
		}

		for (let char in anagramCount) {
			if (!sourceCount[char]) {
				missingCount += anagramCount[char];
			}
		}

		resultDetails.value = `Matches word: ${matchCount}, Missing word: ${missingCount}`;
	}
};

const clearInputs = () => {
	isText1.value = "";
	isText2.value = "";
	resultMessage.value = "";
	resultDetails.value = ""; // Clear the detailed result when clearing
};
</script>

<template>
	<div class="bg-zinc-900 h-screen">
		<div
			class="bg-layout absolute inset-0 m-auto w-[800px] h-[800px] opacity-20 z-10 rounded-full"
		></div>
		<h1
			class="flex gap-5 justify-center absolute top-32 text-white text-center w-full text-[35px]"
		>
			Gramana
		</h1>
		<ul class="flex gap-5 justify-center absolute top-56 text-white text-center w-full z-20">
			<li><a href="">Checker</a></li>
			<li><a href="">About</a></li>
			<li><a href="">FAQ</a></li>
			<li><a href="">Contact</a></li>
		</ul>
		<div class="flex justify-center h-full w-full items-center">
			<div class="grid grid-cols-2 w-[1000px] gap-10">
				<div class="py-6 bg-zinc-950 rounded-xl z-20">
					<h1 class="text-white mb-5 mx-6">Anagram checker</h1>

					<div class="flex justify-between items-center mb-3 mx-6">
						<h1 class="text-zinc-400 text-[14px] font-medium">First word</h1>
						<input
							v-model="isText1"
							type="text"
							class="w-[250px] border border-zinc-600 border-opacity-50 rounded-lg px-2.5 py-1.5 text-[14px] focus:border focus:border-zinc-700 focus:outline-none bg-zinc-800 text-white"
							placeholder="Enter a word"
						/>
					</div>
					<div class="flex justify-between items-center mb-9 mx-6">
						<h1 class="text-zinc-400 text-[14px] font-medium">Second word</h1>
						<input
							v-model="isText2"
							type="text"
							class="w-[250px] border border-zinc-600 border-opacity-50 rounded-lg px-2.5 py-1.5 text-[14px] focus:border focus:border-zinc-700 focus:outline-none bg-zinc-800 text-white"
							placeholder="Enter a word"
						/>
					</div>

					<div class="border-b-[1px] border-zinc-800"></div>

					<div class="w-full flex justify-end gap-2">
						<button
							@click="clearInputs"
							class="text-white mt-7 bg-zinc-800 px-3 py-1.5 text-[14px] font-semibold rounded-lg"
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
				<div class="bg-white rounded-xl py-6 z-20">
					<h1 class="text-black mb-5 font-semibold mx-6">Anagram check results</h1>
					<h1
						:class="{
							'text-green-500': resultMessage === 'A valid anagram :)',
							'text-red-500': resultMessage === 'Not a valid anagram :(',
							'text-[30px]': true,
							'font-bold': true,
							'px-6': true,
						}"
					>
						{{ resultMessage ? resultMessage : "Type your words ..." }}
					</h1>
					<div class="border-b-[1px] border-zinc-800 border-opacity-10 mt-[75px]"></div>
					<h2 class="text-black text-[16px] font-medium mt-2 mx-6" v-if="resultDetails">
						{{ resultDetails }}
					</h2>
				</div>
			</div>
		</div>
	</div>
</template>

<style scoped>
.bg-layout {
	background-image: linear-gradient(#3f3f46 1px, transparent 1px),
		linear-gradient(to right, #3f3f46 1px, #18181b 1px);
	background-size: 60px 60px;
}
</style>
