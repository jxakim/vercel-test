<script lang="ts">
	type Direction = 'top' | 'bottom';

	let {
		direction = 'bottom',
		class: className = ''
	} = $props<{
		direction?: Direction;
		class?: string;
	}>();

	const gradientDirection = $derived(direction === 'top' ? '0deg' : '180deg');
	const positionClass = $derived(direction === 'top' ? 'top-[-220px]' : 'bottom-[-220px]');
</script>

<div class={`lights-root ${className}`.trim()} aria-hidden="true">
	<div
		class={`lights-blur ${positionClass}`}
		style={`background: conic-gradient(from ${gradientDirection} at 50% 50%, rgba(14, 165, 233, 0.65) 0deg, rgba(34, 211, 238, 0.6) 160deg, rgba(250, 204, 21, 0.45) 1turn);`}
	></div>
</div>

<style>
	.lights-root {
		position: absolute;
		inset: 0;
		overflow: hidden;
		pointer-events: none;
	}

	.lights-blur {
		position: absolute;
		left: -10%;
		width: 120%;
		height: 64%;
		filter: blur(85px);
		opacity: 0.2;
		animation: lights-drift 14s ease-in-out infinite;
	}

	@keyframes lights-drift {
		0%,
		100% {
			transform: translateY(0px) scale(1);
			opacity: 0.18;
		}
		50% {
			transform: translateY(-14px) scale(1.04);
			opacity: 0.24;
		}
	}

	@media (prefers-reduced-motion: reduce) {
		.lights-blur {
			animation: none;
		}
	}
</style>
