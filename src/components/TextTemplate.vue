<template>
    <p>
        <span class="mistake">{{ mistake }}</span>
        <span>{{ remainingSlice }}</span>
    </p>
</template>

<script setup lang="ts">
import { computed } from "vue";

const props = defineProps({
    initialSlice: {
        type: String, 
        required: true,
    },
    hasMistake: {
        type: Boolean,
        default: false,
    },
})

const mistake = computed(() => props.hasMistake ? props.initialSlice[0] : "");
const remainingSlice = computed(() => props.hasMistake ? props.initialSlice.slice(1) : props.initialSlice);
</script>

<style scoped>
p {
    padding-top: 40px;
    font-size: 24px;
}
.mistake {
    color: red;
}

@media (width < 768px) {
    p {
        font-size: 16px;
    }
}
</style>