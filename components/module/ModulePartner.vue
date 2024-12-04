<script setup>
defineProps({
    module: Object,
})
</script>

<template>
    <section class="partner-area">
        <div class="container">
            <div class="row">
                <div class="col-xl-12">
                    <div class="partner-box">
                        <!--Start Single Partner Logo Box-->
                        <div class="single-partner-logo-box"
                            v-for="(partner, index) in JSON.parse(module.module_metadata).content_data" :key="index">
                            <div class="overlay-box text-center">
                                <span v-if="partner.imgorsvg === 'image'">
                                    <img :src="partner.item_slide_image" :alt="partner.item_slide_image_alt"
                                        :title="partner.item_slide_image_title" width="100">
                                </span>

                                <span v-else v-html="partner.item_slide_svg"></span>
                                    
                            </div>
                        </div>
                        <!--End Single Partner Logo Box-->
                    </div>
                </div>
            </div>
        </div>
    </section>
</template>

<style scoped>
section {
    background: #f7f8f8;
}

.partner-box {
    position: relative;
    overflow: hidden;
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    align-items: center;
}

.single-partner-logo-box {
    position: relative;
    overflow: hidden;
    flex: 0 0 20%;
    max-width: 20%;
    width: 100%;
    margin-bottom: 40px;
}
</style>
