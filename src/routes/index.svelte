<script lang="ts">
	let gameState = Array(9).fill(Array(9).fill(-1))

	function pointClasses(row: number, col: number) {
		let type = ''
		let position = ''
		if (row === 0 || row === 8 || col === 0 || col === 8) {
			if ((row === 0 || row === 8) && (col === 0 || col === 8)) {
				type = 'corner'
			} else {
				type = 'edge'
			}
		} else {
			type = 'middle'
			return type
		}

		if (row === 0) {
			position += 't'
		} else if (row === 8) {
			position += 'b'
		}
		if (col === 0) {
			position += 'l'
		} else if (col === 8) {
			position += 'r'
		}

		return `${type}-${position}`
	}
</script>

<svelte:head>
	<title>Go</title>
	<link rel="stylesheet" href="../src/assets/app.css" />
</svelte:head>

<main>
	<div class="board">
		{#each gameState as row, rowIndex}
			{#each row as point, pointIndex}
				<div class="point {pointClasses(rowIndex, pointIndex)}">
					{rowIndex * 9 + pointIndex}
				</div>
			{/each}
		{/each}
	</div>
</main>

<style>
	main {
		width: calc(100vmin - 16px);
		height: calc(100vmin - 16px);
		padding: 8px;
	}

	.board {
		display: grid;
		width: 100%;
		height: 100%;
		grid-template-rows: repeat(9, 1fr);
		grid-template-columns: repeat(9, 1fr);
		background-color: #debc95;
		border-radius: 16px;
	}

	.point {
		background-size: cover;
		display: flex;
		justify-content: center;
		align-items: center;
	}

	.middle {
		background-image: url('../assets/middle.svg');
	}

	.corner-tl {
		background-image: url('../assets/corner-tl.svg');
	}

	.corner-tr {
		background-image: url('../assets/corner-tr.svg');
	}

	.corner-bl {
		background-image: url('../assets/corner-bl.svg');
	}

	.corner-br {
		background-image: url('../assets/corner-br.svg');
	}

	.edge-t {
		background-image: url('../assets/edge-t.svg');
	}

	.edge-b {
		background-image: url('../assets/edge-b.svg');
	}

	.edge-l {
		background-image: url('../assets/edge-l.svg');
	}

	.edge-r {
		background-image: url('../assets/edge-r.svg');
	}
</style>
