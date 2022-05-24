<svelte:head>
 	<title>Go</title>
	<link rel="stylesheet" href="../app.css">
</svelte:head>

<script lang='ts'>
 	let gameState = Array(9).fill(Array(9).fill(-1))
</script>

<main>
<div class='board'>
 	{#each gameState as row, rowIndex}
 	 	{#each row as point, pointIndex}
 	 	 	<div class='point'>{rowIndex * 9 + pointIndex}</div>
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
 	}

 	.point {
 	 	background-image: url('../assets/middle.svg');
 	 	background-size: cover;
 	}
</style>
