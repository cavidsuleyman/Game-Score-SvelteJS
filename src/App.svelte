<script>
	let rows = [];
	let score = 0;
	let gameOver = false;
	let name = prompt("Write your name:");

	function generateRow(){
		let row = new Array(4).fill("white");
		let pos = Math.trunc(Math.random() * 4);
		row[pos] = 'black';
		return row;
	}

	function fillRows(){
		for(let i=0; i<4; i++){
			rows.push(generateRow());
		}
	}

	function tapped(i,j){
		if(i != rows.length - 1 || rows[i][j] == "white"){
			gameOver = true;
			rows[i][j] = "red";
		}else{
			rows.splice(i);
			rows = [generateRow(), ...rows];
			score++
		}
	}

	function restartGame(){
		score = 0;
		name = prompt("Write your name:");
		gameOver = false;
		rows = [];
		fillRows();
	}

	fillRows();
</script>

<main class="app">
	<div class="header">
		<h4>Win Game</h4>
		<p>Score: {score}</p>
		<p>Name: {name || "User"}</p>
	</div>
	<div class="game">
		{#each rows as row,i}
			<div class="row">
				{#each row as box,j}
					<div class={"box " + box} on:click={() => tapped(i,j)}></div>
				{/each}
			</div>
		{/each}
	</div>
	{#if gameOver}
		<div class="result">
			<h2>Game Over</h2>
			<p>Score: {score}</p>
			<p>Name: {name || "User"}</p>
			<button on:click={restartGame}>Restart Game</button>
		</div>
	{/if}
</main>

<style>
	.app{
		position: fixed;
		top: 0;
		left: 50%;
		transform: translateX(-50%);
		width: 100%;
		height: 100%;
		max-width: 400px;
	}
	.app .header{
		position: sticky;
		top: 0;
		left: 0;
		width: 100%;
		height: 50px;
		background: #8B1874;
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 0 20px;
		color: #fff;
	}
	.app .game{
		width: 100%;
		height: calc(100% - 50px);
		background: white;
	}
	.app .game .row{
		display: flex;
		width: 100%;
		height: calc(100% / 4);
	}
	.app .game .row .box {
		flex: 1;
		border: 1px solid #555;
		cursor: pointer;
	}
	.app .game .row .box.black{
		background: #B71375;
	}
	.app .game .row .box.red{
		animation: blinkRed 500ms ease-in-out infinite;
	}
	@keyframes blinkRed{
		0%,100%{
			background: white;
		}
		50%{
			background: #FF6969;
		}
	}
	.result{
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background: rgba(0, 0, 0, 0.8);
		display: flex;
		justify-content: center;
		align-items: center;
		flex-direction: column;
		gap: 10px;
	}
	.result h2{
		font-size: 40px;
		color: white;
	}
	.result p{
		font-size: 20px;
		margin-bottom: 10px;
		color: #eee;
	}
	.result button{
		cursor: pointer;
		padding: 10px 20px;
		font-size: 16px;
		border: 2px solid white;
		color: #8B1874;
		font-weight: 600;
		outline: none;
		border-radius: 5px;
	}
</style>