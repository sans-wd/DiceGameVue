<script setup>
import { ref } from "vue";
const player1Score = ref(null);
const player1GameScore = ref(null);
const player2Score = ref(null);
const player2GameScore = ref(null);
const player1Active = ref(null);
const player2Active = ref(null);
const player1Win = ref(null);
const player2Win = ref(null);
const isGameStart = ref(null);
const randomNum = ref(null);
const diceIcons = [
  '<i class="fa-solid fa-dice-one"></i>',
  '<i class="fa-solid fa-dice-two"></i>',
  '<i class="fa-solid fa-dice-three"></i>',
  '<i class="fa-solid fa-dice-four"></i>',
  '<i class="fa-solid fa-dice-five"></i>',
  '<i class="fa-solid fa-dice-six"></i>',
];
const diceSelector = ref("");

const gameStart = () => {
  player1GameScore.value = 0;
  player2GameScore.value = 0;
  player1Score.value = 0;
  player2Score.value = 0;
  player1Active.value = true;
  player2Active.value = false;
  isGameStart.value = false;
  player1Win.value = false;
  player2Win.value = false;
};
gameStart();

const rollDice = () => {
  if (!player1Win.value && !player2Win.value) {
    randomNum.value = Math.floor(Math.random() * 6);
    diceSelector.value = diceIcons[randomNum.value];
    console.log(diceSelector.value);
    isGameStart.value = true;
    if (randomNum.value == 0) {
      player1Active.value = !player1Active.value;
      player1GameScore.value = 0;
      player2Active.value = !player2Active.value;
      player2GameScore.value = 0;
    }

    if (player1Active.value == true && randomNum.value != 0) {
      player1GameScore.value += randomNum.value + 1;
    }
    if (player2Active.value == true && randomNum.value != 0) {
      player2GameScore.value += randomNum.value + 1;
    }
  }
};

const hold = () => {
  if (!player1Win.value && !player2Win.value) {
    if (player1Active.value) {
      player1Score.value += player1GameScore.value;
      player1GameScore.value = 0;
    }
    if (player2Active.value) {
      player2Score.value += player2GameScore.value;
      player2GameScore.value = 0;
    }

    if (player1Score.value >= 100) {
      player1Win.value = true;
      console.log(player1Win.value);
    }
    if (player2Score.value >= 100) {
      player2Win.value = true;
    }
    player1Active.value = !player1Active.value;

    player2Active.value = !player2Active.value;
  }
};
</script>

<template>
  <main
    class="bg-gradient-to-t from-fuchsia-400 via-cyan-900 to-violet-600 h-screen w-screen flex justify-center items-center"
  >
    <div class="apWrapper relative flex justify-between">
      <div
        class="gameButtons p-14 absolute -translate-x-1/2 left-1/2 flex flex-col justify-between text-center"
      >
        <div class="top flex flex-col">
          <button
            @click="gameStart()"
            type="button"
            class="text-gray-900 bg-gradient-to-r from-red-200 via-red-300 to-yellow-200 hover:bg-gradient-to-bl focus:ring-4 focus:outline-none focus:ring-red-100 dark:focus:ring-red-400 font-medium rounded-lg text-sm px-5 py-2.5 text-center mr-2 mb-2"
          >
            New Game
          </button>
        </div>
        <p
          v-html="diceSelector"
          :class="{ hidden: !isGameStart }"
          class="diceNum text-8xl"
        ></p>
        <div class="bottom flex flex-col">
          <button
            @click="rollDice"
            type="button"
            class="text-gray-900 bg-gradient-to-r from-red-200 via-red-300 to-yellow-200 hover:bg-gradient-to-bl focus:ring-4 focus:outline-none focus:ring-red-100 dark:focus:ring-red-400 font-medium rounded-lg text-sm px-5 py-2.5 text-center mr-2 mb-2"
          >
            Roll Dice
          </button>
          <button
            @click="hold()"
            type="button"
            class="text-gray-900 bg-gradient-to-r from-red-200 via-red-300 to-yellow-200 hover:bg-gradient-to-bl focus:ring-4 focus:outline-none focus:ring-red-100 dark:focus:ring-red-400 font-medium rounded-lg text-sm px-5 py-2.5 text-center mr-2 mb-2"
          >
            Hold
          </button>
        </div>
      </div>
      <div class="player1Wrapper w-1/2">
        <div
          :class="{
            'bg-opacity-70': !player1Active,
            'bg-green-600': player1Win,
            'bg-white': !player1Win,
          }"
          class="player1Body transition-all rounded-tl-2xl rounded-bl-2xl flex flex-col justify-between items-center p-11"
        >
          <div class="text flex flex-col justify-center items-center">
            <h1 class="text-pink-500 text-4xl">PLAYER 1</h1>
            <h1 class="text-pink-500 text-7xl mt-4">{{ player1Score }}</h1>
          </div>
          <div
            class="score bg-pink-500 text-white text-2xl rounded-xl p-4 flex flex-col justify-center items-center w-52"
          >
            <h1 class="">Current</h1>
            <h2 class="mt-7">{{ player1GameScore }}</h2>
          </div>
        </div>
      </div>
      <div class="player2Wrapper w-1/2">
        <div
          class="player2Body transition-all rounded-tr-2xl rounded-br-2xl flex flex-col justify-between items-center p-11"
          :class="{
            'bg-opacity-70': !player2Active,
            'bg-green-600': player2Win,
            'bg-white': !player2Win,
          }"
        >
          <div class="text flex flex-col justify-center items-center">
            <h1 class="text-pink-500 text-4xl">PLAYER 2</h1>
            <h1 class="text-pink-500 text-7xl mt-4">{{ player2Score }}</h1>
          </div>
          <div
            class="score bg-pink-500 text-white text-2xl rounded-xl p-4 flex flex-col justify-center items-center w-52"
          >
            <h1 class="">Current</h1>
            <h2 class="mt-7">{{ player2GameScore }}</h2>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
.player1Body,
.player2Body {
  width: 453px;
  height: 610px;
}
.gameButtons {
  height: 610px;
}
</style>
