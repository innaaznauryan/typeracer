<template>
    <p>
        <span class="matching">{{ matchingSlice }}</span>
        <span>{{ initialSlice }}</span>
    </p>
</template>

<script setup lang="ts">
defineProps({
    initialSlice: {
        type: String, 
        required: true,
    },
    matchingSlice: {
        type: String, 
        required: true,
    }
})
</script>

<style scoped>
.matching {
    color: green;
}
</style>