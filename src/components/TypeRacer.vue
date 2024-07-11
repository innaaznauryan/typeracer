<template>
    <h1>TypeRacer</h1>
    <BaseButton v-if="!gameIsOn" @click="handleStartGame">
        Start New Game
    </BaseButton>
    <template v-if="gameIsOn">
        <Timer :gameIsOn="gameIsOn" @stopGame="handleStopGame" />
        <TextTemplate :matchingSlice="matchingSlice" :initialSlice="initialSlice" />
        <TextInput v-model="typedText" :hasMistake="hasMistake" />
    </template>
    <template v-if="showResults">
        <p v-if="matchingSlice.length === text.length">Congrats!</p>
        <p>Your score is {{ matchingSlice.length }} out of {{ text.length }}</p>
    </template>
</template>

<script setup lang="ts">
import { ref, watch, onMounted } from "vue";
import BaseButton from "./BaseButton.vue";
import Timer from "./Timer.vue";
import TextTemplate from "./TextTemplate.vue";
import TextInput from "./TextInput.vue";
import texts from "../constants/texts.json";

const text = ref<string>("");
const typedText = ref<string>("");
const matchingSlice = ref<string>("");
const initialSlice = ref<string>("");
const hasMistake = ref<boolean>(false);
const gameIsOn = ref<boolean>(false);
const showResults = ref<boolean>(false);

const handleStartGame = () => {
    gameIsOn.value = true;
    typedText.value = "";
    matchingSlice.value = "";
    initialSlice.value = text.value;
    hasMistake.value = false;
    showResults.value = false;
    resetText();
}

const handleStopGame = () => {
    showResults.value = true;
    gameIsOn.value = false;
}

const resetText = () => {
    const index = Math.floor(Math.random() * texts.length);
    text.value = texts[index];
    initialSlice.value = text.value;
}

watch(typedText, () => {
    if (typedText.value === text.value.slice(0, typedText.value.length)) { 
        matchingSlice.value = typedText.value;
        initialSlice.value = text.value.slice(typedText.value.length);
        
        hasMistake.value = false;
    } else {
        hasMistake.value = true;
    }   
})

onMounted(() => {
    resetText();
})
</script>