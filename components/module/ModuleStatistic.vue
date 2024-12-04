<script setup>
import { onMounted } from 'vue'
defineProps({
    module_metadata: Object
})
</script>

<template>
    <section>
        <div class="container">
            <div class="white-bg p-6">
                <div class="row">
                    <div class="col-12 col-lg-3 col-md-6" v-for="(statistic, index) in module_metadata.content_data"
                        :key="index">
                        <div class="counter">
                            <div class="counter-icon" v-html="statistic.item_slide_svg" v-if="statistic.imgorsvg"></div>
                            <div class="counter-icon" v-else>
                                <img :src="statistic.item_slide_image" :alt="statistic.item_slide_image"
                                    :title="statistic.item_slide_image_title">
                            </div>
                            <div class="counter-desc">
                                <div class="o-text-odometer" format="d"
                                    :data-odometer="statistic.item_slide_value" />
                                <span>{{statistic.item_slide_value}}+</span>
                                <p class="counter-icon-title">{{ statistic.item_slide_title }}</p>
                            </div>
                        </div>
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

.counter {
    position: relative;
    z-index: 9;
    display: flex;
    align-items: center;
}

.counter-icon {
    margin-right: 20px;
}

.counter-icon-title {
    display: block;
    color: #4a5156;
    font-size: 18px;
    font-weight: 500;
    position: relative;
    margin-bottom: 0;
    line-height: 24px;
}

.counter span {
    font-size: 50px;
    font-weight: 600;
    line-height: 1;
    color: #12181c;
    display: inline-block;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    -webkit-text-stroke: 1px #12181c;
}

.counter span+span {
    font-weight: normal;
    font-size: 30px;
    color: var(--themeht-text-color);
    margin-bottom: 0;
}
</style>
