<template>
    <h3>
        <span>Time:</span>
        <transition name="fade" mode="out-in">
            <span :key="updateTimerBy" class="counter">{{ updateTimerBy }}</span>
        </transition>
    </h3>
     <transition name="fade" mode="out-in">
        <div :key="formatTime">{{ formatTime }}</div>
    </transition>
</template>

<script setup lang="ts">
import { ref, watch, computed, onMounted, onUnmounted } from "vue";

const props = defineProps({
    gameIsOn: {
        type: Boolean,
        required: true,
    },
    time: {
        type: Number,
        required: true,
    },
    updateTimerBy: {
        type: String,
        required: true,
    }
})
const emit = defineEmits(["stopGame", "decrementTimer"])

const intervalId = ref<number | null>(null);
const timeValue = computed(() => props.time);

const formatTime = computed(() => {
    const minutes = Math.floor(props.time / 60) + "";
    const seconds = props.time % 60 + ""
    return `${minutes.padStart(2, "0")}:${seconds.padStart(2, "0")}`
})

const startTimer = () => {
    if (!intervalId.value) {
        intervalId.value = setInterval(() => {
            emit("decrementTimer")
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

watch(timeValue, () => {
    if (timeValue.value <= 0) {
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

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

h3 {
    position: relative;
    display: inline-block;
}

.counter {
    position: absolute;
    right: -100%;
}
</style>