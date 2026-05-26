<script lang="ts">
	let {
		rows = 11,
		cols = 22,
		class: className = '',
		tileColor = 'rgba(56, 189, 248, 0.22)'
	} = $props<{
		rows?: number;
		cols?: number;
		class?: string;
		tileColor?: string;
	}>();

	const rowIndices = $derived(Array.from({ length: rows }, (_, i) => i));
	const colIndices = $derived(Array.from({ length: cols }, (_, i) => i));
</script>

<div class={`background-boxes ${className}`.trim()} style={`--tile-hover: ${tileColor};`} aria-hidden="true">
	{#each rowIndices as row}
		<div class="background-boxes__row" style={`--row-index:${row};`}>
			{#each colIndices as col}
				<div class="background-boxes__tile" style={`--col-index:${col};`}></div>
			{/each}
		</div>
	{/each}
</div>
