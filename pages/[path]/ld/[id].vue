<script setup>
import ModuleLayout from "@/components/module/ModuleLayout.vue";
import { ref, watch } from 'vue'
import { useRoute } from 'vue-router';
import isMobile from 'is-mobile';

const route = useRoute()

const apiURL = import.meta.env.VITE_API_URL;
const data = ref(null);

data.value = await $fetch(`${apiURL}/${route.params.path}-ld${route.params.id}?view=${!isMobile() ? 'desktop' : 'mobile'}`)

watch(route, async (current) => {
    data.value = await $fetch(`${apiURL}/${current.params.path}-ld${current.params.id}?view=${!isMobile() ? 'desktop' : 'mobile'}`)
})

</script>

<template>
    <ModuleLayout :modules="data?.modules" />
</template>
