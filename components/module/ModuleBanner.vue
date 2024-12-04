<script setup>
import { Swiper, SwiperSlide } from 'swiper/vue';

// Import Swiper styles
import 'swiper/css';

import 'swiper/css/pagination';
import 'swiper/css/navigation';

// import required modules
import { Autoplay, Pagination, Navigation } from 'swiper/modules';

defineProps({
    module_metadata: Object
})

</script>

<template>
    <!-- {{ module_metadata.content_data }} -->
    <section class="banner-slider py-0">
        <swiper :spaceBetween="30" :centeredSlides="true" :autoplay="{
            delay: 3500,
            disableOnInteraction: false,
        }" :navigation="true" :modules="[Autoplay, Pagination, Navigation]" class="mySwiper">
            <swiper-slide v-for="(banner, index) in module_metadata.content_data" :key="index" class="animeslide-slide">
                <img :src="banner.item_slide_image" :alt="banner.item_slide_image_alt"
                    :title="banner.item_slide_image_title" />
                <div class="banner-content">
                    <div class="container">
                        <div class="row">
                            <div class="col-xxl-7 col-xl-8 col-lg-10 col-12">
                                <h2 class="mb-4 slide_title" data-animate="bottom">{{ banner.item_slide_title }}</h2>
                                <p data-animate="bottom" class="slide_desc">{{ banner.item_slide_desc }}</p>

                                <div class="btn-box mt-5 button-seemore" data-animate="bottom"
                                    v-if="banner.item_slide_url">
                                    <a class="primary-btn" :href="banner.item_slide_url">
                                        <span>{{ banner.item_slide_title_url }}</span>
                                    </a>
                                </div>

                            </div>
                        </div>
                    </div>
                </div>
            </swiper-slide>
        </swiper>
    </section>
</template>

<style scoped>
.swiper {
    width: 100%;
    height: 88vh;
}

.swiper-slide {
    text-align: center;
    font-size: 18px;
    background: #fff;

    /* Center slide text vertically */
    display: flex;
    justify-content: center;
    align-items: center;
}

.swiper-slide img {
    display: block;
    width: 100%;
    height: 100%;
    object-fit: cover;
}

.banner-content {
    position: absolute;
    top: 80px;
}

[data-animate] {
    opacity: 0;
    transition: all 0.8s ease-out;
}

[data-animate="bottom"] {
    transform: translate3d(0, 15px, 0);
}

.animeslide-slide.swiper-slide-active [data-animate] {
    opacity: 1;
    transform: none;
}

.banner-slider h2 {
    color: #ffff;
    position: relative;
    font-weight: 600;
    transition-delay: 0.3s;
    opacity: 0;
}

.banner-content p {
    color: #ffff;
    position: relative;
    font-weight: 600;
    transition-delay: 0.6s;
    opacity: 0;
}

.button-seemore {
    transition-delay: 0.9s;
    opacity: 0;
}

.primary-btn {
    position: relative;
    transition: all 0.6s;
}

.primary-btn {
    color: #000;
    background: none;
    padding: 16px 35px;
    overflow: hidden;
    border-radius: 0;
    position: relative;
    position: relative;
    display: inline-block;
    cursor: pointer;
    outline: none;
    border: 0;
    vertical-align: middle;
    text-decoration: none;
}

.primary-btn::before {
    left: 4px;
    z-index: 1;
    opacity: 0;
    background: #003789;
    transform: scale(0.1, 1);
}

.primary-btn::after {
    transition: all 0.6s;
    background: #003789;
}

.primary-btn::before,
.primary-btn::after {
    content: '';
    position: absolute;
    transition: all 0.6s;
    bottom: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 1;
}

.primary-btn:hover::before {
    opacity: 1;
    transform: scale(1, 1);
}

.primary-btn:hover::after {
    transform: scale(1, .1);
    opacity: 0;
}

.primary-btn span {
    transition: all 0.6s;
    z-index: 9;
    position: relative;
    color: #ffffff;
}

@media screen and (max-width: 768px) {
    .slide_title{
        font-size: 16px;
    }
    .slide_desc{
        font-size: 14px;;
    }
    .primary-btn{
        padding: 8px 25px;
    }
}
</style>