<template>
    <h3>Time:</h3>
    <span>{{ formatTime }}</span>
</template>

<script setup lang="ts">
import { ref, watch, computed, onMounted, onUnmounted } from "vue";

const props = defineProps({
    gameIsOn: {
        type: Boolean,
        required: true,
    }
})
const emit = defineEmits(["stopGame"])

const time = ref<number>(60);
const intervalId = ref<number | null>(null);

const formatTime = computed(() => {
    const minutes = Math.floor(time.value / 60) + "";
    const seconds = time.value % 60 + ""
    return `${minutes.padStart(2, "0")}:${seconds.padStart(2, "0")}`
})

const startTimer = () => {
    if (!intervalId.value) {
        intervalId.value = setInterval(() => {
            time.value--;
        }, 1000)
    }
}

const stopTimer = () => {
    if (intervalId.value) {
        clearInterval(intervalId.value);
        intervalId.value = null;
    }
    emit("stopGame");
}

watch(time, () => {
    if (time.value <= 0) {
        stopTimer();
    }
})

onMounted(() => {
    if (props.gameIsOn) {
        startTimer();
    }
})
onUnmounted(() => {
    stopTimer();
})
</script>