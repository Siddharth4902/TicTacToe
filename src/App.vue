<template>
  <div id="app">
    <h1>X and 0</h1>
    <div class="board">
      <div
        v-for="(cell, index) in board"
        :key="index"
        class="cell"
        :class="{ 'cell-x': cell === 'X', 'cell-o': cell === 'O'}"
        @click="onClick(index)"
      >
        {{ cell }}
      </div>
    </div>
    <div v-if="winner" class="status">
      {{ winner === 'Tie' ? 'It\'s a tie!' : winner + ' wins!' }}
    </div>
    <button class="button" @click="resetGame">Reset Game</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      board: Array(9).fill(null),
      currentPlayer: 'X',
      winner: null,
    };
  },
  methods: {
    onClick(index) {
      if (this.board[index] || this.winner) return;

      this.$set(this.board, index, this.currentPlayer);
      if (this.checkWinner()) {
        this.winner = this.currentPlayer;
      } else if (this.board.every(cell => cell)) {
        this.winner = 'Tie';
      } else {
        this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X';
      }
    },
    checkWinner() {
      const winPatterns = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6],
      ];

      return winPatterns.some(pattern => {
        const [a, b, c] = pattern;
        return this.board[a] && this.board[a] === this.board[b] && this.board[a] === this.board[c];
      });
    },
    resetGame() {
      this.board = Array(9).fill(null);
      this.currentPlayer = 'X';
      this.winner = null;
    },
  },
};
</script>

<style>
#app {
  text-align: center;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: #2c3e50;
  margin-top: 60px;
}

.board {
  display: grid;
  grid-template-columns: repeat(3, 90px);
  gap: 6px;
  justify-content: center;
}

.cell {
  width: 90px;
  height: 90px;
  display: flex;
  background-color: rgba(148, 134, 134, 0.2);

  align-items: center;
  justify-content: center;
  font-size: 24px;
  border: 2px solid #000;
  cursor: pointer;
  transition: all 0.3s ease;
}

.cell:hover {
    background-color: rgba(255, 255, 255, 0.2);
    transform: scale(1.05);
  }
.cell-x {
  color: rgb(0, 36, 94);
}

.cell-o {
  color: rgb(27, 190, 79);
}

.cell-win {
  position: relative;
  font-weight: bold;
}

.status {
  margin-top: 20px;
  font-size: 20px;
}

.button {
  --b: 3px;  
  --s: .45em; 
  --color: #373B44;
  margin-top: 20px;
  padding: calc(.8em + var(--s)) calc(.9em + var(--s));
  color: var(--color);
  --_p: var(--s);
  background:
    conic-gradient(from 90deg at var(--b) var(--b),#0000 90deg,var(--color) 0)
    var(--_p) var(--_p)/calc(100% - var(--b) - 2*var(--_p)) calc(100% - var(--b) - 2*var(--_p));
  transition: .3s linear, color 0s, background-color 0s;
  outline: var(--b) solid #0000;
  outline-offset: .6em;
  font-size: 16px;

  border: 0;

  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
}

.button:hover,
.button:focus-visible{
  --_p: 0px;
  outline-color: var(--color);
  outline-offset: .05em;
}

.button:active {
  background: var(--color);
  color: #ffffff;
}
</style>
