<script lang="ts">
	import { onMount } from 'svelte';
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

	let firstPageShell: HTMLElement | null = null;
	let nextSection: HTMLElement | null = null;
	let scrollLocked = false;
	let touchStartY = 0;
	let touchStartX = 0;
	let touchStartTime = 0;

	function inFirstPageRange() {
		if (!firstPageShell) return false;
		const threshold = firstPageShell.offsetTop + firstPageShell.offsetHeight - 140;
		return window.scrollY < threshold;
	}

	function inSecondPageRange() {
		if (!nextSection) return false;
		return window.scrollY >= nextSection.offsetTop - 140;
	}

	function autoScrollDown() {
		if (!nextSection || scrollLocked) return;
		scrollLocked = true;
		nextSection.scrollIntoView({ behavior: 'smooth', block: 'start' });
		window.setTimeout(() => {
			scrollLocked = false;
		}, 700);
	}

	function autoScrollUp() {
		if (!firstPageShell || scrollLocked) return;
		scrollLocked = true;
		firstPageShell.scrollIntoView({ behavior: 'smooth', block: 'start' });
		window.setTimeout(() => {
			scrollLocked = false;
		}, 700);
	}

	function onWheel(event: WheelEvent) {
		if (mobileMenuOpen || scrollLocked) return;

		if (event.deltaY > 12 && inFirstPageRange()) {
			event.preventDefault();
			autoScrollDown();
			return;
		}

		if (event.deltaY < -12 && inSecondPageRange()) {
			event.preventDefault();
			autoScrollUp();
		}
	}

	function onTouchStart(event: TouchEvent) {
		const touch = event.touches[0];
		if (!touch) return;
		touchStartX = touch.clientX;
		touchStartY = touch.clientY;
		touchStartTime = performance.now();
	}

	function onTouchEnd(event: TouchEvent) {
		if (mobileMenuOpen || scrollLocked) return;

		const touch = event.changedTouches[0];
		if (!touch) return;

		const deltaY = touchStartY - touch.clientY;
		const deltaX = Math.abs(touchStartX - touch.clientX);
		const duration = performance.now() - touchStartTime;
		const isIntentionalSwipe = Math.abs(deltaY) > 64 && deltaX < 36 && duration < 460;

		if (!isIntentionalSwipe) return;

		if (deltaY > 0 && inFirstPageRange()) {
			autoScrollDown();
			return;
		}

		if (deltaY < 0 && inSecondPageRange()) {
			autoScrollUp();
		}
	}

	onMount(() => {
		window.addEventListener('wheel', onWheel, { passive: false });
		window.addEventListener('touchstart', onTouchStart, { passive: true });
		window.addEventListener('touchend', onTouchEnd, { passive: true });

		return () => {
			window.removeEventListener('wheel', onWheel);
			window.removeEventListener('touchstart', onTouchStart);
			window.removeEventListener('touchend', onTouchEnd);
		};
	});
</script>

<main class="relative flex min-h-screen flex-col overflow-x-hidden bg-slate-950 text-slate-100">
	<Particles className="particles-layer fixed inset-0 z-0" quantity={70} refresh={false} />
	<div bind:this={firstPageShell} class="first-page-shell relative z-10 min-h-screen">
		<div class="boxes-shell" aria-hidden="true">
			<BackgroundBoxes
				class="boxes-layer absolute inset-0 h-full w-full"
				cellSize={gridPreset[gridDensity].cellSize}
				overscan={gridPreset[gridDensity].overscan}
				tileColor="rgba(34, 211, 238, 0.24)"
			/>
			<Lights class="lights-layer" direction="top" />
			<div class="boxes-fade"></div>
		</div>

		<nav class="fade-in relative z-10 px-3 pt-4 sm:px-6 md:px-10" class:mobile-nav-open={mobileMenuOpen} style="--fade-delay: 60ms">
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

		<section class="fade-in relative z-10 mx-auto flex w-full max-w-5xl min-h-[calc(100svh-6rem)] flex-col items-center justify-center gap-8 px-3 py-8 text-center sm:gap-10 sm:px-6 md:px-10 md:py-16 lg:py-24 xl:px-0" style="--fade-delay: 180ms">
			<p class="text-xs sm:text-sm font-semibold uppercase tracking-[0.24em] text-cyan-300/80">Web solutions</p>
			<h1 class="max-w-3xl text-2xl font-black leading-tight tracking-tight sm:text-4xl md:text-5xl lg:text-6xl xl:text-7xl">
			 Welcome to <span class="kontrast-word">Kontrast</span>
			</h1>
			<p class="max-w-xl text-xs leading-relaxed text-slate-300 sm:text-base md:text-lg lg:text-xl xl:text-2xl">Kontrast is under construction. Please check back later.</p>
			<div class="flex flex-col items-center gap-3 sm:flex-row sm:gap-6 mt-6 w-full max-w-md mx-auto">
			</div>

			<a
				href="#next-page"
				class="group absolute bottom-6 left-1/2 inline-flex -translate-x-1/2 flex-col items-center gap-2 text-slate-300 transition hover:text-cyan-300 focus:outline-none focus-visible:ring-2 focus-visible:ring-cyan-300 focus-visible:ring-offset-2 focus-visible:ring-offset-slate-900"
				aria-label="Scroll to next section"
			>
				<svg class="scroll-arrow h-6 w-6" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
					<path d="M6 9l6 6 6-6" />
				</svg>
			</a>
		</section>
	</div>

	<section bind:this={nextSection} id="next-page" class="relative z-10 mx-auto flex min-h-screen w-full max-w-5xl flex-col items-center justify-center gap-6 px-4 py-16 text-center sm:px-6 md:px-10">
		<p class="text-xs font-semibold uppercase tracking-[0.24em] text-cyan-300/80 sm:text-sm">Next page</p>
		<h2 class="max-w-3xl text-2xl font-black leading-tight tracking-tight text-slate-100 sm:text-4xl md:text-5xl">You made it to the next section</h2>
		<p class="max-w-2xl text-sm leading-relaxed text-slate-300 sm:text-base md:text-lg">This section acts like the next page. Keep scrolling or add more content blocks here.</p>
		<a href="/new-page" class="mt-2 inline-flex items-center rounded-lg border border-cyan-300/40 bg-cyan-400/10 px-5 py-2.5 text-sm font-semibold text-cyan-100 transition hover:bg-cyan-300/20 focus:outline-none focus-visible:ring-2 focus-visible:ring-cyan-300 focus-visible:ring-offset-2 focus-visible:ring-offset-slate-900">Open dedicated new page</a>
	</section>
</main>


