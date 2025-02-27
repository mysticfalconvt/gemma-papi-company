<script lang="ts">
	export let title: string;
	export let text: string;
</script>

<div
	class="transform rounded-2xl bg-gradient-to-r from-blue-400 via-green-400 to-teal-400 p-6 text-white shadow-xl transition duration-300 hover:scale-105"
>
	<h2 class="text-2xl font-bold">{title}</h2>
	<p class="mt-2">{text}</p>
</div>
