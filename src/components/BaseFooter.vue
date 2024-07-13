<template>
    <footer>
        <p>© {{ year }} Coded by Inna Aznauryan</p>
        <p class="icons">
            <a v-for="link in links" :href="link.href" target="_blank">
                <component 
                    :is="link.icon" 
                    color="#646cff" 
                    size="24" 
                    stroke="2">
                </component>
            </a>
        </p>
    </footer>
</template>

<script setup lang="ts">
import { IconBrandGithub } from '@tabler/icons-vue';
import { IconBrandLinkedin } from '@tabler/icons-vue';

const year = new Date().getFullYear();

const links = [
    {
        href: "https://www.linkedin.com/in/inna-aznauryan/",
        icon: IconBrandLinkedin
    },
    {
        href: "https://github.com/innaaznauryan",
        icon: IconBrandGithub
    }
]
</script>

<style scoped>
footer {
    padding-top: 40px;
}
.icons {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 10px;
}
</style>