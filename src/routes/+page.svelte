<script lang="ts">
	type State = 'start' | 'solving';

	let state: State = 'start';
	let grid = [
		[5, 3, 0, 0, 7, 0, 0, 0, 0],
		[6, 0, 0, 1, 9, 5, 0, 0, 0],
		[0, 9, 8, 0, 0, 0, 0, 6, 0],
		[8, 0, 0, 0, 6, 0, 0, 0, 3],
		[4, 0, 0, 8, 0, 3, 0, 0, 1],
		[7, 0, 0, 0, 2, 0, 0, 0, 6],
		[0, 6, 0, 0, 0, 0, 2, 8, 0],
		[0, 0, 0, 4, 1, 9, 0, 0, 5],
		[0, 0, 0, 0, 8, 0, 0, 7, 9]
	];
</script>

{#if state === 'start'}
	<h1>Sudoku Solver</h1>
	<button on:click={() => (state = "solving")}>Start</button>
{/if}

{#if state === 'solving'}
	<div class="grid">
		{#each grid as row}
			<div class="row">
				{#each row as cell}
					<div class="cell">
					{#if cell !== 0}
						{cell}
					{/if}
					</div>
				{/each}
			</div>
		{/each}
	</div>
{/if}

