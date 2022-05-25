<script lang="ts">
	let gameState: number[][] = Array(9)
		.fill(undefined)
		.map(() => Array(9).fill(-1))
	let lastState = [...gameState]
	let turn = 0

	function placePoint(x: number, y: number) {
		if (gameState[x][y] !== -1) {
			return
		}

		let futureState = JSON.parse(JSON.stringify(gameState))
		futureState[x][y] = turn % 2
		futureState = validateState(futureState, x, y)
		gameState = futureState
		turn++
	}

	function validateState(state: number[][], x: number, y: number): number[][] {
		// Check self-capturing move
		if (validateGroup(state, x, y, turn % 2).length !== 0) {
			console.log('Self-capturing move')
			return gameState
		}

		console.log('No errors here!')
		return state
	}

	function validateGroup(
		state: number[][],
		x: number,
		y: number,
		color: number,
		points: Point[] = []
	): Point[] {
		if (state[x][y] === -1) return []

		points = [...points, { x, y }]
		for (let i = 0; i < 4; i++) {
			const newX = [-1, 0, 1, 0][i] + x
			const newY = [0, 1, 0, -1][i] + y
			if (
				newX >= 0 &&
				newX < state.length &&
				newY >= 0 &&
				newY < state[0].length
			) {
				if (
					state[newX][newY] !== (color + 1) % 2 &&
					!points.find((point) => point.x === newX && point.y === newY)
				) {
					const newPoints = validateGroup(state, newX, newY, color, points)
					if (newPoints.length === 0) return []
					points = newPoints
				}
			}
		}

		console.log('Done checking group')
		return points
	}

	interface Point {
		x: number
		y: number
	}

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
				{#if gameState[rowIndex][pointIndex] === -1}
					<div
						class="point {pointClasses(rowIndex, pointIndex)}"
						on:click={() => {
							placePoint(rowIndex, pointIndex)
						}}
					/>
				{:else}
					<div class="point {pointClasses(rowIndex, pointIndex)}">
						<div
							class="stone {gameState[rowIndex][pointIndex] === 0
								? 'black'
								: 'white'}"
						/>
					</div>
				{/if}
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
		padding: 16px;
	}

	.stone {
		width: 100%;
		height: 100%;
		border-radius: 50%;
	}

	.stone.black {
		background-color: #323231;
	}

	.stone.white {
		background-color: #ededed;
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
