<script lang="ts">
	const images = [
		{ file: 'tokyo-alley.jpg', alt: 'Narrow neon-lit alley, Shinjuku, Tokyo' },
		{ file: 'tokyo-crossing.jpg', alt: 'Pedestrians crossing in the rain, Shibuya, Tokyo' },
		{ file: 'amsterdam-canal.jpg', alt: 'Bicycles along a quiet canal, Amsterdam' },
		{ file: 'amsterdam-morning.jpg', alt: 'Early morning light on a cobblestone street, Amsterdam' },
		{ file: 'coffee-window-seat.jpg', alt: 'Window-seat portrait in a corner coffee shop' },
		{ file: 'coffee-counter.jpg', alt: 'Steam rising over the counter at a small espresso bar' }
	];

	let openIndex: number | null = $state(null);

	function open(i: number) {
		openIndex = i;
	}

	function close() {
		openIndex = null;
	}

	function next() {
		if (openIndex === null) return;
		openIndex = (openIndex + 1) % images.length;
	}

	function prev() {
		if (openIndex === null) return;
		openIndex = (openIndex - 1 + images.length) % images.length;
	}

	function handleKeydown(e: KeyboardEvent) {
		if (openIndex === null) return;
		if (e.key === 'Escape') close();
		if (e.key === 'ArrowRight') next();
		if (e.key === 'ArrowLeft') prev();
	}
</script>

<svelte:window onkeydown={handleKeydown} />

<div class="grid grid-cols-1 gap-8 pt-4 pb-16 md:grid-cols-2">
	{#each images as image, i}
		<button onclick={() => open(i)} class="block w-full cursor-pointer text-left">
			<img
				src={`/images/${image.file}`}
				alt={image.alt}
				class="w-full aspect-[3/2] border border-black/5 object-cover grayscale contrast-110"
			/>
		</button>
	{/each}
</div>

{#if openIndex !== null}
	<div
		class="fixed inset-0 z-50 flex items-center justify-center bg-white/95 px-6"
		onclick={close}
		role="dialog"
		aria-modal="true"
		aria-label="Image viewer"
	>
		<button
			onclick={(e) => {
				e.stopPropagation();
				prev();
			}}
			class="absolute left-6 text-gray-400/40 transition-colors hover:text-black md:left-16"
			aria-label="Previous image"
		>
			<svg width="28" height="28" viewBox="0 0 24 24" fill="none">
				<path
					d="M15 6 L9 12 L15 18"
					stroke="currentColor"
					stroke-width="1.5"
					stroke-linecap="round"
					stroke-linejoin="round"
				/>
			</svg>
		</button>

		<img
			src={`/images/${images[openIndex].file}`}
			alt={images[openIndex].alt}
			class="max-h-[80vh] max-w-[80vw] object-contain grayscale contrast-110"
			onclick={(e) => e.stopPropagation()}
		/>

		<button
			onclick={(e) => {
				e.stopPropagation();
				next();
			}}
			class="absolute right-6 text-gray-400/40 transition-colors hover:text-black md:right-16"
			aria-label="Next image"
		>
			<svg width="28" height="28" viewBox="0 0 24 24" fill="none">
				<path
					d="M9 6 L15 12 L9 18"
					stroke="currentColor"
					stroke-width="1.5"
					stroke-linecap="round"
					stroke-linejoin="round"
				/>
			</svg>
		</button>
	</div>
{/if}
