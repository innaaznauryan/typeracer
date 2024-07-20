<template>
    <h1>TypeRacer</h1>
    <BaseButton v-if="!gameIsOn" @click="handleStartGame">
        Start New Game
    </BaseButton>
    <template v-if="gameIsOn">
        <Timer
            @stopGame="handleStopGame" 
            @decrementTimer="decrementTimer"
            :gameIsOn="gameIsOn" 
            :time="time" 
            :updateTimerBy="updateTimerBy" />
        <TextTemplate :initialSlice="initialSlice" :hasMistake="hasMistake" />
    </template>
    <template v-if="showResults">
        <p v-if="!initialSlice.length">Congrats!</p>
        <p>Your score is {{ text.length - initialSlice.length }} out of {{ text.length }}</p>
    </template>
</template>

<script setup lang="ts">
import { ref } from "vue";
import BaseButton from "./BaseButton.vue";
import Timer from "./Timer.vue";
import TextTemplate from "./TextTemplate.vue";
import texts from "../constants/texts.json";

const text = ref<string>("");
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
    hasMistake.value = false;
    time.value = 10;
    updateTimerBy.value = "";
    showResults.value = false;
    gameIsOn.value = true;
    const input = document.createElement("input");
    input.style.opacity = "0";
    input.style.position = "absolute";
    input.style.pointerEvents = "none";
    document.body.appendChild(input);
    input.focus();
    input.addEventListener("input", handleInput);
}

const handleStopGame = () => {
    showResults.value = true;
    gameIsOn.value = false;
    const input = document.querySelector("input");
    if (input) {
        input.removeEventListener("input", handleInput);
        document.body.removeChild(input);
    }
}

const decrementTimer = () => {
    time.value--;
}
const incrementTimer = () => {
    time.value++;
}

const handleMatch = () => {
    initialSlice.value = initialSlice.value.slice(1);
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
    if (initialSlice.value.length <= 0) {
        handleStopGame();
    }
}

const handleMistake = () => {
    hasMistake.value = true;
    navigator.vibrate(200);
    decrementTimer();
    updateTimerBy.value = "-1";
    setTimeout(() => {
        updateTimerBy.value = "";
    }, 1000);
}

const handleInput = (e: Event) => {
    if (gameIsOn.value && e.target instanceof HTMLInputElement) {
        const value = e.target.value;      
        value[value.length - 1] === initialSlice.value[0] ? handleMatch() : handleMistake();
    }
}
</script>