<script>
	import Counter from './Counter.svelte';
	import welcome from '$lib/images/svelte-welcome.webp';
	import welcome_fallback from '$lib/images/svelte-welcome.png';

	import { initializeApp } from 'firebase/app';
	import { getFirestore } from 'firebase/firestore';
	import { getAuth } from 'firebase/auth';

	// Initialize Firebase
	const app = initializeApp({
		apiKey: 'AIzaSyBvO1PrFKBZGGGhlvvywlSB8kfCSdQE064',
		authDomain: 'hanzidamage.firebaseapp.com',
		projectId: 'hanzidamage',
		storageBucket: 'hanzidamage.appspot.com',
		messagingSenderId: '47006688824',
		appId: '1:47006688824:web:fb58d2a6b5e7e25b40e8cd',
		measurementId: 'G-24WG0MV1VN'
	});
	export const firestore = getFirestore(app);
	export const auth = getAuth(app);
	import { FirebaseApp, Doc, Collection } from 'sveltefire';

	import { doc, setDoc } from 'firebase/firestore';

	/**
	 * @type {string}
	 */
	let newWord;
	/**
	 * @type {string}
	 */
	let newMnemonic;

	async function addWord() {
		await setDoc(doc(firestore, 'words', newWord), {
			word: newWord,
			mnemonic: newMnemonic,
			createdAt: Date.now()
		});
	}

	// Load dictionary

	// let request = new XMLHttpRequest();

	let percent;

	let cedictData;

	// request.addEventListener('progress', (evt) => {
	// 	percent = evt.loaded / 9497637;
	// });

	// request.addEventListener('load', (evt) => {
	// 	let { responseText } = evt.currentTarget;

	// 	console.log(responseText);
	// 	cedictData = responseText;
	// });

	// request.open('GET', 'static/cedict_ts.u8.txt');
	// request.send();

	// Write a function to fetch a dictionary text file that is located at static/cedict_ts.u8.txt
	// and assign it to the variable cedictData

	(async () => {
		let cedictData = await fetch('static/cedict_ts.u8.txt').then((evt) => {
			console.log(evt);
			// let { responseText } = evt.currentTarget;
			// let responseText = evt;

			// console.log(responseText);
			// cedictData = responseText;)
		});
	})();

	// (async () => {
	// 	await fetch("static/cedict_ts.u8.txt")
	// 	.then((evt) => {
	// 		console.log(evt)
	// 	// let { responseText } = evt.currentTarget;
	// 	// let responseText = evt;

	// 	// console.log(responseText);
	// 	// cedictData = responseText;)
	// })();

	// import wordDict from '$lib/data/cedict_ts.u8';
	// import wordIndex from '$lib/data/cedict.idx';

	// function findWord(data, text) {
	// 	const tlen = text.length;
	// 	var beg = 0;
	// 	var end = data.length - 1;
	// 	var i;
	// 	var mi;
	// 	var mis;

	// 	while (beg < end) {
	// 		mi = Math.floor((beg + end) / 2);
	// 		i = data.lastIndexOf('\n', mi) + 1;

	// 		mis = data.substr(i, tlen);
	// 		if (text < mis) end = i - 1;
	// 		else if (text > mis) beg = data.indexOf('\n', mi + 1) + 1;
	// 		else return data.substring(i, data.indexOf('\n', mi + 1));
	// 	}
	// 	return null;
	// }
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<!-- 1. 🔥 Firebase App -->
<FirebaseApp {auth} {firestore}>
	<Collection ref={'words'} let:data={words} let:ref={wordsRef}>
		{#each words as word}
			<b>{word.id}</b>
			<p>{word.mnemonic}</p>
			<br />
		{/each}
		<input type="text" bind:value={newWord} placeholder="word: 好" />

		<h1>{newWord}</h1>
		<input type="text" bind:value={newMnemonic} placeholder="mnemonic: woman with child is good" />
		<button
			on:click={async () => {
				console.log('hi');
				await fetch('https://chinese-dictionary.azurewebsites.net/' + newWord)
					.then((response) => response.json())
					.then((data) => console.log(data))
					.catch((e) => console.error(e));
			}}>Find word</button
		>
		<button on:click={addWord}> Add Word </button>
	</Collection>
</FirebaseApp>

<!-- 
<section>
	<h1>
		<span class="welcome">
			<picture>
				<source srcset={welcome} type="image/webp" />
				<img src={welcome_fallback} alt="Welcome" />
			</picture>
		</span>

		to your new<br />SvelteKit app
	</h1>

	<h2>
		try editing <strong>src/routes/+page.svelte</strong>
	</h2>

	<Counter />
</section>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 0.6;
	}

	h1 {
		width: 100%;
	}

	.welcome {
		display: block;
		position: relative;
		width: 100%;
		height: 0;
		padding: 0 0 calc(100% * 495 / 2048) 0;
	}

	.welcome img {
		position: absolute;
		width: 100%;
		height: 100%;
		top: 0;
		display: block;
	}
</style> -->
<style>
	input {
		display: block;
		width: 100%;
	}
</style>
