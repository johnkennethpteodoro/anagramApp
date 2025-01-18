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
		resultMessage.value = "Both inputs must be filled";
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
	<div class="h-screen bg-zinc-900">
		<div class="absolute inset-0 z-10 w-full h-screen m-auto bg-layout opacity-40"></div>
		<div class="hidden xl:block lg:block md:hidden sm:hidden">
			<div
				class="bg-zinc-900 rounded-b-full w-[300px] h-[150px] absolute z-20 left-[500px]"
			></div>
			<div
				class="bg-zinc-900 rounded-b-full w-[400px] h-[200px] absolute z-20 left-[200px]"
			></div>
			<div
				class="bg-zinc-900 rounded-e-full w-[250px] h-[400px] absolute z-20 left-0 top-[500px]"
			></div>
			<div
				class="bg-zinc-900 rounded-t-full w-[400px] h-[200px] absolute z-20 left-[100px] bottom-0"
			></div>
			<div class="bg-zinc-900 rounded-s-full w-[300px] h-[500px] absolute z-20 right-0"></div>
			<div
				class="bg-zinc-900 rounded-b-full w-[300px] h-[150px] absolute z-20 right-[70px]"
			></div>
			<div
				class="bg-zinc-900 rounded-full w-[300px] h-[300px] absolute z-20 right-[300px] bottom-[100px]"
			></div>
		</div>
		<h1
			class="flex gap-5 justify-center absolute xl:top-32 lg:top-32 md:top-8 sm:top-8 top-4 text-white text-center w-full text-[25px] uppercase tracking-widest font-extrabold z-30"
		>
			Anagram
		</h1>
		<ul
			class="flex gap-12 xl:text-[14px] lg:text-[14px] md:text-[14px] sm:text-[13px] text-[12px] justify-center absolute xl:top-56 lg:top-56 md:top-28 sm:top-28 top-20 text-zinc-500 font-semibold text-center w-full z-20 uppercase"
		>
			<li><a href="">About</a></li>
			<li><a href="">Checker</a></li>
			<li><a href="">FAQ</a></li>
			<li><a href="">Contact</a></li>
		</ul>
		<div class="flex items-center justify-center w-full h-full">
			<div
				class="xl:grid xlgrid-cols-2 lg:grid lg:grid-cols-2 md:grid md:grid-cols-1 sm:grid sm:grid-cols-1 md:mx-16 xl:mx-0 lg:mx-0 mx-5 grid grid-cols-1 w-[1000px] gap-7"
			>
				<div class="z-20 py-6 bg-zinc-950 rounded-xl box-shadow">
					<h1 class="z-50 mx-6 mb-5 tracking-widest text-white uppercase text-[14px]">
						Anagram checker
					</h1>

					<div
						class="items-center justify-between block mx-6 xl:flex lg:flex md:block sm:block mb-9"
					>
						<h1
							class="text-zinc-400 text-[12px] font-medium uppercase tracking-wider xl:mb-0 lg:mb-0 md:mb-2 sm:mb-2 mb-2"
						>
							First word
						</h1>
						<input
							v-model="isText1"
							type="text"
							class="xl:w-[250px] lg:w-[250px] md:w-full sm:w-full w-full border border-zinc-600 border-opacity-50 rounded-lg px-2.5 py-1.5 text-[14px] focus:border focus:border-zinc-700 focus:outline-none bg-zinc-800 text-white"
							placeholder="Enter a word"
						/>
					</div>
					<div
						class="items-center justify-between block mx-6 xl:flex lg:flex md:block sm:block mb-9"
					>
						<h1
							class="text-zinc-400 text-[12px] font-medium uppercase tracking-wider xl:mb-0 lg:mb-0 md:mb-2 sm:mb-2 mb-2"
						>
							Second word
						</h1>
						<input
							v-model="isText2"
							type="text"
							class="xl:w-[250px] lg:w-[250px] md:w-full sm:w-full w-full border border-zinc-600 border-opacity-50 rounded-lg px-2.5 py-1.5 text-[14px] focus:border focus:border-zinc-700 focus:outline-none bg-zinc-800 text-white"
							placeholder="Enter a word"
						/>
					</div>

					<div class="border-b-[1px] border-zinc-800"></div>

					<div class="flex justify-end w-full gap-2">
						<button
							@click="clearInputs"
							class="text-white uppercase mt-7 bg-zinc-800 px-3 py-1.5 text-[14px] rounded-lg"
						>
							Clear
						</button>
						<button
							@click="handleAnagramWord(isText1, isText2)"
							class="text-zinc-700 mt-7 uppercase bg-white px-3 py-1.5 text-[12px] font-semibold rounded-lg mr-6"
						>
							Check
						</button>
					</div>
				</div>
				<div class="z-20 py-6 bg-white rounded-xl box-shadow">
					<h1
						class="mx-6 mb-5 font-semibold tracking-widest text-black uppercase text-[14px]"
					>
						Anagram check results
					</h1>
					<h1
						:class="{
							'text-green-500': resultMessage === 'A valid anagram :)',
							'text-red-500': resultMessage === 'Not a valid anagram :(',
							'text-orange-500': resultMessage === 'Both inputs must be filled',
							'text-[25px]': true,
							'font-bold': true,
							'px-6': true,
						}"
					>
						{{ resultMessage ? resultMessage : "Type your words ..." }}
					</h1>
					<div
						class="border-b-[1px] border-zinc-800 border-opacity-10 mt-[100px] mb-10"
					></div>
					<h2
						class="text-[14px] font-medium mt-2 mx-6 uppercase text-zinc-500"
						v-if="resultDetails"
					>
						{{ resultDetails }}
					</h2>
				</div>
			</div>
			<p
				class="z-30 text-white absolute xl:bottom-[200px] lg:bottom-[200px] md:bottom-[100px] sm:bottom-[100px] bottom-[50px] uppercase text-[12px]"
			>
				Developed by: John Kenneth Teodoro
			</p>
		</div>
	</div>
</template>

<style scoped>
.bg-layout {
	background-image: linear-gradient(#3f3f46 1px, transparent 1px),
		linear-gradient(to right, #3f3f46 1px, #18181b 1px);
	background-size: 60px 60px;
}

.box-shadow {
	box-shadow: rgba(99, 99, 99, 0.2) 0px 2px 8px 0px;
}
</style>
