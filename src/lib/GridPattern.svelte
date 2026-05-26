<script lang="ts">
	type GridSquare = [number, number];

	let {
		squares = [],
		fillColor = 'rgb(156 163 175 / 0.3)',
		class: className = ''
	} = $props<{
		squares?: GridSquare[];
		fillColor?: string;
		class?: string;
	}>();

	const cellSize = 40;
	const patternId = 'grid-pattern';
</script>

<svg
	class={className}
	viewBox="0 0 1600 900"
	preserveAspectRatio="xMidYMid slice"
	aria-hidden="true"
>
	<defs>
		<pattern id={patternId} width={cellSize} height={cellSize} patternUnits="userSpaceOnUse">
			<path d={`M ${cellSize} 0 L 0 0 0 ${cellSize}`} fill="none" stroke="rgba(148, 163, 184, 0.12)" stroke-width="1" />
		</pattern>
	</defs>

	<rect width="100%" height="100%" fill={`url(#${patternId})`} />

	{#each squares as [column, row]}
		<rect
			x={column * cellSize}
			y={row * cellSize}
			width={cellSize}
			height={cellSize}
			fill={fillColor}
		/>
	{/each}
</svg>
