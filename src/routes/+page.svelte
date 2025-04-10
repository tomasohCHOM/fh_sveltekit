<script>
  let board = $state([
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
  ]);
  let playerTurn = $state(0);
  let isGameOver = $state(false);
  let winner = $state("");

  function checkBoardState() {
    // Check rows and columns
    for (let i = 0; i < 3; i++) {
      if (board[i][0] && board[i][0] === board[i][1] && board[i][1] === board[i][2]) {
        isGameOver = true;
        winner = board[i][0];
        return;
      }
      if (board[0][i] && board[0][i] === board[1][i] && board[1][i] === board[2][i]) {
        isGameOver = true;
        winner = board[0][i];
        return;
      }
    }

    // Check diagonals
    if (board[0][0] && board[0][0] === board[1][1] && board[1][1] === board[2][2]) {
      isGameOver = true;
      winner = board[0][0];
      return;
    }
    if (board[0][2] && board[0][2] === board[1][1] && board[1][1] === board[2][0]) {
      isGameOver = true;
      winner = board[0][2];
      return;
    }

    // Check for a tie
    if (board.flat().every((cell) => cell !== "")) {
      isGameOver = true;
      winner = "Tie";
    }
  }

  function handleCellClick(row, col) {
    // Don't do anything if the game is over or this cell is already occupied
    if (board[row][col] || isGameOver) {
      return;
    }
    // Player takes this cell
    if (playerTurn === 0) {
      board[row][col] = "X";
    } else {
      board[row][col] = "O";
    }
    // Check for a win or tie
    checkBoardState();
    if (!isGameOver) {
      // Toggle betwen player turns (0, then 1, then 0...)
      playerTurn ^= 1;
    }
  }
</script>

<svelte:head>
  <title>Tic Tac Toe</title>
</svelte:head>

<main>
  <h1>Tic Tac Toe</h1>
  <div class="board">
    {#each board as row, rowIndex}
      <div class="row">
        {#each row as cell, colIndex}
          <button
            onclick={() => handleCellClick(rowIndex, colIndex)}
            class:red={cell === "X"}
            class:blue={cell === "O"}
            class="cell"
          >
            {cell}
          </button>
        {/each}
      </div>
    {/each}
  </div>
  <p>
    {#if isGameOver}
      {#if winner === "Tie"}
        It's a tie!
      {:else}
        Player {winner} has won the game!
      {/if}
    {:else}
      Player {playerTurn}'s turn
    {/if}
  </p>
</main>

<style>
  /* DEFAULT STYLES */
  * {
    font-family: Verdana;
  }
  main {
    padding: 4rem 2rem;
    max-width: 1024px;
    margin: 0 auto;
    text-align: center;
    display: grid;
    align-items: center;
    justify-content: center;
    gap: 1rem;
  }
  h1 {
    font-size: 2rem;
    font-weight: bold;
    margin-bottom: 1rem;
  }
  button {
    cursor: pointer;
  }
  p {
    text-align: center;
    font-size: 1.125rem;
  }

  /* Styles we will create */
  .board {
    display: flex;
    flex-direction: column;
  }
  .row {
    display: flex;
  }
  .cell {
    width: 5rem;
    height: 5rem;
    font-size: 1.5rem;
    font-weight: bold;
    border-style: solid;
    background: none;
    cursor: pointer;
  }
  .red {
    color: red;
  }
  .blue {
    color: blue;
  }
  @media screen and (min-width: 768px) {
    .cell {
      width: 6rem;
      height: 6rem;
    }
  }
</style>
