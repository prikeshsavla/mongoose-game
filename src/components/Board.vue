<script setup>
import { ref } from 'vue'
import shuffle from 'lodash/shuffle'
import Card from './Card.vue'
function getAllCards() {
  return [
    { card: "A♦", rank: 1, suit: "diams" },
    { card: "2♦", rank: 2, suit: "diams" },
    { card: "3♦", rank: 3, suit: "diams" },
    // { card: "4♦", rank: 4, suit: "diams" },
    // { card: "5♦", rank: 5, suit: "diams" },
    // { card: "6♦", rank: 6, suit: "diams" },
    // { card: "7♦", rank: 7, suit: "diams" },
    // { card: "8♦", rank: 8, suit: "diams" },
    // { card: "9♦", rank: 9, suit: "diams" },
    // { card: "10♦", rank: 10, suit: "diams" },
    // { card: "J♦", rank: 11, suit: "diams" },
    // { card: "Q♦", rank: 12, suit: "diams" },
    // { card: "K♦", rank: 13, suit: "diams" },
    { card: "A♠", rank: 1, suit: "spades" },
    { card: "2♠", rank: 2, suit: "spades" },
    { card: "3♠", rank: 3, suit: "spades" },
    // { card: "4♠", rank: 4, suit: "spades" },
    // { card: "5♠", rank: 5, suit: "spades" },
    // { card: "6♠", rank: 6, suit: "spades" },
    // { card: "7♠", rank: 7, suit: "spades" },
    // { card: "8♠", rank: 8, suit: "spades" },
    // { card: "9♠", rank: 9, suit: "spades" },
    // { card: "10♠", rank: 10, suit: "spades" },
    // { card: "J♠", rank: 11, suit: "spades" },
    // { card: "Q♠", rank: 12, suit: "spades" },
    // { card: "K♠", rank: 13, suit: "spades" },
    { card: "A♥", rank: 1, suit: "hearts" },
    { card: "2♥", rank: 2, suit: "hearts" },
    { card: "3♥", rank: 3, suit: "hearts" },
    // { card: "4♥", rank: 4, suit: "hearts" },
    // { card: "5♥", rank: 5, suit: "hearts" },
    // { card: "6♥", rank: 6, suit: "hearts" },
    // { card: "7♥", rank: 7, suit: "hearts" },
    // { card: "8♥", rank: 8, suit: "hearts" },
    // { card: "9♥", rank: 9, suit: "hearts" },
    // { card: "10♥", rank: 10, suit: "hearts" },
    // { card: "J♥", rank: 11, suit: "hearts" },
    // { card: "Q♥", rank: 12, suit: "hearts" },
    // { card: "K♥", rank: 13, suit: "hearts" },
    { card: "A♣", rank: 1, suit: "clubs" },
    { card: "2♣", rank: 2, suit: "clubs" },
    { card: "3♣", rank: 3, suit: "clubs" },
    // { card: "4♣", rank: 4, suit: "clubs" },
    // { card: "5♣", rank: 5, suit: "clubs" },
    // { card: "6♣", rank: 6, suit: "clubs" },
    // { card: "7♣", rank: 7, suit: "clubs" },
    // { card: "8♣", rank: 8, suit: "clubs" },
    // { card: "9♣", rank: 9, suit: "clubs" },
    // { card: "10♣", rank: 10, suit: "clubs" },
    // { card: "J♣", rank: 11, suit: "clubs" },
    // { card: "Q♣", rank: 12, suit: "clubs" },
    // { card: "K♣", rank: 13, suit: "clubs" },
  ]
}

// const count = ref(0)
const cards = getAllCards();
const activePlayer = ref(0)
const playerTemplate = { index: 0, openCards: [], cardInHand: null, cards: [] };
var players = ref([]);
var boards = ref([
  { title: "♦ diamonds", openCards: [], suit: "diams" },
  { title: "♠ spades", openCards: [], suit: "spades" },
  { title: "♥ hearts", openCards: [], suit: "hearts" },
  { title: "♣ clubs", openCards: [], suit: "clubs" }
]);
// shuffle cards
var shuffled_cards =  shuffle(cards);
// get number of players
const numberOfPlayers = 1;
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

const pickAPlacedCard = (playerIndex) => {
  const lastPos = players.value[playerIndex].openCards.length - 1;
  players.value[playerIndex].cardInHand = players.value[playerIndex].openCards[lastPos];
  players.value[playerIndex].openCards.splice(lastPos, 1)
  // players[playerIndex].openCards = [players[playerIndex].cards.pop, ...players[playerIndex].openCards]
}



const showPlayerCard = (playerIndex, defaultValue = "No Card") => {
  return players.value[playerIndex].cardInHand;
}

const placeACardOnPlayer = (playerIndex) => {
  players.value[playerIndex].openCards = [players.value[activePlayer.value].cardInHand, ...players.value[playerIndex].openCards]
  players.value[activePlayer.value].cardInHand = null;
}

const resetOpenCards = (playerIndex) => {
  players.value[playerIndex].cards = [...players.value[playerIndex].openCards];
  players.value[playerIndex].openCards = [];
}

const placeACardOnBoard = (boardIndex) => {
  const baseCard = boards.value[boardIndex].openCards[0]
  const cardInHand = players.value[activePlayer.value].cardInHand;
  console.log(cardInHand);
  console.log(boards.value[boardIndex].suit);
  if (boards.value[boardIndex].suit == cardInHand.suit && (cardInHand.rank == 1 || (baseCard && baseCard.rank + 1 == cardInHand.rank))) {
    boards.value[boardIndex].openCards = [cardInHand, ...boards.value[boardIndex].openCards]
    players.value[activePlayer.value].cardInHand = null;
  }
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
    <table>
      <tr>
        <td v-for="i in numberOfPlayers" v-key="i">
          <table width="100%">
            <tr>
              <th>Player 1 (Cards: {{ cardCount(i - 1) }}) </th>
            </tr>
            <tr>
              <td>
                <b>Card Pile (Cards: {{ cardPileCount(i - 1) }})</b>
              </td>
            </tr>
            <tr>
              <td>
                <Card :card="showLastOpenCard(i - 1)" noCardText="No Card Picked" />
              </td>
            </tr>
            <tr>
              <td>
                <button :disabled="players[i - 1].cardInHand != null ||  players[i - 1].openCards.length == 0 " @click="pickAPlacedCard(i - 1)">
                  Pick Placed</button> <br /><br />
                <button :disabled="players[i - 1].cardInHand == null" @click="placeACardOnPlayer(i - 1)">Place</button>
                <button :disabled="players[i - 1].cards.length != 0" @click="resetOpenCards(i - 1)">
                  Reset Cards</button> <br /><br />
              </td>
            </tr>
            <tr>
              <td>
                <b>Card in Hand</b>
              </td>
            </tr>
            <tr>
              <td>
                <Card :card="showPlayerCard(i - 1)" noCardText="No Card Picked" />
              </td>
            </tr>
            <tr>
              <td>
                <button :disabled="players[i - 1].cardInHand" @click="pickACard(i - 1)">Pick</button> <br /><br />
              </td>
            </tr>
          </table>
        </td>
      </tr>
    </table>

  </div>
</template>

<style scoped></style>
