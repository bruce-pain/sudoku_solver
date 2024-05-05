<script lang="ts">
	export let grid;

	const seperator_indexes: number[] = [2, 5];
</script>

<div class="grid">
	{#each grid as row, rowIndex}
		<div class="row" class:rowDivide={seperator_indexes.includes(rowIndex)}>
			{#each row as cell, cellIndex}
				<div class="cell" class:colDivide={seperator_indexes.includes(cellIndex)}>
					{#if cell !== 0}
						{cell}
					{/if}
				</div>
			{/each}
		</div>
	{/each}
</div>
