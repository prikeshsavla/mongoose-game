<script setup>
const props = defineProps(['card', 'noCardText']);
const faces = {1: "a", 11: "j", 12: "q", 13: "k"}
const rank = () => {
    console.log(props.card)
    return faces.hasOwnProperty(props.card.rank) ? faces[props.card.rank] : props.card.rank.toString()
}
const suit =  () => {
    return '&' + props.card.suit + ';'
}
</script>
<template>
    <!-- <div class="card back">{{ card ? card.card : noCardText }}</div> -->
    <template v-if="card">
    <div :class="'card rank-' + rank() +  ' ' + card.suit ">
        <span class="rank">{{ rank(card.rank).toUpperCase() }}</span>
        <span class="suit" v-html="suit()"></span>
    </div>
    </template>
    
    <div class="card back" v-else>{{ noCardText }}</div>
</template>