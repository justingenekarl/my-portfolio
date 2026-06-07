<script lang="ts">
	import favicon from '$lib/assets/favicon.svg';
	import '../app.css';
	import { fade } from 'svelte/transition';
	import { page } from '$app/state';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

<div class="min-h-screen bg-white px-10 py-10 font-sans text-black md:px-20 md:py-20">
	<header class="-mt-9 -ml-5 flex flex-wrap items-center justify-between gap-4">
		<a href="/" class="text-base font-light tracking-[0.3em]">Justin Gene Karl</a>
		<nav class="flex gap-6 text-[9px] font-light uppercase tracking-widest text-gray-400">
			<a href="/work" class="transition-colors hover:text-black">Work</a>
			<a href="/about" class="transition-colors hover:text-black">About</a>
			<a href="/contact" class="transition-colors hover:text-black">Contact</a>
		</nav>
	</header>

	<main class="mt-16">
		{#key page.url.pathname}
			<div in:fade={{ duration: 200 }}>
				{@render children()}
			</div>
		{/key}
	</main>

	<footer class="mt-20 text-[10px] text-gray-300">&copy; 2026 Justin Gene Karl</footer>
</div>
