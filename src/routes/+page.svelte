<script lang="ts">
	import logo from '$lib/assets/favicon.png';
	import longLogo from '$lib/assets/Kontrast-long.png';

	let mobileMenuOpen = $state(false);
</script>

<main
	class="min-h-screen bg-slate-900 text-slate-100"
>
	<nav class="pt-10">
		<div class="flex w-full items-center px-6">
			<a
				href="/"
				class="inline-flex transition duration-200 hover:-translate-y-0.5 hover:scale-[1.02]"
			>
				<img src={longLogo} alt="Kontrast Web Logo" class="w-32 sm:hidden" />
				<img src={logo} alt="Kontrast Web Logo" class="hidden w-16 sm:block" />
			</a>

			<button
				type="button"
				class="ml-auto inline-flex items-center justify-center rounded-md p-2 text-slate-300 transition hover:bg-slate-800 hover:text-white sm:hidden"
				onclick={() => (mobileMenuOpen = !mobileMenuOpen)}
				aria-expanded={mobileMenuOpen}
				aria-controls="mobile-nav"
			>
				<span class="sr-only">Toggle navigation menu</span>
				<svg class="h-6 w-6" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
					{#if mobileMenuOpen}
						<path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
					{:else}
						<path stroke-linecap="round" stroke-linejoin="round" d="M4 6h16M4 12h16M4 18h16" />
					{/if}
				</svg>
			</button>

			<ul class="ml-6 hidden w-fit items-center gap-5 text-md font-medium text-slate-300 sm:flex sm:gap-10 sm:text-md">
				<li><a href="/" class="transition hover:text-white">Home</a></li>
				<li><a href="/" class="transition hover:text-white">Portfolio</a></li>
				<li><a href="/" class="transition hover:text-white">About</a></li>
				<li><a href="/" class="transition hover:text-white">Contact</a></li>
			</ul>
		</div>

		{#if mobileMenuOpen}
			<ul id="mobile-nav" class="mt-4 flex flex-col gap-3 px-6 text-base font-medium text-slate-300 sm:hidden">
				<li><a href="/" class="block rounded-md px-2 py-1 transition hover:bg-slate-800 hover:text-white">Home</a></li>
				<li><a href="/" class="block rounded-md px-2 py-1 transition hover:bg-slate-800 hover:text-white">Portfolio</a></li>
				<li><a href="/" class="block rounded-md px-2 py-1 transition hover:bg-slate-800 hover:text-white">About</a></li>
				<li><a href="/" class="block rounded-md px-2 py-1 transition hover:bg-slate-800 hover:text-white">Contact</a></li>
			</ul>
		{/if}
	</nav>

	<section class="mx-auto flex w-full max-w-5xl flex-col items-center gap-6 px-6 py-20 text-center sm:gap-10 sm:px-20">
        <h1 class="text-2xl font-bold sm:text-2xl md:text-4xl">Welcome to Kontrast Web</h1>
        <p class="text-lg text-slate-400 sm:text-xl md:text-xl">Kontrast web is under construction. Please check back later.</p>
	</section>
</main>
