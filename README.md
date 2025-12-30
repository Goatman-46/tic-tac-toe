<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Tic Tac Toe - Human or Bot</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      margin-top: 40px;
    }
    h1 {
      margin-bottom: 10px;
    }
    select, button {
      padding: 8px;
      font-size: 16px;
      margin-bottom: 15px;
    }
    #board {
      display: grid;
      grid-template-columns: repeat(3, 100px);
      grid-template-rows: repeat(3, 100px);
      gap: 5px;
    }
    .cell {
      width: 100px;
      height: 100px;
      font-size: 36px;
      text-align: center;
      line-height: 100px;
      border: 2px solid #333;
      cursor: pointer;
    }
    .cell:hover {
      background-color: #eee;
    }
    #status {
      margin-top: 20px;
      font-size: 20px;
    }
  </style>
</head>
<body>
  <h1>Tic Tac Toe</h1>

  <select id="gameMode">
    <option value="human">2 Player (Human vs Human)</option>
    <option value="ai">1 Player (You vs AI)</option>
  </select>

  <button onclick="startGame()">Start Game</button>

  <div id="board"></div>
  <div id="status"></div>
  <button onclick="startGame()">Restart</button>

  <script>
    const board = document.getElementById("board");
    const status = document.getElementById("status");
    const modeSelect = document.getElementById("gameMode");

    let currentPlayer = "X";
    let gameBoard = ["", "", "", "", "", "", "", "", ""];
    let gameActive = true;
    let vsAI = false;

    const winPatterns = [
      [0, 1, 2], [3, 4, 5], [6, 7, 8],
      [0, 3, 6], [1, 4, 7], [2, 5, 8],
      [0, 4, 8], [2, 4, 6],
    ];

    function checkWinner() {
      for (const [a, b, c] of winPatterns) {
        if (gameBoard[a] && gameBoard[a] === gameBoard[b] && gameBoard[b] === gameBoard[c]) {
          gameActive = false;
          status.textContent = `${gameBoard[a]} wins!`;
          return;
        }
      }
      if (!gameBoard.includes("")) {
        gameActive = false;
        status.textContent = "It's a draw!";
      }
    }

    function aiMove() {
      if (!gameActive) return;
      const emptyCells = gameBoard.map((val, i) => val === "" ? i : null).filter(i => i !== null);
      if (emptyCells.length === 0) return;
      const move = emptyCells[Math.floor(Math.random() * emptyCells.length)];
      gameBoard[move] = "O";
      document.getElementById(`cell-${move}`).textContent = "O";
      checkWinner();
      if (gameActive) {
        currentPlayer = "X";
        status.textContent = "Your turn (X)";
      }
    }

    function handleCellClick(index) {
      if (!gameActive || gameBoard[index]) return;
      gameBoard[index] = currentPlayer;
      document.getElementById(`cell-${index}`).textContent = currentPlayer;
      checkWinner();

      if (gameActive) {
        if (vsAI) {
          if (currentPlayer === "X") {
            currentPlayer = "O";
            status.textContent = "AI is thinking...";
            setTimeout(aiMove, 500);
          }
        } else {
          currentPlayer = currentPlayer === "X" ? "O" : "X";
          status.textContent = `${currentPlayer}'s turn`;
        }
      }
    }

    function createBoard() {
      board.innerHTML = "";
      for (let i = 0; i < 9; i++) {
        const cell = document.createElement("div");
        cell.className = "cell";
        cell.id = `cell-${i}`;
        cell.addEventListener("click", () => handleCellClick(i));
        board.appendChild(cell);
      }
    }

    function startGame() {
      gameBoard = ["", "", "", "", "", "", "", "", ""];
      currentPlayer = "X";
      gameActive = true;
      vsAI = modeSelect.value === "ai";
      createBoard();
      status.textContent = vsAI ? "Your turn (X)" : "X's turn";
    }

    // Load the game on page load
    startGame();
  </script>
</body>
</html>
