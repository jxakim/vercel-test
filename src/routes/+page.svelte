<script lang="ts">
	import longLogo from '$lib/assets/Kontrast-long.png';
	import BackgroundBoxes from '$lib/BackgroundBoxes.svelte';
	import Lights from '$lib/Lights.svelte';
	import Particles from '$lib/Particles.svelte';

	let mobileMenuOpen = $state(false);

	type GridDensity = 'tight' | 'balanced' | 'airy';
	const gridDensity: GridDensity = 'balanced';

	const gridPreset = {
		tight: { cellSize: 34, overscan: 4 },
		balanced: { cellSize: 44, overscan: 3 },
		airy: { cellSize: 56, overscan: 2 }
	} as const;
</script>

<main
	class="relative min-h-screen overflow-hidden text-slate-100"
>
	<div class="interactive-cut-bg" aria-hidden="true"></div>
	<Particles className="particles-layer absolute inset-0 z-0" refresh={true} />
	<div class="boxes-shell" aria-hidden="true">
		<BackgroundBoxes
			class="boxes-layer absolute inset-0 h-full w-full"
			cellSize={gridPreset[gridDensity].cellSize}
			overscan={gridPreset[gridDensity].overscan}
			tileColor="rgba(34, 211, 238, 0.24)"
		/>
		<Lights class="lights-layer" direction="bottom" />
		<div class="boxes-fade"></div>
	</div>

	<nav class="fade-in relative z-10 px-4 pt-6 sm:px-6" class:mobile-nav-open={mobileMenuOpen} style="--fade-delay: 60ms">
		<div class="mx-auto grid w-full max-w-6xl grid-cols-[1fr_auto_1fr] items-center px-4 py-3 sm:px-6">
			<a
				href="/"
				class="col-start-1 hidden h-10 items-center justify-self-start transition-opacity duration-200 hover:opacity-90 sm:inline-flex"
			>
				<img src={longLogo} alt="Kontrast Web Logo" class="h-8 w-auto" />
			</a>

			<a
				href="/"
				class="col-start-1 inline-flex items-center justify-self-start transition-opacity duration-200 hover:opacity-90 sm:hidden"
			>
				<img src={longLogo} alt="Kontrast Web Logo" class="h-7 w-auto" />
			</a>

			<button
				type="button"
				class="menu-toggle col-start-3 inline-flex justify-self-end sm:hidden"
				class:menu-toggle-open={mobileMenuOpen}
				onclick={() => (mobileMenuOpen = !mobileMenuOpen)}
				aria-expanded={mobileMenuOpen}
				aria-controls="mobile-nav"
			>
				<span class="sr-only">Toggle navigation menu</span>
				<span class="menu-toggle-line"></span>
				<span class="menu-toggle-line"></span>
				<span class="menu-toggle-line"></span>
			</button>

			<ul class="col-start-2 hidden h-10 items-center gap-7 text-sm font-medium text-slate-300 sm:flex lg:gap-10">
				<li><a href="/" class="nav-link inline-flex h-10 items-center">Home</a></li>
				<li><a href="/" class="nav-link inline-flex h-10 items-center">Portfolio</a></li>
				<li><a href="/" class="nav-link inline-flex h-10 items-center">About</a></li>
				<li><a href="/" class="nav-link inline-flex h-10 items-center">Contact</a></li>
			</ul>
		</div>

		{#if mobileMenuOpen}
			<ul id="mobile-nav" class="mobile-menu-panel fade-in fixed inset-x-0 top-[4.85rem] z-[80] mx-3 flex flex-col gap-1 p-2 text-left text-sm font-medium text-slate-200 sm:hidden" style="--fade-delay: 80ms">
				<li><a href="/" class="mobile-nav-link">Home</a></li>
				<li><a href="/" class="mobile-nav-link">Portfolio</a></li>
				<li><a href="/" class="mobile-nav-link">About</a></li>
				<li><a href="/" class="mobile-nav-link">Contact</a></li>
			</ul>
		{/if}
	</nav>

	<section class="fade-in relative z-10 mx-auto flex w-full max-w-5xl flex-col items-center gap-8 px-6 py-16 text-center sm:gap-10 sm:px-10 md:py-24" style="--fade-delay: 180ms">
		<p class="text-xs font-semibold uppercase tracking-[0.24em] text-cyan-300/80">Web solutions</p>
		<h1 class="max-w-4xl text-3xl font-black leading-tight tracking-tight sm:text-4xl md:text-6xl">
			Welcome to <span class="kontrast-word">Kontrast</span>
		</h1>
		<p class="max-w-2xl text-base leading-relaxed text-slate-300 sm:text-lg md:text-xl">Kontrast is under construction. Please check back later.</p>
	</section>
</main>
