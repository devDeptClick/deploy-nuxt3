<script setup>
import { Swiper, SwiperSlide } from 'swiper/vue';

// Import Swiper styles
import 'swiper/css';

import 'swiper/css/pagination';
import 'swiper/css/navigation';

// import required modules
import { Autoplay, Pagination, Navigation } from 'swiper/modules';

defineProps({
    module: Object,
})

</script>

<template>
    <!-- {{ module }} -->
    <section class="testimonials-section" :style="{ backgroundImage: `url(${module.module_background})` }">
        <div class="container">
            <div class="row">
                <div class="col-lg-8">
                    <div class="sec-title">
                        <div class="sub-title" v-html="module.module_title"></div>
                        <h2 v-html="module.module_description"></h2>
                    </div>
                    <div class="testimonial-block-area">
                        <swiper :slidesPerView="3" :centeredSlides="true" :spaceBetween="30" :loop="true"
                            :loopedSlides="1" :autoplay="{
                                delay: 2500,
                                disableOnInteraction: false,
                            }" :navigation="false" :modules="[Autoplay, Navigation]"
                            class="mySwiper testimonial-thumbs">
                            <swiper-slide v-for="(customer, index) in JSON.parse(module.module_metadata).content_data"
                                :key="index" class="animeslide-slide">
                                <div class="author-thumb">
                                    <img :src="customer.item_slide_image" :alt="customer.item_slide_image_alt"
                                        width="80">
                                </div>
                            </swiper-slide>
                        </swiper>


                        <div class="swiper-container testimonial-content">
                            <div class="swiper-wrapper">
                                <swiper :centeredSlides="true" :spaceBetween="30" :loop="true" :loopedSlides="1"
                                    :autoplay="{
                                        delay: 2500,
                                        disableOnInteraction: false,
                                    }" :navigation="false" :modules="[Autoplay, Pagination]" class="mySwiper">
                                    <swiper-slide class="swiper-slide"
                                        v-for="(customer, index) in JSON.parse(module.module_metadata).content_data"
                                        :key="index">
                                        <!-- Testimonial Block -->
                                        <div class="testimonial-block">
                                            <div class="inner-box">
                                                <div class="text">{{ customer.item_slide_desc }}</div>
                                                <div class="author-title">{{ customer.item_slide_title }} <span>/ Khách
                                                        Hàng</span></div>
                                            </div>
                                        </div>
                                    </swiper-slide>
                                </swiper>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="col-lg-4">
                    <div class="icon-box wow fadeInUp" data-wow-duration="1500ms">
                        <div class="icon" style="background-color: #003789;">
                            <span class="flaticon-maintenance"></span>
                        </div>
                        <p class="icon-box-title">Bền bỉ</p>
                    </div>
                    <div class="icon-box wow fadeInUp" data-wow-duration="1500ms">
                        <div class="icon" style="background-color: #003789;"><span class="flaticon-settings"></span>
                        </div>
                        <p class="icon-box-title">Chắc chắn</p>
                    </div>
                    <div class="icon-box wow fadeInUp" data-wow-duration="1500ms">
                        <div class="icon" style="background-color: #003789;"><span
                                class="flaticon-industrial-robot"></span>
                        </div>
                        <p class="icon-box-title">Chất lượng</p>
                    </div>
                </div>
            </div>
        </div>
    </section>
</template>

<style scoped>
.sec-title .sub-title {
    color: #003789;
    border: 3px solid #003789;
}

.sec-title .sub-title {
    position: relative;
    font-size: 16px;
    font-weight: 700;
    margin-bottom: 10px;
    display: inline-block;
    padding: 2.5px 17px;
}

.sec-title h2 {
    position: relative;
    font-size: 60px;
    font-weight: 900;
    text-transform: uppercase;
    line-height: 70px;
    letter-spacing: -1px;
}

.testimonials-section .testimonial-thumbs {
    max-width: 305px;
    cursor: pointer;
    margin-left: 0;
    margin-bottom: 40px;
    padding-right: 15px;
}

.testimonials-section .author-thumb {
    position: relative;
    cursor: pointer;
    padding: 10px;
    height: 100px;
    width: 100px;
    transition: .5s ease;
    border-radius: 50%;
    overflow: hidden;
    cursor: pointer;
}

.testimonials-section .author-thumb:before {
    position: absolute;
    content: '';
    border: 10px solid transparent;
    height: 100px;
    width: 100px;
    left: 0px;
    top: 0px;
    transition: .9s ease;
    border-radius: 50%;
}

.testimonials-section .swiper-slide-active .author-thumb:before {
    border-color: #003789;
}

.testimonials-section .author-thumb img {
    border-radius: 50%;
}

.testimonial-block .text {
    font-size: 20px;
    font-weight: 600;
    font-style: italic;
    line-height: 40px;
    margin-bottom: 30px;
    color: #181614;
    max-width: 700px;
}

.testimonial-block .author-title {
    font-size: 20px;
    font-weight: 700;
    text-transform: uppercase;
    color: #000;
}

.testimonial-block .author-title span {
    color: #ce9233;
    font-weight: 700;
}

.testimonials-section .icon-box {
    position: relative;
    background-color: #fff;
    margin-bottom: 10px;
    padding: 40px 0;
    display: flex;
    flex-wrap: wrap;
    align-items: center;
}

.testimonials-section .icon-box .icon {
    text-align: center;
    width: 80px;
    height: 80px;
    line-height: 80px;
    font-size: 40px;
    color: #fff;
    margin-right: 20px;
}

.testimonials-section .icon-box-title {
    position: relative;
    font-size: 20px;
    font-weight: 600;
    line-height: 30px;
}
</style>
