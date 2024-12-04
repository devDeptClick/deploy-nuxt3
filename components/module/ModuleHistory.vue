<script setup>
defineProps({
    module: Object,
})
</script>

<template>
    <section class="ttm-row bg-layer-section bg-layer break-1199-colum clearfix">
        <div class="container-fluid">
            <!-- row -->
            <div class="row row-equal-height" style="gap: 0 !important;">
                <div class="col-lg-6" style="padding: 0 !important;">
                    <!-- col-bg-img-four -->
                    <div class="col-bg-img-four ttm-col-bgimage-yes ttm-bg padding-12"
                        :style="{ backgroundImage: `url(${module.module_avatar})` }"
                        style=" height: 100%; background-repeat: no-repeat; background-position: center; background-size: cover; width: 100%;">
                        <div class="ttm-col-wrapper-bg-layer ttm-bg-layer"></div>
                    </div><!-- col-bg-img-four end -->
                </div>
                <div class="col-lg-6" style="padding: 0 !important;">
                    <div class="ttm-col-bgcolor-yes ttm-bg ttm-right-span ttm-bgcolor-darkgrey p-5 res-767-p-30"
                        style="background-color: #020d26;">
                        <div class="ttm-col-wrapper-bg-layer ttm-bg-layer"></div>
                        <!--layer-content-->
                        <div class="layer-content">
                            <!--section-title-->
                            <div class="section-title">
                                <h5 class="text-white">{{ module.module_name_second }}</h5>
                                <h2 class="title text-white" v-html="module.module_title"></h2>
                                <div class="heading-seperator"><span></span></div>
                            </div><!--section-title end-->
                            <div id="ttmscore-timeline" class="ttmscore-timeline-wrapper clearfix">
                                <!--ttmscore-timeline-round-->
                                <div
                                    class="timeline ttmscore-timeline ttmscore-timeline-style-1 ttmscore-timeline-round text-white">
                                    <div class="timeline-panel"
                                        v-for="(history, index) in JSON.parse(module.module_metadata).content_data"
                                        :key="index">
                                        <div class="timeline-date">
                                            <span class="timeline-year">{{ history.item_slide_value }}</span>
                                        </div>
                                        <div class="timeline-body">
                                            <h5 class="title text-white">{{ history.item_slide_title }}</h5>
                                            <p>{{ history.item_slide_desc }}</p>
                                        </div>
                                    </div>


                                </div><!--ttmscore-timeline-round end-->
                            </div>
                        </div><!--layer-content end-->
                    </div>
                </div>
            </div><!--row end-->
        </div>
    </section>
</template>

<style scoped>
.text-white {
    color: rgba(255, 255, 255, 1) !important;
}

h5 {
    font-size: 20px;
    font-style: normal;
    margin-bottom: 10px;
    font-weight: 600;
    line-height: 26px;
}

h2 {
    font-size: 50px;
    font-style: normal;
    line-height: 60px;
    font-weight: 600;
    margin-bottom: 10px;
    position: relative;
}

.title p {
    font-size: 44px;
    line-height: 1.5;
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
    bottom: 10px;
    width: 59px;
    border-bottom-width: 3px;
    border-bottom-style: solid;
}

.timeline {
    position: relative;
}

.timeline .timeline-panel {
    position: relative;
    padding-left: 100px;
}

.timeline.ttmscore-timeline-round .timeline-year,
.timeline.ttmscore-timeline-round .timeline-year::before {
    border-radius: 50%;
}

.timeline.ttmscore-timeline-round .timeline-year,
.timeline.ttmscore-timeline-round .timeline-year::before {
    border-radius: 50%;
    background-color: #003789;
    border: 3px solid #fff;
}

.timeline .timeline-year {
    left: 0;
    position: absolute;
    top: 5px;
    width: 74px;
    height: 74px;
    padding: 0px;
    line-height: 70px;
    text-align: center;
    color: #fff;
    font-size: 16px;
    background-color: #e9103a;
    font-weight: 600;
}

.timeline .timeline-panel .timeline-body {
    position: relative;
    padding-top: 0;
    padding-bottom: 20px;
    margin-left: 35px;
}

.timeline .timeline-body h5:after {
    background-color: #003789;
    content: "";
    position: absolute;
    border-radius: 50%;
    left: -34px;
    top: 10px;
    width: 10px;
    height: 10px;
}

.timeline .timeline-panel .timeline-body p:before {
    content: "";
    position: absolute;
    left: -30px;
    top: 11px;
    width: 2px;
    height: 100%;
    background: #f0f0f0;
    background: rgba(255, 255, 255, .1);
}
</style>
