<script setup>
defineProps({
    module: Object,
})
</script>

<template>
    <section class="ls ms s-pt-60 s-pb-10 s-py-lg-100">
        <div class="container">
            <div class="row c-mb-50 c-mb-lg-0">
                <div class="col-12 col-lg-4 mt-3"
                    v-for="(feature, index) in JSON.parse(module.module_metadata).content_data" :key="index">
                    <div class="icon-box px-xl-50 text-center">
                        <div class="fs-50" v-html="feature.item_slide_svg" v-if="feature.imgorsvg === 'svg'"></div>

                        <div class="fs-50" v-else>
                            <img :src="feature.item_slide_image" :alt="feature.item_slide_image_alt"
                                :title="feature.item_slide_image_title" width="64">
                        </div>

                        <h4>
                            <span class="color-main" style="color: #003789;">{{ feature.item_slide_title }}</span>
                        </h4>
                        <p>{{ feature.item_slide_desc }}</p>
                    </div>
                </div>
            </div>
        </div>
    </section>
</template>

<style scoped>
.color-main {
    font-weight: 600;
}
</style>
