<template></template>

<script>
import { reactive, ref, h, defineComponent } from "vue";

export default defineComponent({
  setup() {
    const board = reactive(Array(9).fill(null));    //for board array
    const currentPlayer = ref('X');                 //track if player X or O
    const winner = ref(null);                       //track winner

    const makeMove = (index) => {                   //when click on cell
      if (!board[index] && !winner.value) {
        board[index] = currentPlayer.value;         //X or O
        if (checkWinner()) {                        //Check if winner after a move
          winner.value = currentPlayer.value;
        } else {
          currentPlayer.value = currentPlayer.value === 'X' ? 'O' : 'X';        //else next turn
        }
      }
    };

    const checkWinner = () => {           
      const winningCombinations = [
        [0, 1, 2], [3, 4, 5], [6, 7, 8],    //row
        [0, 3, 6],[1, 4, 7],[2, 5, 8],      //column
        [0, 4, 8],[2, 4, 6],                //diagonal
      ];
      return winningCombinations.some(([a, b, c]) => {
        return board[a] && board[a] === board[b] && board[a] === board[c];
      });
    };

    const resetGame = () => {           //clears board
      board.fill(null);
      currentPlayer.value = 'X';
      winner.value = null;
    };

    return () =>            //board triggered by click
      h('div', { id: 'app' }, [
        h('h1', "Tic Tac Toe"),
        h('div', { class: 'board', style: { display: 'grid', 'grid-template-columns': 'repeat(3, 100px)', 'grid-gap': '10px' } },
          board.map((value, index) => h('div', {
            class: 'cell',
            style: { width: '100px', height: '100px', backgroundColor: '#f0f0f0', display: 'flex', justifyContent: 'center', alignItems: 'center', fontSize: '2em', cursor: 'pointer' },
            onClick: () => makeMove(index)
          }, value))
        ),
        winner.value ? h('h2', `${winner.value} wins!`) : null,   //display winner
        h('button', { onClick: resetGame, style: { marginTop: '20px', padding: '10px 20px', fontSize: '1em' } }, 'Reset Game')  //reset button appears
      ]);
  }
});
</script>

<style scoped>
#app {
  text-align: center;
  margin-top: 50px;
}
.board {
  display: grid;
  grid-template-columns: repeat(3, 100px);
  grid-gap: 10px;
  justify-content: center;
  color: red;
}
.cell {
  width: 100px;
  height: 100px;
  background-color: #f0f0f0;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 2em;
  cursor: pointer;

}
button {
  margin-top: 20px;
  padding: 10px 20px;
  font-size: 1em;
  color: green;
}
</style>
