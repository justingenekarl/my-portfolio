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

<div class="flex min-h-screen flex-col bg-white px-10 pt-10 pb-6 font-sans text-black md:px-20 md:pt-20 md:pb-10">
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

	<footer class="mt-auto flex items-center justify-between pt-12 text-[10px] text-gray-300">
		<span>&copy; 2026 Justin Gene Karl</span>
		<a href="/privacy" class="transition-colors hover:text-black">Privacy Policy</a>
	</footer>
</div>
