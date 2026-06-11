<script lang="ts">
	import { onMount } from 'svelte';

	const images = [
		{ file: 'tokyo-alley.jpg', alt: 'Narrow neon-lit alley, Shinjuku, Tokyo', ratio: '2 / 3' },
		{
			file: 'tokyo-crossing.jpg',
			alt: 'Pedestrians crossing in the rain, Shibuya, Tokyo',
			ratio: '3 / 2'
		},
		{ file: 'amsterdam-canal.jpg', alt: 'Bicycles along a quiet canal, Amsterdam', ratio: '3 / 2' },
		{
			file: 'amsterdam-morning.jpg',
			alt: 'Early morning light on a cobblestone street, Amsterdam',
			ratio: '2 / 3'
		},
		{
			file: 'coffee-window-seat.jpg',
			alt: 'Window-seat portrait in a corner coffee shop',
			ratio: '4 / 5'
		},
		{
			file: 'coffee-counter.jpg',
			alt: 'Steam rising over the counter at a small espresso bar',
			ratio: '3 / 2'
		}
	];

	let track: HTMLDivElement;
	let imageEls: HTMLImageElement[] = [];
	let currentIndex = $state(0);
	let canScrollLeft = $derived(currentIndex > 0);

	function syncIndexFromScroll() {
		if (!track) return;
		const maxScroll = track.scrollWidth - track.clientWidth;
		if (track.scrollLeft >= maxScroll - 1) {
			currentIndex = images.length - 1;
			return;
		}
		if (track.scrollLeft <= 1) {
			currentIndex = 0;
			return;
		}
		let closest = 0;
		let closestDist = Infinity;
		imageEls.forEach((el, i) => {
			if (!el) return;
			const dist = Math.abs(el.offsetLeft - track.scrollLeft);
			if (dist < closestDist) {
				closestDist = dist;
				closest = i;
			}
		});
		currentIndex = closest;
	}

	function updateFades() {
		if (!track) return;
		const trackRect = track.getBoundingClientRect();
		for (const el of imageEls) {
			if (!el) continue;
			const rect = el.getBoundingClientRect();
			const visibleLeft = Math.max(rect.left, trackRect.left);
			const visibleRight = Math.min(rect.right, trackRect.right);
			const visibleWidth = Math.max(0, visibleRight - visibleLeft);
			const fraction = rect.width > 0 ? visibleWidth / rect.width : 1;

			let opacity = 1;
			if (rect.left < trackRect.left - 1) {
				// exiting on the left edge - fade out steeply so it "disappears"
				const threshold = 0.6;
				opacity = fraction >= 1 ? 1 : Math.max(0, (fraction - threshold) / (1 - threshold));
			}
			el.style.opacity = String(opacity);
		}
	}

	function handleScroll() {
		syncIndexFromScroll();
		updateFades();
	}

	function scrollToIndex(i: number) {
		const clamped = Math.max(0, Math.min(images.length - 1, i));
		currentIndex = clamped;
		const el = imageEls[clamped];
		if (el && track) {
			track.scrollTo({ left: el.offsetLeft, behavior: 'smooth' });
		}
	}

	function handleLeftClick() {
		scrollToIndex(currentIndex - 1);
	}

	function handleRightClick() {
		if (currentIndex < images.length - 1) {
			scrollToIndex(currentIndex + 1);
		} else {
			scrollToIndex(0);
		}
	}

	function arrowCursor(direction: 'left' | 'right') {
		const path = direction === 'left' ? 'M21 10 L15 18 L21 26' : 'M15 10 L21 18 L15 26';
		const svg = `<svg xmlns='http://www.w3.org/2000/svg' width='36' height='36' viewBox='0 0 36 36'><circle cx='18' cy='18' r='17' fill='white' fill-opacity='0.85'/><path d='${path}' fill='none' stroke='black' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'/></svg>`;
		return `url("data:image/svg+xml,${encodeURIComponent(svg)}") 18 18, pointer`;
	}

	onMount(() => {
		updateFades();
	});
</script>

<div class="relative py-2">
	<div
		bind:this={track}
		onscroll={handleScroll}
		class="no-scrollbar relative flex gap-6 overflow-x-auto scroll-smooth"
	>
		{#each images as image, i}
			<img
				bind:this={imageEls[i]}
				src={`/images/${image.file}`}
				alt={image.alt}
				style="aspect-ratio: {image.ratio};"
				class="h-[80vh] w-auto flex-shrink-0 border border-black/5 object-cover grayscale contrast-110 transition-opacity duration-300 ease-out"
			/>
		{/each}
	</div>

	{#if canScrollLeft}
		<div
			role="button"
			tabindex="0"
			onclick={handleLeftClick}
			onkeydown={(e) => e.key === 'Enter' && handleLeftClick()}
			class="absolute top-0 left-0 h-full w-1/2"
			style="cursor: {arrowCursor('left')};"
			aria-label="Scroll left"
		></div>
	{/if}

	<div
		role="button"
		tabindex="0"
		onclick={handleRightClick}
		onkeydown={(e) => e.key === 'Enter' && handleRightClick()}
		class="absolute top-0 right-0 h-full w-1/2"
		style="cursor: {arrowCursor('right')};"
		aria-label="Scroll right"
	></div>
</div>
