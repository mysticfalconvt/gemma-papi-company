<script lang="ts">
	type Card = {
		title: string;
		text: string;
		link?: string;
	};
	export let cardData: Card[];
</script>

<div class="card mt-8 grid w-full max-w-4xl grid-cols-1 gap-6 sm:grid-cols-2 lg:grid-cols-3">
	{#each cardData as card}
		<a href={card.link} class=" cursor-pointer">
			<div
				class="transform rounded-2xl bg-gradient-to-r from-blue-400 via-green-400 to-pink-400 p-6 text-white shadow-xl transition duration-300 hover:scale-105"
			>
				<h2 class="text-2xl font-bold">{card.title}</h2>
				<p class="mt-2">{card.text}</p>
			</div>
		</a>
	{/each}
</div>

<style>
	@media (min-width: 1024px) {
		/* lg breakpoint */
		.card > :only-child {
			grid-column: 2;
		}
	}
</style>
