<template>
    <textarea 
        :value="modelValue" 
        @input="handleInput" 
        :class="{'mistake': hasMistake}" >
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
    width: 60%;
    height: 200px;
}
.mistake {
    border: 1px solid red;
}

@media (max-width: 768px) {
    textarea {
        width: 80%;
    }
}
</style>