<template>
  <div>
    <div class="text-center">
      <h1>Tic Tac Toe</h1>
      <h3 id="turnText">Turn Of :{{this.game.currentTurn}}</h3>
      <div class="score" v-if="game.players">
        <span>
          <u>Scores</u>
        </span>
        <br />
        <span id="p1_score">Player One :{{this.game.players[0].score}}</span>
        <span id="p2_score">Player Two :{{this.game.players[1].score}}</span>
        <span id="draw_score">Draw :{{this.game.draw_score}}</span>
      </div>
      <button class="replay" id="restartBtn" @tap="resetGame()">Restart</button>
    </div>

    <div class="gamebox" style="margin-top:20px">
      <button id="00" @click="onClick($event)">{{btnText["00"]}}</button>
      <button id="01" @click="onClick($event)">{{btnText["01"]}}</button>
      <button id="02" @click="onClick($event)">{{btnText["02"]}}</button>
      <button id="10" @click="onClick($event)">{{btnText["10"]}}</button>
      <button id="11" @click="onClick($event)">{{btnText["11"]}}</button>
      <button id="12" @click="onClick($event)">{{btnText["12"]}}</button>
      <button id="20" @click="onClick($event)">{{btnText["20"]}}</button>
      <button id="21" @click="onClick($event)">{{btnText["21"]}}</button>
      <button id="22" @click="onClick($event)">{{btnText["22"]}}</button>
    </div>
  </div>
</template>

<script>
import Player from "../Player";
import Game from "../Game";

const game = new Game();
game.players.push(new Player("X"));
game.players.push(new Player("O"));

export default {
  name: "HelloWorld",
  data() {
    return {
      game: game,
      buttons: [],
      btnText: []
    };
  },
  methods: {
    onClick($event) {
      let btn = $event.target;
      //check if filled already
      if (this.btnText[btn.id.toString()].length > 0) {
        alert("Already filled");
        return;
      }
      //fill X/O
      this.btnText[btn.id] = this.game.currentTurn;
      this.game.changeTurn();
      //check if game won
      setTimeout(() => {
        if (this.game.checkWinner(btn) == true) {
          this.resetGame();
        }
      }, 100);
    },
    resetGame() {
      //UI and backend reset
      this.game.reset();
      [...this.buttons].forEach(btn => {
        this.btnText[btn.id] = "";
      });
    }
  },
  mounted() {
    //load all buttons and reset initially
    this.$nextTick(() => {
      this.buttons = this.$el.querySelectorAll(".gamebox button");
      this.resetGame();
    });
  }
};
</script>

<style scoped>
.gamebox button {
  width: 100px;
  height: 100px;
  border: 1px solid #000;
  display: inline-block;
  font-size: 30px;
  color: #fff;
}
.gamebox {
  margin: 0 auto;
  max-width: 300px;
  display: grid;
  justify-items: center;
  align-content: space-around;
  grid-template-columns: repeat(3, 1fr);
}
.replay {
  border: 1px solid #ddd;
  height: 60px;
  margin-top: 20px;
  padding: 10px;
}
</style>
