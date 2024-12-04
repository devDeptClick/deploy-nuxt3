<script setup>
import { useRoute } from 'vue-router'
import ModuleLayout from '@/components/module/ModuleLayout.vue';
import ProductList from '@/components/Category/ProductList.vue';
import PostList from '@/components/Category/PostList.vue';
import TheBreadcrumb from '@/components/Breadcrumb/TheBreadcrumb.vue';

const route = useRoute()
const apiURL = import.meta.env.VITE_API_URL;

const filters = {
  page: route.query.page ? route.query.page : 1,
  price_from: route.query.price_from || '',
  price_to: route.query.price_to || '',
  sorted_price: route.query.sorted_price || '',
  item_per_page: route.query.item_per_page || 12,
  sorted_by: route.query.sorted_by || ''
}

const data = await $fetch(`${apiURL}/${route.params.path}-ca${route.params.id}?price_from=${filters.price_from}&price_to=${filters.price_to}&sorted_price=${filters.sorted_price}&item_per_page=${filters.item_per_page}&sorted_by=${filters.sorted_by}&page=${filters.page}`)
const dataCate = ref(data)

useSeoMeta({
  title: data.category.category_title,
  ogTitle: data.category.category_title,
  description: data.category.category_title,
  ogDescription: data.category.category_title,
})

watch(route, async (current) => {
  console.log(current.sorted_price)
  dataCate.value = await $fetch(`${apiURL}/${current.params.path}-ca${current.params.id}?price_from=${current.query.price_from}&price_to=${current.query.price_to}&sorted_price=${current.query.sorted_price}&item_per_page=${current.query.item_per_page}&sorted_by=${current.query.sorted_by}&page=${current.query.page}`, {
    async onRequest() {
      document.getElementById("ht-preloader").style.display = "flex";
    },
    async onResponse({ request, response }) {
      console.log("[fetch response]", request, response.status, response.body);
      document.getElementById("ht-preloader").style.display = "none";
    },
  })
  window.scrollTo({ top: 700 });
})
</script>

<template>
  <div>
    <TheBreadcrumb :data="dataCate.categories" :currentName="dataCate.category.category_title" />
    <ModuleLayout :modules="dataCate.modules" />
    <ProductList :categories="dataCate" v-if="dataCate.type === 1" />
    <PostList :categories="dataCate" v-if="dataCate.type === 2" />
  </div>
</template>

<style scoped></style>
