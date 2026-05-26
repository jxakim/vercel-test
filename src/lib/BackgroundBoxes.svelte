<script lang="ts">
	import { onMount } from 'svelte';
	import { browser } from '$app/environment';

	let {
		rows = 11,
		cols = 22,
		autoFit = true,
		cellSize = 42,
		overscan = 2,
		class: className = '',
		tileColor = 'rgba(56, 189, 248, 0.22)'
	} = $props<{
		rows?: number;
		cols?: number;
		autoFit?: boolean;
		cellSize?: number;
		overscan?: number;
		class?: string;
		tileColor?: string;
	}>();

	let containerEl: HTMLDivElement;

	function getInitialGrid() {
		if (!autoFit) {
			return {
				rows: rows,
				cols: cols
			};
		}

		if (!browser) {
			return {
				rows: 11,
				cols: 22
			};
		}

		const safeCellSize = Math.max(24, cellSize);
		return {
			cols: Math.max(8, Math.ceil(window.innerWidth / safeCellSize) + overscan),
			rows: Math.max(6, Math.ceil(window.innerHeight / safeCellSize) + overscan)
		};
	}

	const initialGrid = getInitialGrid();
	let resolvedRows = $state(initialGrid.rows);
	let resolvedCols = $state(initialGrid.cols);

	$effect(() => {
		if (autoFit) return;
		resolvedRows = rows;
		resolvedCols = cols;
	});

	function updateGrid(width: number, height: number) {
		if (!autoFit) {
			resolvedRows = rows;
			resolvedCols = cols;
			return;
		}

		const safeCellSize = Math.max(24, cellSize);
		resolvedCols = Math.max(8, Math.ceil(width / safeCellSize) + overscan);
		resolvedRows = Math.max(6, Math.ceil(height / safeCellSize) + overscan);
	}

	onMount(() => {
		if (!containerEl) return;

		updateGrid(window.innerWidth, window.innerHeight);

		const rect = containerEl.getBoundingClientRect();
		updateGrid(rect.width, rect.height);

		const observer = new ResizeObserver((entries) => {
			const entry = entries[0];
			if (!entry) return;
			updateGrid(entry.contentRect.width, entry.contentRect.height);
		});

		observer.observe(containerEl);

		const onWindowResize = () => {
			updateGrid(window.innerWidth, window.innerHeight);
		};

		window.addEventListener('resize', onWindowResize);

		return () => {
			observer.disconnect();
			window.removeEventListener('resize', onWindowResize);
		};
	});

	const rowIndices = $derived(Array.from({ length: resolvedRows }, (_, i) => i));
	const colIndices = $derived(Array.from({ length: resolvedCols }, (_, i) => i));
</script>

<div class={`background-boxes ${className}`.trim()} style={`--tile-hover: ${tileColor};`} aria-hidden="true" bind:this={containerEl}>
	{#each rowIndices as row}
		<div class="background-boxes__row" style={`--row-index:${row};`}>
			{#each colIndices as col}
				<div class="background-boxes__tile" style={`--col-index:${col};`}></div>
			{/each}
		</div>
	{/each}
</div>
