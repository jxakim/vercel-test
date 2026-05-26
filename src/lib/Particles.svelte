<script lang="ts">
	import { onMount } from 'svelte';

	type Circle = {
		x: number;
		y: number;
		translateX: number;
		translateY: number;
		size: number;
		alpha: number;
		targetAlpha: number;
		dx: number;
		dy: number;
		magnetism: number;
	};

	let {
		className = '',
		quantity = 100,
		staticity = 50,
		ease = 50,
		size = 0.4,
		refresh = true,
		color = '#ffffff',
		vx = 0,
		vy = 0
	} = $props<{
		className?: string;
		quantity?: number;
		staticity?: number;
		ease?: number;
		size?: number;
		refresh?: boolean;
		color?: string;
		vx?: number;
		vy?: number;
	}>();

	let canvasRef: HTMLCanvasElement;
	let canvasContainerRef: HTMLDivElement;
	let context: CanvasRenderingContext2D | null = null;
	let circles: Circle[] = [];
	let frameId = 0;
	let rgb: number[] = [255, 255, 255];
	let reduceMotion = false;
	let mouse = { x: 0, y: 0 };
	let canvasSize = { w: 0, h: 0 };
	let dpr = 1;

	function hexToRgb(hex: string): number[] {
		let normalized = hex.replace('#', '');

		if (normalized.length === 3) {
			normalized = normalized
				.split('')
				.map((char) => char + char)
				.join('');
		}

		const hexInt = Number.parseInt(normalized, 16);
		if (Number.isNaN(hexInt)) {
			return [255, 255, 255];
		}

		const red = (hexInt >> 16) & 255;
		const green = (hexInt >> 8) & 255;
		const blue = hexInt & 255;
		return [red, green, blue];
	}

	function circleParams(): Circle {
		const x = Math.floor(Math.random() * canvasSize.w);
		const y = Math.floor(Math.random() * canvasSize.h);
		const translateX = 0;
		const translateY = 0;
		const particleSize = Math.floor(Math.random() * 2) + size;
		const alpha = 0;
		const targetAlpha = Number.parseFloat((Math.random() * 0.6 + 0.1).toFixed(2));
		const dx = (Math.random() - 0.5) * 0.1;
		const dy = (Math.random() - 0.5) * 0.1;
		const magnetism = 0.1 + Math.random() * 4;

		return {
			x,
			y,
			translateX,
			translateY,
			size: particleSize,
			alpha,
			targetAlpha,
			dx,
			dy,
			magnetism
		};
	}

	function resizeCanvas() {
		if (!canvasContainerRef || !canvasRef || !context) return;

		dpr = Math.min(window.devicePixelRatio || 1, 2);
		circles.length = 0;
		canvasSize.w = canvasContainerRef.offsetWidth;
		canvasSize.h = canvasContainerRef.offsetHeight;
		canvasRef.width = Math.floor(canvasSize.w * dpr);
		canvasRef.height = Math.floor(canvasSize.h * dpr);
		canvasRef.style.width = `${canvasSize.w}px`;
		canvasRef.style.height = `${canvasSize.h}px`;
		context.setTransform(dpr, 0, 0, dpr, 0, 0);
	}

	function clearContext() {
		if (context) {
			context.clearRect(0, 0, canvasSize.w, canvasSize.h);
		}
	}

	function drawCircle(circle: Circle, update = false) {
		if (!context) return;

		const { x, y, translateX, translateY, size: particleSize, alpha } = circle;
		context.translate(translateX, translateY);
		context.beginPath();
		context.arc(x, y, particleSize, 0, 2 * Math.PI);
		context.fillStyle = `rgba(${rgb.join(', ')}, ${alpha})`;
		context.fill();
		context.setTransform(dpr, 0, 0, dpr, 0, 0);

		if (!update) {
			circles.push(circle);
		}
	}

	function drawParticles() {
		clearContext();
		for (let i = 0; i < quantity; i += 1) {
			drawCircle(circleParams());
		}
	}

	function remapValue(value: number, start1: number, end1: number, start2: number, end2: number): number {
		const remapped = ((value - start1) * (end2 - start2)) / (end1 - start1) + start2;
		return remapped > 0 ? remapped : 0;
	}

	function animate() {
		if (reduceMotion) {
			drawParticles();
			return;
		}

		clearContext();
		circles.forEach((circle, index) => {
			const edge = [
				circle.x + circle.translateX - circle.size,
				canvasSize.w - circle.x - circle.translateX - circle.size,
				circle.y + circle.translateY - circle.size,
				canvasSize.h - circle.y - circle.translateY - circle.size
			];

			const closestEdge = edge.reduce((a, b) => Math.min(a, b));
			const remapClosestEdge = Number.parseFloat(remapValue(closestEdge, 0, 20, 0, 1).toFixed(2));

			if (remapClosestEdge > 1) {
				circle.alpha += 0.02;
				if (circle.alpha > circle.targetAlpha) {
					circle.alpha = circle.targetAlpha;
				}
			} else {
				circle.alpha = circle.targetAlpha * remapClosestEdge;
			}

			circle.x += circle.dx + vx;
			circle.y += circle.dy + vy;
			circle.translateX += (mouse.x / (staticity / circle.magnetism) - circle.translateX) / ease;
			circle.translateY += (mouse.y / (staticity / circle.magnetism) - circle.translateY) / ease;

			drawCircle(circle, true);

			if (
				circle.x < -circle.size ||
				circle.x > canvasSize.w + circle.size ||
				circle.y < -circle.size ||
				circle.y > canvasSize.h + circle.size
			) {
				circles.splice(index, 1);
				drawCircle(circleParams());
			}
		});

		frameId = window.requestAnimationFrame(animate);
	}

	function onMouseMove(event: MouseEvent) {
		if (!canvasRef) return;

		const rect = canvasRef.getBoundingClientRect();
		const { w, h } = canvasSize;
		const x = event.clientX - rect.left - w / 2;
		const y = event.clientY - rect.top - h / 2;
		const inside = x < w / 2 && x > -w / 2 && y < h / 2 && y > -h / 2;

		if (inside) {
			mouse.x = x;
			mouse.y = y;
		}
	}

	function startParticles() {
		cancelAnimationFrame(frameId);
		drawParticles();
		animate();
	}

	onMount(() => {
		context = canvasRef.getContext('2d');
		if (!context) return;

		const media = window.matchMedia('(prefers-reduced-motion: reduce)');
		reduceMotion = media.matches;
		const motionListener = (event: MediaQueryListEvent) => {
			reduceMotion = event.matches;
			startParticles();
		};

		rgb = hexToRgb(color);
		resizeCanvas();
		startParticles();

		window.addEventListener('resize', resizeCanvas);
		window.addEventListener('mousemove', onMouseMove);
		media.addEventListener('change', motionListener);

		return () => {
			cancelAnimationFrame(frameId);
			window.removeEventListener('resize', resizeCanvas);
			window.removeEventListener('mousemove', onMouseMove);
			media.removeEventListener('change', motionListener);
		};
	});

	$effect(() => {
		rgb = hexToRgb(color);
	});

	$effect(() => {
		if (!canvasRef || !context) return;
		if (!refresh) return;

		resizeCanvas();
		startParticles();
	});
</script>

<div class={className} bind:this={canvasContainerRef} aria-hidden="true">
	<canvas bind:this={canvasRef} class="size-full"></canvas>
</div>

<style>
	.size-full {
		width: 100%;
		height: 100%;
	}
</style>
