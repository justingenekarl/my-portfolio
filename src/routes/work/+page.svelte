<script lang="ts">
	const images = [
		{ file: 'tokyo-alley.jpg', alt: 'Narrow neon-lit alley, Shinjuku, Tokyo' },
		{ file: 'tokyo-crossing.jpg', alt: 'Pedestrians crossing in the rain, Shibuya, Tokyo' },
		{ file: 'amsterdam-canal.jpg', alt: 'Bicycles along a quiet canal, Amsterdam' },
		{ file: 'amsterdam-morning.jpg', alt: 'Early morning light on a cobblestone street, Amsterdam' },
		{ file: 'coffee-window-seat.jpg', alt: 'Window-seat portrait in a corner coffee shop' },
		{ file: 'coffee-counter.jpg', alt: 'Steam rising over the counter at a small espresso bar' }
	];
</script>

<div class="grid grid-cols-1 gap-8 pt-4 pb-16 md:grid-cols-2">
	{#each images as image}
		<img
			src={`/images/${image.file}`}
			alt={image.alt}
			class="w-full aspect-[3/2] border border-black/5 object-cover grayscale contrast-110 dark:border-white/10"
		/>
	{/each}
</div>
