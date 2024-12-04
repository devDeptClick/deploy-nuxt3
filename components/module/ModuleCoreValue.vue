<script setup>
defineProps({
    module: Object,
})
</script>

<template>
    <section>
        <div class="container">
            <div class="row">
                <div class="col-lg-9 col-md-12">
                    <!-- section title -->
                    <div class="section-title clearfix">
                        <h2 class="title fw-bold" v-html="module.module_title"></h2>
                        <div class="heading-seperator" v-html="module.module_description"></div>
                    </div>
                    <!-- section title end -->
                </div>
            </div>

            <!-- ttm-section-info_box -->
            <div class="intro-content ttm-section-info_box box-shadow ttm-bgcolor-white clearfix">
                <div class="row row-equal-height ttm-box-seperator style2 res-991-pb-30 res-991-pt-30">

                    <div class="col-lg-4 col-md-4"
                        v-for="(core, index) in JSON.parse(module.module_metadata).content_data" :key="index">
                        <div class="featured-box text-center">
                            <div class="featured-icon icon-shape-square icon-bgcolor-grey featured-icon-core" v-html="core.item_slide_svg">
                            </div>

                            <div class="featured-content">
                                <div class="featured-title">
                                    <h5>{{ core.item_slide_title }}</h5>
                                </div>
                                <div class="featured-desc">
                                    <p>{{ core.item_slide_desc }}</p>
                                </div>
                            </div>
                        </div>
                    </div>

                </div>
            </div>
        </div>
    </section>
</template>

<style scoped>
.fw-bold {
    font-weight: 700 !important;
}

h2 {
    font-size: 50px;
    font-style: normal;
    line-height: 60px;
    font-weight: 600;
    margin-bottom: 10px;
    position: relative;
}

.featured-title h5 {
    color: #1176d3;
}

.featured-title h5 {
    margin-bottom: 10px;
    text-transform: capitalize;
}

h5 {
    font-size: 20px;
    font-style: normal;
    margin-bottom: 10px;
    font-weight: 600;
    line-height: 26px;
}

p {
    line-height: 1.7;
    margin: 0 0 15px !important;
}

.heading-seperator {
    position: relative;
}

.heading-seperator:after {
    left: 0;
    margin-left: 0;
}

.heading-seperator:after {
    border-bottom-color: #003789;
    content: "";
    position: absolute;
    bottom: 55px;
    width: 59px;
    border-bottom-width: 3px;
    border-bottom-style: solid;
}

.ttm-box-seperator.style2.row {
    padding: 40px 17px 0px 12px;
    margin-top: 24px;
}

.row {
    gap: 20px 0;
}

.featured-box .featured-icon.icon-shape-square {
    margin-bottom: 25px;
    background-color: #003789;
    color: #fff;
    display: inline-block;
    width: 80px;
    height: 80px;
    line-height: 80px;
    font-size: 30px;
    text-align: center;
    border: 1px solid transparent;
    border-radius: 0;
}

.featured-icon {
    transition: all 0.5s ease-in-out 0s;
    font-weight: 400;
}
</style>
