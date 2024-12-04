<script setup>
import ModuleLayout from "@/components/module/ModuleLayout.vue";
import isMobile from 'is-mobile';

const apiURL = import.meta.env.VITE_API_URL;
const data = await $fetch(`${apiURL}/homepage?view=${!isMobile() ? 'desktop' : 'mobile'}`)
</script>

<template>
    <ModuleLayout :modules="data?.modules" />
</template>
