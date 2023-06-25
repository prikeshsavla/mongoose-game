<script setup>
import { ref } from 'vue'
import shuffle from 'lodash/shuffle'
import Card from './Card.vue'
function getAllCards() {
  return [
    { card: "A♦", rank: 1, face: "diams" },
    { card: "2♦", rank: 2, face: "diams" },
    { card: "3♦", rank: 3, face: "diams" },
    { card: "4♦", rank: 4, face: "diams" },
    { card: "5♦", rank: 5, face: "diams" },
    { card: "6♦", rank: 6, face: "diams" },
    { card: "7♦", rank: 7, face: "diams" },
    { card: "8♦", rank: 8, face: "diams" },
    { card: "9♦", rank: 9, face: "diams" },
    { card: "10♦", rank: 10, face: "diams" },
    { card: "J♦", rank: 11, face: "diams" },
    { card: "Q♦", rank: 12, face: "diams" },
    { card: "K♦", rank: 13, face: "diams" },
    { card: "A♠", rank: 1, face: "spades" },
    { card: "2♠", rank: 2, face: "spades" },
    { card: "3♠", rank: 3, face: "spades" },
    { card: "4♠", rank: 4, face: "spades" },
    { card: "5♠", rank: 5, face: "spades" },
    { card: "6♠", rank: 6, face: "spades" },
    { card: "7♠", rank: 7, face: "spades" },
    { card: "8♠", rank: 8, face: "spades" },
    { card: "9♠", rank: 9, face: "spades" },
    { card: "10♠", rank: 10, face: "spades" },
    { card: "J♠", rank: 11, face: "spades" },
    { card: "Q♠", rank: 12, face: "spades" },
    { card: "K♠", rank: 13, face: "spades" },
    { card: "A♥", rank: 1, face: "hearts" },
    { card: "2♥", rank: 2, face: "hearts" },
    { card: "3♥", rank: 3, face: "hearts" },
    { card: "4♥", rank: 4, face: "hearts" },
    { card: "5♥", rank: 5, face: "hearts" },
    { card: "6♥", rank: 6, face: "hearts" },
    { card: "7♥", rank: 7, face: "hearts" },
    { card: "8♥", rank: 8, face: "hearts" },
    { card: "9♥", rank: 9, face: "hearts" },
    { card: "10♥", rank: 10, face: "hearts" },
    { card: "J♥", rank: 11, face: "hearts" },
    { card: "Q♥", rank: 12, face: "hearts" },
    { card: "K♥", rank: 13, face: "hearts" },
    { card: "A♣", rank: 1, face: "clubs" },
    { card: "2♣", rank: 2, face: "clubs" },
    { card: "3♣", rank: 3, face: "clubs" },
    { card: "4♣", rank: 4, face: "clubs" },
    { card: "5♣", rank: 5, face: "clubs" },
    { card: "6♣", rank: 6, face: "clubs" },
    { card: "7♣", rank: 7, face: "clubs" },
    { card: "8♣", rank: 8, face: "clubs" },
    { card: "9♣", rank: 9, face: "clubs" },
    { card: "10♣", rank: 10, face: "clubs" },
    { card: "J♣", rank: 11, face: "clubs" },
    { card: "Q♣", rank: 12, face: "clubs" },
    { card: "K♣", rank: 13, face: "clubs" },
  ]
}

// const count = ref(0)
const cards = getAllCards();
const activePlayer = ref(0)
const playerTemplate = { index: 0, openCards: [], cardInHand: null, cards: [] };
var players = ref([]);
var boards = ref([
  { face: "♦ diamonds", openCards: [] },
  { face: "♠ spades", openCards: [] },
  { face: "♥ hearts", openCards: [] },
  { face: "♣ clubs", openCards: [] }
]);
// shuffle cards
var shuffled_cards = shuffle(cards);
// get number of players
const numberOfPlayers = 2;
for (var i = 0; i < numberOfPlayers; i++) {
  players.value.push({ ...playerTemplate, index: i });
}
// give equal number of cards one at a time
shuffled_cards.forEach((element, index) => {
  players.value[index % numberOfPlayers]["cards"] = [element, ...players.value[index % numberOfPlayers]["cards"]];
});
console.log(players);
// const incCount = () => {
//   count.value = count.value + 1;
// }

const pickACard = (playerIndex) => {
  players.value[playerIndex].cardInHand = players.value[playerIndex].cards.pop();
  // players[playerIndex].openCards = [players[playerIndex].cards.pop, ...players[playerIndex].openCards]
}

const showPlayerCard = (playerIndex, defaultValue = "No Card") => {
  return players.value[playerIndex].cardInHand;
}

const placeACardOnPlayer = (playerIndex) => {
  players.value[playerIndex].openCards = [players.value[activePlayer.value].cardInHand, ...players.value[playerIndex].openCards]
  players.value[activePlayer.value].cardInHand = null;
}

const placeACardOnBoard = (boardIndex) => {
  boards.value[boardIndex].openCards = [players.value[activePlayer.value].cardInHand, ...boards.value[boardIndex].openCards]
  players.value[activePlayer.value].cardInHand = null;
}

const showLastOpenCard = (playerIndex) => {
  return players.value[playerIndex].openCards[0];
}

const showBoardsLastOpenCard = (boardIndex) => {
  return boards.value[boardIndex].openCards[0];
}

const cardCount = (playerIndex) => {
  return players.value[playerIndex].cards.length;
}
const cardPileCount = (playerIndex) => {
  return players.value[playerIndex].openCards.length;
}

</script>

<template>
  <h1>Mongoose</h1>
  <!-- <div class="card">
    <button type="button" @click="incCount">count is {{ count }}</button>
  </div> -->
  <div class="playingCards fourColours faceImages  rotateHand">
    <div class="card rank-7 spades">
      <span class="rank">7</span>
      <span class="suit">&spades;</span>
    </div>

    <!-- playingCards fourColours rotateHand -->
    <table>
      <tr>
        <th>♦ diamonds</th>
        <th>♠ spades</th>
        <th>♥ hearts</th>
        <th>♣ clubs</th>
      </tr>
      <tr>
        <td>
          <Card :card="showBoardsLastOpenCard(0)" noCardText="No Card Picked" />
        </td>
        <td>
          <Card :card="showBoardsLastOpenCard(1)" noCardText="No Card Picked" />
        </td>
        <td>
          <Card :card="showBoardsLastOpenCard(2)" noCardText="No Card Picked" />
        </td>
        <td>
          <Card :card="showBoardsLastOpenCard(3)" noCardText="No Card Picked" />
        </td>
      </tr>
      <tr>
        <td><button @click="placeACardOnBoard(0)">place card here</button></td>
        <td><button @click="placeACardOnBoard(1)">place card here</button></td>
        <td><button @click="placeACardOnBoard(2)">place card here</button></td>
        <td><button @click="placeACardOnBoard(3)">place card here</button></td>
      </tr>
    </table>

    <h2> Players</h2>

    <table width="100%">
      <tr>
        <th>Player 1 (Cards: {{ cardCount(0) }}) </th>
        <th>Player 2 (Cards: {{ cardCount(1) }})</th>
      </tr>
      <tr>
        <td>
          <b>Card Pile (Cards: {{ cardPileCount(0) }})</b>
        </td>
        <td>
          <b>Card Pile (Cards: {{ cardPileCount(1) }})</b>
        </td>
      </tr>
      <tr>
        <td>
          <Card :card="showLastOpenCard(0)" noCardText="No Card Picked" />
        </td>
        <td>
          <Card :card="showLastOpenCard(1)" noCardText="No Card Picked" />
        </td>
      </tr>
      <tr>
        <td>
          <button data-target="clubs" @click="placeACardOnPlayer(0)">place card here</button>
        </td>
        <td>
          <button data-target="clubs" @click="placeACardOnPlayer(1)">place card here</button>
        </td>
      </tr>
      <tr>
        <td>
          <b>Card in Hand</b>
        </td>
        <td>
          <b>Card in Hand</b>
        </td>
      </tr>
      <tr>
        <td>
          <Card :card="showPlayerCard(0)" noCardText="No Card Picked" />
          <!-- <div class="card back">{{ showPlayerCard(0, "No Card Picked") }}</div> -->
        </td>
        <td>
          <Card :card="showPlayerCard(1)" noCardText="No Card Picked" />
        </td>
      </tr>
      <tr>
        <td>
          <button data-target="clubs" @click="pickACard(0)">Pick a card</button> <br /><br />
        </td>
        <td>
          <button data-target="clubs" @click="pickACard(1)">Pick a card</button> <br /><br />
        </td>
      </tr>
    </table>
  </div>
</template>

<style scoped></style>
