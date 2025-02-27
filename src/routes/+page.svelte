<script lang="ts">
	import AudioDisplay from '../components/audioDisplay.svelte';
	import Card from '../components/card.svelte';

	let message = 'Welcome to The Gemma and Papi World Company!';

	const cards = [
		// { title: 'Our Mission', text: 'Bringing joy and innovation to people of all ages.' },
		{
			title: 'Contact',
			text: 'Get in touch to collaborate or learn more about us.',
			link: 'mailto:gemma@boskind.tech'
		}
	];
</script>

<div
	class="flex min-h-screen flex-col items-center justify-center bg-gradient-to-r from-green-500 via-purple-800 to-blue-600 p-6 text-yellow-200"
>
	<h1 class="text-center text-5xl font-extrabold drop-shadow-lg">{message}</h1>
	<p class="mt-4 max-w-2xl text-center text-lg">
		Explore a world of creativity, fun, and endless possibilities with us!
	</p>
	<p class="mt-4 max-w-2xl text-center text-lg">We sell posters that we make!!</p>
	<!-- this next one needs to be a nice looking callout showcase -->
	<div
		class="gap-2rounded-2xl mt-4 mb-4 flex max-w-2xl flex-col items-center rounded-2xl border-4 border-yellow-200 bg-gradient-to-tl from-green-500 via-purple-800 to-blue-600 p-5 text-center text-lg font-bold"
	>
		We are doing a special 1 day only lemonade sale!
		<div class="text-md w-min rounded-md border-2 border-yellow-200 p-1">February 28th!!</div>
		$1.00
	</div>
	<AudioDisplay />
	<Card cardData={cards} />
</div>
