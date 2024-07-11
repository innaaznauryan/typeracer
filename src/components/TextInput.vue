<template>
    <textarea 
        :value="modelValue" 
        @input="handleInput" 
        :class="{'mistake': hasMistake}"
        rows="10"
        cols="50">
    </textarea>
</template>

<script setup lang="ts">
defineProps({
    modelValue: {
        type: String,
        required: true,
    },
    hasMistake: {
        type: Boolean,
        required: true
    }
})
const emit = defineEmits(["update:modelValue"])

const handleInput = (e: Event) => {
    const target = e.target as HTMLTextAreaElement | null;
    emit("update:modelValue", target?.value)
}
</script>

<style scoped>
textarea {
    resize: none;
    outline: none;
}
.mistake {
    border: 1px solid red;
}
</style>