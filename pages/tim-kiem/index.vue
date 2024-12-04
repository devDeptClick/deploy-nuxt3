<script setup>
import { useRoute } from 'vue-router'
import ProductList from '@/components/Category/ProductList.vue';
import TheBreadcrumb from '@/components/Breadcrumb/TheBreadcrumb.vue';

const route = useRoute()
const apiURL = import.meta.env.VITE_API_URL;
const data = await $fetch(`${apiURL}/search?q=${route.query.q}`)
const dataCate = ref(data)

watch(route, async (current) => {
    dataCate.value = await $fetch(`${apiURL}/search?q=${current.query.q}&page=${current.query.page}`, {
        async onRequest() {
            document.getElementById("ht-preloader").style.display = "flex";
        },
        async onResponse() {
            document.getElementById("ht-preloader").style.display = "none";
        },
    })
    window.scrollTo({ top: 0 });
})
</script>

<template>
    <div>
        <TheBreadcrumb :data="[]" currentName="Sản phẩm" />
        <ProductList :categories="dataCate" />
    </div>
</template>

<style scoped></style>
