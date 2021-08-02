<template>
<div class="board">
<form class="board">
  <div v-for="i in 9" :key="i">
    <button 
        v-on:click.prevent="selectSquare(i-1)" 
        :style="cursorStyles(currentPlayer)" >
      <img height="40" width="40" :src="tokenImg(i-1)"/>
    </button>
  </div>
  <button @click="restart">New Game</button>
 </form>
   <div v-if="isWinner('x') && !isWinner('o')"><h2>x wins</h2></div>
   <div v-if="isWinner('o') && !isWinner('x')"><h2>o wins</h2></div>
 </div>
</template>

<script>
var gameDone = false;
var board = [0,0,0,0,0,0,0,0,0];
var currentPlayer = 1;
const path = ["img/0.png","img/x.png","img/o.png"];
const token = [0, "x", "o"];
const moveData = [ [0, [4, 8]],  [0, [1, 2]], 
                   [0, [3, 6]],  [1, [0, 2]], 
                   [1, [4, 7]],  [2, [0, 1]], 
                   [2, [4, 6]],  [2, [5, 8]],
                   [3, [0, 6]],  [3, [4, 5]], 
                   [4, [0, 8]],  [4, [1, 7]], 
                   [4, [2, 6]],  [4, [3, 5]], 
                   [5, [2, 8]],  [5, [3, 4]],
                   [6, [0, 3]],  [6, [7, 8]],
                   [6, [2, 4]],  [7, [6, 8]],
                   [7, [1, 4]],  [8, [2, 5]],
                   [8, [6, 7]],  [8, [0, 4]] ];
var gameMoves = [];
export default {
  name: "App",
  data() {
    return {
      gameDone,
      board,
      currentPlayer,
      path,
      token,
      moveData,
      gameMoves,
    };
  },
  methods: {
    tokenImg(i) {
      var pathi, x;
      x = token.indexOf(board[i]);
      pathi = path[x];
      return require(`./${pathi}`);
    },
    cursorStyles(i) {
      var pathi = path[i];
      return {
        cursor: "url("+require(`./${pathi}`)+"), grab",
        float: "left",
        width: "70px",
        height: "70px",
      }
    },
    restart() {
      board = [0,0,0,0,0,0,0,0,0];
      gameMoves = [];
      currentPlayer = 1;
      gameDone = false;
    },
    isWinner(player) {
      var flag;
      flag = (
          (board[0] == player &&
           board[1] == player &&
           board[2] == player) ||
          (board[3] == player &&
           board[4] == player &&
           board[5] == player) ||
          (board[6] == player &&
           board[7] == player &&
           board[8] == player) ||
          (board[0] == player &&
           board[3] == player &&
           board[6] == player) ||
          (board[1] == player &&
           board[4] == player &&
           board[7] == player) ||
          (board[2] == player &&
           board[5] == player &&
           board[8] == player) ||
          (board[0] == player &&
           board[4] == player &&
           board[8] == player) ||
          (board[2] == player &&
           board[4] == player &&
           board[6] == player)
      );
      if (flag) gameDone = true;
      return flag;
    },
    boardLogic(x, y, z){
      if ((this.board[y] === this.board[z]) && this.board[y] != 0)
        if (this.board[y] === 'o')
          return [moveData[x][0], 2];
        else 
          return [moveData[x][0], 1];
      else return [];
    },
    selectSquare(i) {
      var m = -1, movePlaces;
      var bestMove = [], betterMovePlaces = [];
      if (board[i] != 0 || gameDone) return;  

      board[i] = this.token[this.currentPlayer];
      gameMoves.push(i);
      this.currentPlayer = 3-this.currentPlayer;
      // logic for 'o'
      if (this.currentPlayer == 2 && !gameDone) {
        if (gameMoves.length<3 && this.board[4]=='x') {
          // x moves center.   o plays in corner
          movePlaces = [0, 2, 6, 8];
          m = movePlaces[Math.floor(Math.random()*movePlaces.length)];
        } else {
          //block all wins first but better is to move to win
          movePlaces = []; 
          for (var x=0; x<9; x++) 
            if (!board[x]) movePlaces.push(x); 
          bestMove = [];
          var n = [];
          for (x=0; x<moveData.length; x++) {
            n = this.boardLogic(x, moveData[x][1][0], moveData[x][1][1]);
            if (!n || !n.length || this.board[n[0]] != 0) continue;
            if (n[1]==this.currentPlayer) bestMove.push(n[0]);
            else betterMovePlaces.push(n[0]);
          }
        }
        if (bestMove.length) m = bestMove[Math.floor(Math.random() * bestMove.length)];
        else if (betterMovePlaces.length) m = betterMovePlaces[Math.floor(Math.random()*betterMovePlaces.length)];
        else if (movePlaces.length) m = movePlaces[Math.floor(Math.random() * movePlaces.length)];
        else m = movePlaces[Math.floor(Math.random()*movePlaces.length)];
        console.log("Possible Moves: "+movePlaces);
        console.log("Better Moves:"+betterMovePlaces);
        console.log("Best: "+bestMove);
        console.log("Chosen: "+m);
        if (!gameDone) this.selectSquare(m);
      } 
    },
  },
};
</script>

<style scoped>
.board { float: left; width: 220px;}
</style>