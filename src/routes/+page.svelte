<script lang="ts">
	import Card from '../components/card.svelte';

	let message = 'Welcome to The Gemma and Papi World Company!';

	const cards = [
		{ title: 'Our Mission', text: 'Bringing joy and innovation to people of all ages.' },
		{ title: 'Join Us', text: 'Be part of our exciting journey and make a difference.' },
		{ title: 'Contact', text: 'Get in touch to collaborate or learn more about us.' }
	];
</script>

<div
	class="flex min-h-screen flex-col items-center justify-center bg-gradient-to-r from-yellow-400 via-pink-500 to-purple-600 p-6 text-white"
>
	<h1 class="text-center text-5xl font-extrabold drop-shadow-lg">{message}</h1>
	<p class="mt-4 max-w-2xl text-center text-lg">
		Explore a world of creativity, fun, and endless possibilities with us!
	</p>

	<div class="mt-8 grid w-full max-w-4xl grid-cols-1 gap-6 sm:grid-cols-2 lg:grid-cols-3">
		{#each cards as card}
			<Card title={card.title} text={card.text} />
		{/each}
	</div>
</div>
