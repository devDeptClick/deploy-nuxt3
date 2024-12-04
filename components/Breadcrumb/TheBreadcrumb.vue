<script setup>
defineProps({
    data: Object,
    currentName: String
})
</script>

<template>
    <section class="page-title" style="padding: 0 !important">
        <div class="container">
            <div class="row">
                <div class="col-lg-12">
                    <div class="theme-breadcrumb-box">
                        <nav aria-label="breadcrumb" class="page-breadcrumb">
                            <ol class="breadcrumb">
                                <li class="breadcrumb-item">
                                    <RouterLink to="/" style="font-weight: 600;"><i class="las la-home me-1"></i>Trang
                                        Chủ</RouterLink>
                                </li>

                                <li class="breadcrumb-item" aria-current="page" style="font-weight: 600;"
                                    v-for="(item, index) in data" :key="index">
                                    <RouterLink
                                        :to="'/the-loai/' + (item?.slug ? item.slug : item.category_slug) + '/' + (item?.id ? item.id : item.category_id)"
                                        style="font-weight: 600;">{{ item.title ? item.title : item.category_title }}
                                    </RouterLink>
                                </li>

                                <li class="breadcrumb-item" v-if="currentName">
                                    {{ currentName }}
                                </li>
                            </ol>
                        </nav>
                    </div>
                </div>
            </div>
            <!-- / .row -->
        </div>
        <!-- / .container -->
    </section>
</template>

<style scoped>
ol {
    margin-bottom: 0;
    padding: 0;
}

.breadcrumb {
    background: unset !important;
}

.breadcrumb-item+.breadcrumb-item:before {
    color: #003789;
    content: "\f054";
    font-family: 'Line Awesome Free';
    font-weight: 700;
}

.theme-breadcrumb-box {
    padding: 0 !important;
}
</style>
