<script lang="ts">
	import logo from '$lib/assets/favicon.png';
</script>

<main
	class="min-h-screen bg-slate-900 text-slate-100"
>
	<nav class="pt-10 pl-5 ">
		<div class="flex w-full items-center justify-start gap-8 px-6">
			<a
				href="/"
				class="inline-flex transition duration-200 hover:-translate-y-0.5 hover:scale-[1.02]"
			>
				<img src={logo} alt="Kontrast Web Logo" class="w-14 sm:w-16 md:w-16" />
			</a>

			<ul class="flex w-fit items-center gap-5 text-xl font-medium text-slate-300 sm:gap-10 sm:text-md">
				<li><a href="/" class="transition hover:text-white">Home</a></li>
				<li><a href="/" class="transition hover:text-white">Portfolio</a></li>
				<li><a href="/" class="transition hover:text-white">About</a></li>
				<li><a href="/" class="transition hover:text-white">Contact</a></li>
			</ul>
		</div>
	</nav>

	<section class="mx-auto flex w-full max-w-5xl flex-col items-center gap-6 px-6 py-20 text-center sm:gap-10 sm:px-20">
        <h1 class="text-2xl font-bold sm:text-2xl md:text-4xl">Welcome to Kontrast Web</h1>
        <p class="text-lg text-slate-400 sm:text-xl md:text-xl">Kontrast web is under construction. Please check back later.</p>
	</section>
</main>
