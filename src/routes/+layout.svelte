<script lang="ts">
	import favicon from '$lib/assets/favicon.svg';
	import '../app.css';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

<div
	class="min-h-screen bg-white px-10 py-10 font-sans text-black md:px-20 md:py-20 dark:bg-black dark:text-white"
>
	<header class="flex flex-wrap items-baseline justify-between gap-4">
		<a href="/" class="text-sm font-medium tracking-tight">Justin Gene Karl</a>
		<nav class="flex gap-6 text-[11px] uppercase tracking-widest text-gray-400 dark:text-gray-500">
			<a href="/work" class="transition-colors hover:text-black dark:hover:text-white">Work</a>
			<a href="/about" class="transition-colors hover:text-black dark:hover:text-white">About</a>
			<a href="/contact" class="transition-colors hover:text-black dark:hover:text-white">Contact</a>
		</nav>
	</header>

	<main class="mt-16">
		{@render children()}
	</main>

	<footer class="mt-20 text-[10px] text-gray-300 dark:text-gray-600">&copy; 2026 Justin Gene Karl</footer>
</div>
