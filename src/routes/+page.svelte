<script lang="ts">
	type State = 'start' | 'input' | 'solving' | 'finished';

	let state: State = 'start';
	$: grid = [
		[0, 0, 0, 0, 0, 0, 0, 0, 0],
		[0, 0, 0, 0, 0, 0, 0, 0, 0],
		[0, 0, 0, 0, 0, 0, 0, 0, 0],
		[0, 0, 0, 0, 0, 0, 0, 0, 0],
		[0, 0, 0, 0, 0, 0, 0, 0, 0],
		[0, 0, 0, 0, 0, 0, 0, 0, 0],
		[0, 0, 0, 0, 0, 0, 0, 0, 0],
		[0, 0, 0, 0, 0, 0, 0, 0, 0],
		[0, 0, 0, 0, 0, 0, 0, 0, 0]
	];
	// let timeout = 0;
	// $: grid = [
	// 	[5, 3, 0, 0, 7, 0, 0, 0, 0],
	// 	[6, 0, 0, 1, 9, 5, 0, 0, 0],
	// 	[0, 9, 8, 0, 0, 0, 0, 6, 0],
	// 	[8, 0, 0, 0, 6, 0, 0, 0, 3],
	// 	[4, 0, 0, 8, 0, 3, 0, 0, 1],
	// 	[7, 0, 0, 0, 2, 0, 0, 0, 6],
	// 	[0, 6, 0, 0, 0, 0, 2, 8, 0],
	// 	[0, 0, 0, 4, 1, 9, 0, 0, 5],
	// 	[0, 0, 0, 0, 8, 0, 0, 7, 9]
	// ];

	function sleep(ms) {
		return new Promise((resolve) => setTimeout(resolve, ms));
	}

	// solving algo

	function check_digit(digit: number, y: number, x: number) {
		// check horizontal
		for (let i = 0; i < 9; i++) {
			if (grid[y][i] === digit) {
				return false;
			}
		}

		// check vertical
		for (let i = 0; i < 9; i++) {
			if (grid[i][x] === digit) {
				return false;
			}
		}

		// check square (3 x 3)
		let square_origin_x = Math.floor(x / 3) * 3;
		let square_origin_y = Math.floor(y / 3) * 3;

		for (let i = 0; i < 3; i++) {
			for (let j = 0; j < 3; j++) {
				if (grid[square_origin_y + i][square_origin_x + j] === digit) {
					return false;
				}
			}
		}

		return true;
	}

	function find_empty() {
		for (let y = 0; y < 9; y++) {
			for (let x = 0; x < 9; x++) {
				if (grid[y][x] === 0) {
					return [y, x];
				}
			}
		}

		return null;
	}

	async function solve() {
		let empty_position = find_empty();
		let y: number;
		let x: number;

		if (empty_position === null) {
			return true;
		} else {
			[y, x] = empty_position;
		}

		for (let i = 1; i < 10; i++) {
			if (check_digit(i, y, x)) {
				// await sleep(timeout).then(() => {
					grid[y][x] = i;
				// });

				if (await solve()) {
					return true;
				}

				// await sleep(timeout).then(() => {
					grid[y][x] = 0;
				// });
			}
		}

		return false;
	}

	async function start() {
		console.log(grid);
		await solve();
		state = 'finished';
	}

	function updateCell(e: InputEvent, row, col)
	{
		const value = e.data
		grid[row][col] = value !== null ? parseInt(value) : 0
	}

	function resetBoard() {
		state = 'input';
		grid = [
			[0, 0, 0, 0, 0, 0, 0, 0, 0],
			[0, 0, 0, 0, 0, 0, 0, 0, 0],
			[0, 0, 0, 0, 0, 0, 0, 0, 0],
			[0, 0, 0, 0, 0, 0, 0, 0, 0],
			[0, 0, 0, 0, 0, 0, 0, 0, 0],
			[0, 0, 0, 0, 0, 0, 0, 0, 0],
			[0, 0, 0, 0, 0, 0, 0, 0, 0],
			[0, 0, 0, 0, 0, 0, 0, 0, 0],
			[0, 0, 0, 0, 0, 0, 0, 0, 0]
		];
	}

	$: if (state === 'solving') {
		start();
	}

	// $: console.log(grid);
</script>

{#if state === 'start'}
	<h1>Sudoku Solver</h1>
	<button on:click={() => (state = 'input')}>Start</button>
{/if}

{#if state === 'input'}
	<div class="grid">
		{#each grid as row, rowIndex}
			<div class="row">
				{#each row as cell, cellIndex}
					<input
					class="cell-input"
					on:input={(e) => updateCell(e, rowIndex, cellIndex)}
					/>
				{/each}
			</div>
		{/each}
	</div>
	<h1>Edit the Sudoku board</h1>
	<button on:click={() => (state = 'solving')}>Solve</button>
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

{#if state === 'finished'}
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
	<h1>Solved !</h1>
	<button on:click={() => resetBoard()}>Solve another</button>
{/if}
