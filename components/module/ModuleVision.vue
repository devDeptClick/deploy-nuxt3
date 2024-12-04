<script setup>
defineProps({
    module: Object,
})
</script>

<template>
    <section class="ttm-row feature-blog-section break-991-colum ttm-bgcolor-white clearfix"
        style="background-color: #ffff;">
        <div class="container">
            <!-- row -->
            <div class="row row-equal-height">
                <div class="col-lg-5 col-md-12">
                    <!-- about-img -->
                    <div class="about-img mb-35" style="margin: auto;height: 100%;">
                        <img class="img-fluid" :src="module.module_avatar" alt="tam-nhin" style="height: 100%;">
                    </div>
                    <!-- about-img end -->
                </div>

                <div class="col-lg-7 col-md-12 mt-md-3">
                    <div class="pl-15 pr-10 lg-pl-15 res-991-pl-0">
                        <!-- section title -->
                        <div class="section-title without-seprator clearfix">
                            <h3 class="title" style="font-weight: 600;" v-html="module.module_title"></h3>
                            <div v-html="module.module_description"></div>
                        </div>
                        <!-- section title end -->

                        <!-- row-->
                        <div class="row mt-3">
                            <div class="col-md-12 col-sm-12">
                                <!--featured-box-->
                                <div class="featured-box style8 left-icon mb-4"
                                    style="display: flex;gap: 10px;align-items: center;"
                                    v-for="(vision, index) in JSON.parse(module.module_metadata).content_data"
                                    :key="index">
                                    <div class="featured-icon" v-html="vision.item_slide_svg"></div>

                                    <div class="featured-content">
                                        <div class="featured-title">
                                            <h5>{{ vision.item_slide_title }}</h5>
                                        </div>

                                        <div v-html="vision.item_slide_desc"></div>
                                            
                                    </div>
                                </div>
                                <!-- featured-box end-->
                            </div>
                        </div><!-- row end-->
                    </div>
                </div>
            </div><!-- row end -->
        </div>
    </section>
</template>

<style scoped>
.title p {
    font-size: 44px;
    line-height: 1.5;
}
.featured-title h5 {
    color: #1176d3;
    font-weight: 600;
}
.featured-title h5 {
    margin-bottom: 10px;
    text-transform: capitalize;
}
.featured-icon:deep(svg) {
    width: 40px;
    fill: #003789;
}
</style>
