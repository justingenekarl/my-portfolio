<script lang="ts">
	import { onMount } from 'svelte';

	const images = [
		{
			file: 'tokyo-shibuya-neon.jpg',
			alt: 'Vibrant pink and orange neon signs lighting up a street in Shibuya, Tokyo',
			ratio: '2 / 3'
		},
		{
			file: 'fuji-lake.jpg',
			alt: 'Snow-capped Mount Fuji rising above clouds over a lake',
			ratio: '3 / 2'
		},
		{
			file: 'osaka-dotonbori.jpg',
			alt: 'Neon signs lining the street in Dotonbori, Osaka',
			ratio: '2 / 3'
		},
		{
			file: 'tokyo-station.jpg',
			alt: 'Train station platform, Tokyo',
			ratio: '3 / 4'
		},
		{
			file: 'paris-eiffel.jpg',
			alt: 'People walking on the street near the Eiffel Tower, Paris',
			ratio: '3 / 2'
		},
		{
			file: 'osaka-alley.jpg',
			alt: 'Narrow alley lined with signs, Osaka',
			ratio: '2 / 3'
		},
		{
			file: 'kyoto-bamboo.jpg',
			alt: 'Path through the Arashiyama bamboo forest, Kyoto',
			ratio: '2 / 3'
		},
		{
			file: 'tokyo-rain.jpg',
			alt: 'Woman walking with an umbrella in the rain, Shibuya, Tokyo',
			ratio: '2 / 3'
		},
		{
			file: 'prague-street.jpg',
			alt: 'Cobblestone street in Old Town, Prague',
			ratio: '3 / 4'
		},
		{
			file: 'tokyo-vending.jpg',
			alt: 'Glowing vending machine on a street corner at night, Yurakucho, Tokyo',
			ratio: '3 / 2'
		}
	];

	let track: HTMLDivElement;
	let imageEls: HTMLImageElement[] = [];
	let currentIndex = $state(0);
	let canScrollLeft = $derived(currentIndex > 0);

	function syncIndexFromScroll() {
		if (!track) return;
		const viewportCenter = track.scrollLeft + track.clientWidth / 2;
		let closest = 0;
		let closestDist = Infinity;
		imageEls.forEach((el, i) => {
			if (!el) return;
			const elCenter = el.offsetLeft + el.offsetWidth / 2;
			const dist = Math.abs(elCenter - viewportCenter);
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

	function centerOffset(el: HTMLImageElement) {
		return el.offsetLeft + el.offsetWidth / 2 - track.clientWidth / 2;
	}

	function scrollToIndex(i: number, behavior: ScrollBehavior = 'smooth') {
		const clamped = Math.max(0, Math.min(images.length - 1, i));
		const toEl = imageEls[clamped];
		if (toEl && track) {
			track.scrollTo({ left: centerOffset(toEl), behavior });
		}
		currentIndex = clamped;
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
		scrollToIndex(0, 'instant');
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
				class="h-[80vh] w-auto flex-shrink-0 border border-black/5 object-cover transition-opacity duration-300 ease-out [filter:sepia(0.18)_saturate(1.2)_contrast(0.92)_brightness(1.05)]"
			/>
		{/each}
		<div class="w-[50vw] flex-shrink-0" aria-hidden="true"></div>
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
