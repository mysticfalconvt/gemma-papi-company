<script lang="ts">
	import { onDestroy, onMount } from 'svelte';
	import { writable } from 'svelte/store';

	let canvas: HTMLCanvasElement | undefined;
	let audioContext: AudioContext | undefined;
	let analyser: AnalyserNode | undefined;
	let dataArray: Uint8Array;
	let animationFrameId: number;
	let source: MediaStreamAudioSourceNode;

	export let displayType = writable<'waveform' | 'circle'>('waveform');

	async function setupMicrophone() {
		try {
			const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
			audioContext = new window.AudioContext();
			analyser = audioContext.createAnalyser();
			analyser.fftSize = 2048;
			dataArray = new Uint8Array(analyser.frequencyBinCount);
			source = audioContext.createMediaStreamSource(stream);
			source.connect(analyser);
			draw();
		} catch (err) {
			console.error('Error accessing microphone:', err);
		}
	}

	function draw() {
		if (!canvas || !analyser) return;
		const ctx = canvas.getContext('2d');
		if (!ctx) return;
		const { width, height } = canvas;

		function render() {
			if (!ctx) return;
			ctx.clearRect(0, 0, width, height);
			analyser!.getByteTimeDomainData(dataArray);
			ctx.lineWidth = 2;
			ctx.strokeStyle = '#4caf50';

			displayType.subscribe((type) => {
				if (type === 'waveform') {
					drawWaveform(ctx, width, height);
				} else {
					drawCircle(ctx, width, height);
				}
			});

			animationFrameId = requestAnimationFrame(render);
		}
		render();
	}

	function drawWaveform(ctx: CanvasRenderingContext2D, width: number, height: number) {
		ctx.beginPath();
		const sliceWidth = width / dataArray.length;
		let x = 0;
		for (let i = 0; i < dataArray.length; i++) {
			const v = dataArray[i] / 128.0;
			const y = (v * height) / 2;
			i === 0 ? ctx.moveTo(x, y) : ctx.lineTo(x, y);
			x += sliceWidth;
		}
		ctx.stroke();
	}

	function drawCircle(ctx: CanvasRenderingContext2D, width: number, height: number) {
		const centerX = width / 2;
		const centerY = height / 2;
		const radius = Math.min(width, height) / 3;
		ctx.beginPath();
		for (let i = 0; i < dataArray.length; i++) {
			const angle = (i / dataArray.length) * Math.PI * 2;
			const v = dataArray[i] / 255;
			const x = centerX + Math.cos(angle) * (radius + v * 50);
			const y = centerY + Math.sin(angle) * (radius + v * 50);
			i === 0 ? ctx.moveTo(x, y) : ctx.lineTo(x, y);
		}
		ctx.closePath();
		ctx.stroke();
	}

	onMount(() => {
		setupMicrophone();
	});

	onDestroy(() => {
		if (audioContext) {
			audioContext.close();
		}
		cancelAnimationFrame(animationFrameId);
	});
</script>

<div class="flex flex-col items-center">
	<canvas
		bind:this={canvas}
		width={600}
		height={300}
		class="rounded border border-gray-700 shadow-lg"
	></canvas>
	<div class="mt-4">
		<button
			class="mx-2 rounded bg-blue-500 px-4 py-2 text-white"
			on:click={() => displayType.set('waveform')}>Waveform</button
		>
		<button
			class="mx-2 rounded bg-purple-500 px-4 py-2 text-white"
			on:click={() => displayType.set('circle')}>Circle</button
		>
	</div>
</div>
