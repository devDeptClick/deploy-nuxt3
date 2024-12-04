<script setup>
defineProps({
    links: Object,
})
</script>

<template>
    <nav aria-label="..." class="mt-5">
        <ul class="pagination justify-content-center">
            <li class="page-item">
                <NuxtLink class="page-link text-dark"
                    :to="{ query: { q: $route.query.q, page: ($route.query.page && $route.query.page != 1) ? Number($route.query.page) - 1 : 1 } }">
                    Trước
                </NuxtLink>
            </li>

            <li class="page-item" v-for="(paginate, index) in links" :key="index">
                <NuxtLink :to="{ query: { q: $route.query.q, page: Number(paginate.label) ? paginate.label : 1 } }"
                    class="page-link"
                    :class="($route.query.page ? $route.query.page : '1') === paginate.label ? 'page-current-active text-white' : 'text-dark'">
                    {{ paginate.label }}
                </NuxtLink>
            </li>

            <li class="page-item">
                <NuxtLink class="page-link text-dark"
                    :to="{ query: {  q: $route.query.q, page: $route.query.page ? Number($route.query.page) + 1 : 2} }">Sau
                </NuxtLink>
            </li>
        </ul>
    </nav>
</template>

<style scoped>
.page-current-active {
    background-color: #003789;
}
</style>
