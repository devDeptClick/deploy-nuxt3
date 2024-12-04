<script setup>
defineProps({
    module: Object,
})
</script>

<template>
    <section>
        <div class="container">
            <div class="row align-items-center">
                <div class="col-12 col-lg-6 position-relative">
                    <div class="text-end mb-n10">
                        <div class="round-text">
                            <svg version="1.1" xmlns="http://www.w3.org/2000/svg"
                                xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" width="100%" height="100%"
                                viewBox="0 0 180 180" enable-background="new 0 0 180 180" xml:space="preserve">
                                <defs>
                                    <path id="circlePath"
                                        d="M 90, 90 m -60, 0 a 60,60 0 0,1 120,0 a 60,60 0 0,1 -120,0 "></path>
                                </defs>
                                <circle cx="90" cy="90" r="75" fill="none"></circle>
                                <g>
                                    <use xlink:href="#circlePath" fill="none"></use>
                                    <text>
                                        <textPath xlink:href="#circlePath">Watch Over Video • Watch Over Video •
                                        </textPath>
                                    </text>
                                </g>
                            </svg>
                            <a class="video-popup text-btn-icon popup-youtube">
                                <i class="las la-play"></i>
                            </a>
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-8 mx-auto">
                            <img class="img-fluid" :src="JSON.parse(module.module_metadata).media_data.images[0]"
                                alt="">
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-8 ms-auto">
                            <img class="img-fluid border border-10 border-white mt-n10"
                                :src="JSON.parse(module.module_metadata).media_data.images[1]" alt="">
                        </div>
                    </div>
                    <div class="text-large-style">
                        <h3>Since 1985</h3>
                    </div>
                </div>
                <div class="col-12 col-lg-6 ps-lg-10 mt-5 mt-lg-0">
                    <div class="theme-title mb-4">
                        <h6 v-html="module.module_name_second"></h6>
                        <h2 v-html="module.module_title"></h2>
                        <div v-html="module.module_description">

                        </div>
                    </div>
                    <div class="featured-item style-1 mt-5"
                        v-for="(about, index) in JSON.parse(module.module_metadata).content_data" :key="index">
                        <div class="featured-icon" v-html="about.item_slide_svg"></div>

                        <div class="featured-desc">
                            <div class="featured-title">
                                <h5>{{ about.item_slide_title }}</h5>
                            </div>
                            <!-- <p>{{ about.item_slide_value }}</p> -->
                        </div>
                    </div>

                </div>
            </div>
        </div>
    </section>
</template>

<style scoped>
.ms-auto,
.mx-auto {
    margin-left: auto !important;
}

.mb-n10,
.my-n10 {
    margin-bottom: -4.5rem !important;
}

.border-10 {
    border-width: 10px !important;
}

.mt-n10,
.my-n10 {
    margin-top: -4.5rem !important;
}

.border-white {
    border-color: rgba(255, 255, 255, 1) !important;
}

.text-end {
    text-align: right !important;
}

.round-text {
    position: relative;
    width: 180px;
    overflow: hidden;
    height: 180px;
    z-index: 9;
    display: inline-block;
    background: #ffffff;
    border-radius: 100%;

}

.round-text text {
    text-transform: uppercase;
    font-size: 14.2px !important;
    letter-spacing: 2px;
    font-weight: 600;
    fill: #12181c;
}

.round-text svg {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    animation: rotationBreak 9s ease-in-out infinite normal;

}


@keyframes rotationBreak {
    0% {
        transform: rotate(360deg);
    }
}

.text-btn-icon {
    font-size: 45px;
    color: #ffffff !important;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: #003789;
    width: 90px;
    height: 90px;
    border-radius: 100%;
    display: flex;
    justify-content: center;
    align-items: center;

}

.text-large-style {
    position: absolute !important;
    top: calc(100% / 2);
    left: calc(100% / -2);
    width: 100%;
    transform: rotate(90deg);
    margin-left: 50px;
}

.text-large-style h3 {
    font-size: 100px;
    line-height: 1;
    opacity: 0.5;
    text-transform: uppercase;
    color: #ffffff;
    text-shadow: 1px 1px 1px #12181c, 3px 3px 5px #003789;
    white-space: nowrap;
}

.theme-title h6 {
    position: relative;
    display: inline-block;
    font-weight: 600;
    text-transform: uppercase;
    margin-bottom: 10px;
    color: #003789;
}

.theme-title h2 {
    margin-bottom: 15px;
    font-size: 50px;
    font-style: normal;
    line-height: 60px;
    font-weight: 600;
    margin-bottom: 10px;
    position: relative;
}

.theme-title p {
    font-size: 16px;
    margin-bottom: 0;
    font-weight: 500;
    line-height: 30px;
}

.featured-item.style-1 {
    display: flex;
    align-items: center;
}

.featured-item.style-1 .featured-icon {
    background: #f8f8f8;
    margin-right: 20px;
    flex-shrink: 0;
    position: relative;
    z-index: 9;
    width: 70px;
    height: 70px;
    font-size: 40px;
    line-height: 70px;
    text-align: center;
    color: #003789;
    display: inline-block;
    overflow: hidden;
}

.featured-title h5 {
    margin-bottom: 10px;
    text-transform: capitalize;
    font-size: 20px;
    font-style: normal;
    margin-bottom: 10px;
    font-weight: 600;
    line-height: 26px;
    color: #1176d3;

}


.featured-icon:deep(svg) {
    width: 40px;
    fill: #003789;
}
</style>
