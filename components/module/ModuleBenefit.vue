<script setup>
defineProps({
    module: Object,
})
</script>

<template>
    <!-- {{ module }} -->
    <section class="ttm-row service-section style2 clearfix">
        <div class="container">
            <!-- row -->
            <div class="row">
                <div class="col-lg-9 col-md-12">
                    <!-- section-title -->
                    <div class="section-title style2 clearfix">
                        <h2 class="title" v-html="module.module_title"></h2>
                        <div class="content" v-html="module.module_description"></div>
                    </div>
                    <!-- section-title end -->
                </div>
                <div class="col-lg-3 col-md-12 text-lg-end text-md-start"
                    style="margin-bottom: 35px; margin-top: 30px;">
                    <a v-if="module.module_title_url_button"
                        class="ttm-btn ttm-btn-size-md ttm-btn-shape-square ttm-btn-style-border ttm-btn-color-black mt-30 mb-35 res-991-mt-0 float-right"
                        :href="module.module_url_button">{{ module.module_title_url_button }}</a>
                </div>
            </div><!-- row end -->
            <div class="row row-equal-height">
                <div class="col-lg-4 col-md-6 col-sm-12"
                    v-for="(benefit, index) in JSON.parse(module.module_metadata).content_data" :key="index">
                    <!-- featured-box -->
                    <div class="featured-box style2 text-center">
                        <div class="featured-icon" v-html="benefit.item_slide_svg" v-if="benefit.imgorsvg === 'svg'">
                        </div>

                        <div class="featured-icon" v-else>
                            <img :src="benefit.item_slide_image" :alt="benefit.item_slide_image_alt"
                                :title="benefit.item_slide_image_title">
                        </div>

                        <div class="featured-content">
                            <div class="featured-title">
                                <p>{{ benefit.item_slide_title }}</p>
                            </div>
                            <div class="featured-desc">
                                <p class="mb-2">{{ benefit.item_slide_desc }}</p>

                                <a class="ttm-btn ttm-btn-color-skincolor btn-inline"
                                    v-if="benefit.item_slide_title_url" :href="benefit.item_slide_url">{{
                                        benefit.item_slide_title_url }}</a>
                            </div>
                        </div>
                    </div><!-- featured-box end-->
                </div>
            </div><!-- row end-->
        </div>
    </section>
</template>
<style scoped>
.featured-title {
    font-size: 1.25rem;
}

.ttm-btn.ttm-btn-color-black.ttm-btn-style-border {
    color: #ffff;
    background-color: transparent;
    border: 1px solid #003789;
    padding: 11px 32px 11px 31px;
    background-color: #003789;
}

.section-title.style2 {
    margin-bottom: 40px;
    position: relative;
    display: flex;
    align-items: center;
    justify-content: space-between;
}

.section-title.style2 h2 {
    width: 56%;
    padding: 0 60px 0 0;
    font-weight: 600;
    font-size: 45px;
    line-height: 50px;
    color: #020d26;
    padding-bottom: 0px;
    margin-bottom: 15px;
    margin-top: 15px;

}

.section-title>.content {
    width: 40%;
}

.section-title.style2:after {
    content: "";
    right: 50%;
    height: 82%;
    position: absolute;
    top: 10%;
    width: 1px;
    border-right: 1px solid rgba(122, 122, 122, .34);
}

.clearfix::after {
    display: block;
    clear: both;
    content: "";
}

.service-section.style2 .row .col-lg-4:nth-child(-n+3) .featured-box.style2 {
    margin-bottom: 40px;
}

.featured-box.style2 {
    padding: 36px 15px 43px;
    border: 1px solid transparent;
    margin-top: 15px;
    border-color: #f3f7f9;
}

.featured-box {
    transition: all 0.5s ease-in-out 0s;
    position: relative;
}

.featured-icon {
    transition: all 0.5s ease-in-out 0s;
    font-weight: 400;
}

.featured-box.style2 .featured-content {
    padding-top: 18px;
}

.featured-box.style2:after {
    content: "";
    position: absolute;
    bottom: 0;
    left: 0;
    width: 50%;
    height: 100%;
    z-index: -10;
    -webkit-transition: all .3s linear;
    -moz-transition: all .3s linear;
    transition: all .3s linear;
}

.featured-box.style2:after {
    background-color: #f3f7f9;
}

.featured-box.style2:hover {
    border-color: #ff5e14;
}

.featured-box.style2:hover:after {
    border-color: #ff5e14;
    width: 100%;
}

.ttm-btn.btn-inline {
    text-transform: capitalize;
    font-size: 15px;
    font-weight: 600;
    display: inline-block;
    padding: 0 !important;
    border: 1px solid transparent;
    background-color: transparent;
    color: #ff5e14;
}
</style>
