<script lang="ts">
	import logo from '$lib/assets/favicon.png';
	import longLogo from '$lib/assets/Kontrast-long.png';

	let mobileMenuOpen = $state(false);
</script>

<main
	class="min-h-screen text-slate-100"
>
	<nav class="fade-in px-4 pt-6 sm:px-6" style="--fade-delay: 70ms">
		<div class="mx-auto grid w-full max-w-6xl grid-cols-[1fr_auto_1fr] items-center px-4 py-3 sm:px-6">
			<a
				href="/"
				class="hidden h-10 items-center justify-self-start transition-opacity duration-200 hover:opacity-90 sm:inline-flex"
			>
				<img src={longLogo} alt="Kontrast Web Logo" class="h-8 w-auto" />
			</a>

			<a
				href="/"
				class="inline-flex items-center transition-opacity duration-200 hover:opacity-90 sm:hidden"
			>
				<img src={logo} alt="Kontrast Web Logo" class="brand-favicon w-14 sm:w-16" />
			</a>

			<button
				type="button"
				class="col-start-3 inline-flex justify-self-end rounded-md p-2 text-slate-300 transition hover:bg-slate-800 hover:text-white sm:hidden"
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

			<ul class="col-start-2 hidden h-10 items-center gap-7 text-sm font-semibold uppercase tracking-[0.12em] text-slate-300 sm:flex lg:gap-10">
				<li><a href="/" class="nav-link inline-flex h-10 items-center">Home</a></li>
				<li><a href="/" class="nav-link inline-flex h-10 items-center">Portfolio</a></li>
				<li><a href="/" class="nav-link inline-flex h-10 items-center">About</a></li>
				<li><a href="/" class="nav-link inline-flex h-10 items-center">Contact</a></li>
			</ul>
		</div>

		{#if mobileMenuOpen}
			<ul id="mobile-nav" class="fade-in mx-4 mt-3 flex flex-col gap-2 p-3 text-sm font-semibold uppercase tracking-[0.08em] text-slate-300 sm:hidden" style="--fade-delay: 120ms">
				<li><a href="/" class="mobile-nav-link">Home</a></li>
				<li><a href="/" class="mobile-nav-link">Portfolio</a></li>
				<li><a href="/" class="mobile-nav-link">About</a></li>
				<li><a href="/" class="mobile-nav-link">Contact</a></li>
			</ul>
		{/if}
	</nav>

	<section class="fade-in mx-auto flex w-full max-w-5xl flex-col items-center gap-8 px-6 py-24 text-center sm:gap-12 sm:px-10 md:py-28" style="--fade-delay: 230ms">
		<p class="fade-in text-xs font-semibold uppercase tracking-[0.28em] text-cyan-300/80" style="--fade-delay: 250ms">Web solutions</p>
		<h1 class="fade-in text-3xl font-black leading-tight tracking-tight sm:text-4xl md:text-6xl" style="--fade-delay: 280ms">
			Welcome to <span class="kontrast-word">Kontrast</span>
		</h1>
		<p class="fade-in max-w-2xl text-base leading-relaxed text-slate-300 sm:text-lg md:text-xl" style="--fade-delay: 330ms">Kontrast is under construction. Please check back later.</p>
	</section>
</main>
