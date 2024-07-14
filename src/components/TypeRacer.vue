<template>
    <h1>TypeRacer</h1>
    <BaseButton v-if="!gameIsOn" @click="handleStartGame">
        Start New Game
    </BaseButton>
    <template v-if="gameIsOn">
        <Timer 
            :gameIsOn="gameIsOn" 
            @stopGame="handleStopGame" 
            :time="time" 
            @decrementTimer="decrementTimer"
            :updateTimerBy="updateTimerBy" />
        <TextTemplate :matchingSlice="matchingSlice" :initialSlice="initialSlice" />
        <TextInput v-model="typedText" :hasMistake="hasMistake" />
    </template>
    <template v-if="showResults">
        <p v-if="matchingSlice.length === text.length">Congrats!</p>
        <p>Your score is {{ matchingSlice.length }} out of {{ text.length }}</p>
    </template>
</template>

<script setup lang="ts">
import { ref, watch } from "vue";
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
const time = ref<number>(10);
const correctTimes = ref<number>(0);
const updateTimerBy = ref<string>("");

const handleStartGame = () => {
    const index = Math.floor(Math.random() * texts.length);
    text.value = texts[index];
    initialSlice.value = text.value;
    typedText.value = "";
    matchingSlice.value = "";
    hasMistake.value = false;
    time.value = 10;
    updateTimerBy.value = "";
    showResults.value = false;
    gameIsOn.value = true;
}

const handleStopGame = () => {
    showResults.value = true;
    gameIsOn.value = false;
}

const decrementTimer = () => {
    time.value--
}
const incrementTimer = () => {
    time.value++
}

const handleMatch = () => {
    matchingSlice.value = typedText.value;
    initialSlice.value = text.value.slice(typedText.value.length);
    hasMistake.value = false;
    correctTimes.value++;
    if (correctTimes.value >= 3) {
        incrementTimer();
        updateTimerBy.value = "+1";
        setTimeout(() => {
            updateTimerBy.value = "";
        }, 1000);
        correctTimes.value = 0;
    }
    if (matchingSlice.value.length === text.value.length) {
        handleStopGame();
    }
}

const handleMistake = () => {
    hasMistake.value = true;
    decrementTimer();
    updateTimerBy.value = "-1";
    setTimeout(() => {
        updateTimerBy.value = "";
    }, 1000);
}

watch(typedText, () => {
    if (typedText.value === text.value.slice(0, typedText.value.length)) {
        handleMatch();
    } else {
        handleMistake();
    }   
})
</script>