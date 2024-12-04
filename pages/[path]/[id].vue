<script setup>
import ProductRelated from '@/components/product/ProductRelated.vue';
import PostRelated from '@/components/Post/PostRelated.vue';
import TheBreadcrumb from '@/components/Breadcrumb/TheBreadcrumb.vue';
import ModuleLayout from '@/components/module/ModuleLayout.vue';
import { ref, watch } from 'vue'
import { Swiper, SwiperSlide } from 'swiper/vue';
import { FreeMode, Navigation, Thumbs } from 'swiper/modules';
import { useRoute, useRouter } from 'vue-router';
import isMobile from 'is-mobile';

import 'swiper/css';
import 'swiper/css/free-mode';
import 'swiper/css/navigation';
import 'swiper/css/thumbs';

const router = useRouter()
const route = useRoute()
const thumbsSwiper = ref(null);
const apiURL = import.meta.env.VITE_API_URL;
const url = import.meta.env.VITE_URL;
const attributeIndex = ref(0);

const data = await $fetch(`${apiURL}/${route.params.path}-pr${route.params.id}?view=${!isMobile() ? 'desktop' : 'mobile'}`)
const productChild = ref(data.product.childProducts[0])

useSeoMeta({
    title: data.product.product_name,
    ogTitle: data.config_custom.meta_title ? data.config_custom.meta_title : data.product.product_name,
    description: data.config_custom.meta_description ? data.config_custom.meta_description : data.product.product_name,
    ogDescription: data.config_custom.meta_description ? data.config_custom.meta_description : data.product.product_name,
    ogImage: data.product.product_avatar,
    twitterCard: 'summary_large_image',
})

useHead({
    script: [{
        type: 'application/ld+json',
        innerHTML:
        {
            "@context": "https://schema.org/",
            "@type": "Product",
            "@id": "https://schemaapp.com/highlighter/#Product",
            "name": data.product.product_name,
            "image": data.product.product_avatar,
            "description": data.product.product_name,
            "price": data.product.childProducts[0].product_price_sale ? data.product.childProducts[0].product_price_sale : data.product.childProducts[0].product_price,
            "brand": {
                "@type": "Organization",
                "@id": "https://schemaapp.com/#Organization",
                "name": data.product.product_sku,
            }
        }
    }],
})

useHead({
    script: [{
        type: 'application/ld+json',
        innerHTML:
        {
            "@context": "https://schema.org",
            "@type": "BreadcrumbList",
            "itemListElement":
                [
                    {
                        "@type": "ListItem",
                        "position": 1,
                        "item":
                        {
                            "@id": url,
                            "name": "Trang chủ"
                        }
                    },
                    {
                        "@type": "ListItem",
                        "position": 2,
                        "item":
                        {
                            "@id": url + route.fullPath,
                            "name": data.product.product_name
                        }
                    }
                ]
        }
    }],
})


const setThumbsSwiper = (swiper) => {
    thumbsSwiper.value = swiper;
};


const handleDesc = () => {
    document.getElementById("home").style.display = "block";
    document.getElementById("profile").style.display = "none";

    document.getElementById("home").classList.add("show");
    document.getElementById("profile").classList.remove("show");

    document.getElementById("home-tab").classList.add("active");
    document.getElementById("profile-tab").classList.remove("active");
}

const handleSpec = () => {
    document.getElementById("home").style.display = "none";
    document.getElementById("profile").style.display = "block";

    document.getElementById("home").classList.remove("show");
    document.getElementById("profile").classList.add("show");

    document.getElementById("profile-tab").classList.add("active");
    document.getElementById("home-tab").classList.remove("active");
}

const handleQuantity = (name) => {
    if (name === "minus") {
        if (Number(document.querySelector('.form-product-quantity').value) > 1) {
            document.querySelector('.form-product-quantity').value = Number(document.querySelector('.form-product-quantity').value) - 1
        }
    } else {
        document.querySelector('.form-product-quantity').value = Number(document.querySelector('.form-product-quantity').value) + 1

    }
}

const handleAddToCart = (productId) => {
    let cart = JSON.parse(localStorage.getItem('cart')) || []
    let res = cart.find((item) => item.id === productId)
    if (cart.length === 0) {
        cart.push({ id: productId, quantity: Number(document.querySelector('.form-product-quantity').value) || 1 });
    }
    else {
        if (res === undefined) {
            cart.push({ id: productId, quantity: Number(document.querySelector('.form-product-quantity').value) || 1 });
        } else {
            for (let cartItem of cart) {
                if (cartItem.id === productId) {
                    cartItem.quantity += Number(document.querySelector('.form-product-quantity').value) || 1
                }
            }
        }
    }
    localStorage.setItem('cart', JSON.stringify(cart))
    document.getElementById('header-cart-btn').setAttribute('data-cart-items', JSON.parse(localStorage.getItem('cart')).length)
    document.querySelector('.cart-alert').style.display = "block"

    setTimeout(function () {
        document.querySelector('.cart-alert').style.display = "none"
    }, 2500);

    window.scrollTo({ top: 0, behavior: 'smooth' });
}

const handleBuyNow = (productId) => {
    handleAddToCart(productId);
    router.push({
        name: 'cart',
    });
}

const handleAttribute = (e, index) => {
    attributeIndex.value = index
    data.product.childProducts.forEach((item) => {
        if (item.product_child_id === Object.values(data.attribute)[index][0]) {
            productChild.value = item
        }
    });

    // remove and add active
    for (let i = 0; i < document.querySelectorAll('.item_attribute1').length; i++) {
        document.querySelectorAll('.item_attribute1')[i].classList.remove('active');
    }

    for (let i = 0; i < document.querySelectorAll('.item_attribute2').length; i++) {
        document.querySelectorAll('.item_attribute2')[i].classList.remove('active');
    }
    e.target.classList.add('active')

    document.querySelector('.item_attribute2') !== null && document.querySelectorAll('.item_attribute2')[0].classList.add('active');
}

const handleAttribute2 = (e, id) => {
    data.product.childProducts.forEach((item) => {
        if (item.product_child_id === id) {
            productChild.value = item
        }
    });

    for (let i = 0; i < document.querySelectorAll('.item_attribute2').length; i++) {
        document.querySelectorAll('.item_attribute2')[i].classList.remove('active');
    }
    e.target.classList.add('active')
}

</script>

<template>
    <div>
        <TheBreadcrumb :data="data.categories" :currentName="data.product.product_name" />

        <section style="padding: 15px 0 !important">
            <div class="container">
                <div class="row">
                    <div class="col-lg-5 col-md-6">
                        <swiper :style="{
                            '--swiper-navigation-color': '#fff',
                            '--swiper-pagination-color': '#fff',
                        }" :loop="true" :spaceBetween="10" :navigation="true" :thumbs="{ swiper: thumbsSwiper }"
                            :modules="[FreeMode, Navigation, Thumbs]" class="mySwiper2">
                            <swiper-slide v-for="(image, index) in JSON.parse(data.product.product_image)" :key="index">
                                <img :src="image" />
                            </swiper-slide>
                        </swiper>

                        <swiper @swiper="setThumbsSwiper" :loop="true" :spaceBetween="10" :slidesPerView="4"
                            :freeMode="true" :watchSlidesProgress="true" :modules="modules" class="mySwiper">
                            <swiper-slide v-for="(image, index) in JSON.parse(data.product.product_image)" :key="index">
                                <img :src="image" />
                            </swiper-slide>
                        </swiper>
                    </div>

                    <div class="col-lg-7 col-md-6 mt-5 mt-md-0 ps-lg-5">
                        <div class="product-details">
                            <h3>{{ data.product.product_name }}</h3>
                            <div class="mt-3">
                                <i class="fa-solid fa-cart-shopping" style="color: #003789;"></i>
                                <span>Đã bán: {{ data.product.product_sold ? data.product.product_sold : "1" }}</span>
                                <span style="margin: 0 5px;"> | </span>
                                <i class="fa-solid fa-eye" style="color: #003789;"></i>
                                <span>Lượt xem: {{ data.product.product_view ? data.product.product_view : "1" }}</span>
                                <span style="margin: 0 5px;"> | </span>
                                <i class="fa-solid fa-barcode" style="color: #003789;"></i>
                                <span>Mã sản phẩm: </span><span>{{ data.product.product_sku ? data.product.product_sku :
                                    "Không có"
                                    }}</span>
                            </div>


                            <div class="product-price my-4">
                                <span class="product-price text-dark" style="font-size: 21px;"
                                    v-if="productChild.product_price_sale">
                                    <del class="mr-2">
                                        {{ new Intl.NumberFormat('vi-VN', {
                                            currency: 'VND'
                                        }).format(productChild.product_price) }}
                                        đ</del>

                                    <span style="color:#003789 ; font-size: 28px;">
                                        {{ new Intl.NumberFormat('vi-VN', {
                                            currency: 'VND'
                                        }).format(productChild.product_price_sale) }} đ </span>
                                </span>
                            </div>

                            <div class="mb-4" v-if="productChild.attribute_name1">
                                <div class="attribute-child d-flex align-items-center">
                                    <label class="attribute-name text-bold mr-2">{{
                                        productChild.attribute_name1 }}:
                                    </label>
                                    <div class="list-attribute">
                                        <div class="item hover-bg item-attribute1 item_attribute1"
                                            :class="index === 0 && 'active'"
                                            v-for="(attr, index) in Object.keys(data.attribute)" :key="index"
                                            @click="(e) => handleAttribute(e, index)">

                                            {{ attr }}
                                        </div>

                                    </div>
                                </div>

                                <div class="attribute-child d-flex align-items-center mt-3"
                                    v-if="productChild.attribute_name2">
                                    <label class="attribute-name text-bold mr-2">{{
                                        productChild.attribute_name2 }}:
                                    </label>
                                    <div class="list-attribute">
                                        <div class="item hover-bg item_attribute2" :class="index === 0 && 'active'"
                                            v-for="(attr, index) in Object.values(data.attribute)[attributeIndex]"
                                            :key="index" @click="(e) => handleAttribute2(e, attr)">
                                            {{ attr }}
                                        </div>
                                    </div>
                                </div>
                            </div>

                            <div v-html="data.product.product_brief"></div>

                            <div class="row my-4">
                                <div class="d-flex align-items-center">
                                    <h6 class="mb-0 mr-3">Số lượng:</h6>
                                    <div class="d-flex align-items-center">
                                        <button class="btn-product btn-product-up" @click="handleQuantity('minus')">
                                            <i class="las la-minus"></i>
                                        </button>
                                        <input class="form-product form-product-quantity" type="number"
                                            name="form-product" value="1" min="1">
                                        <button class="btn-product btn-product-down" @click="handleQuantity('plus')">
                                            <i class="las la-plus"></i>
                                        </button>
                                    </div>
                                </div>
                                <div class="product-link d-flex align-items-center mt-5">
                                    <button class="primary-btn mr-3" type="button"
                                        @click="handleAddToCart(productChild.product_child_id)">
                                        <span>Thêm vào giỏ hàng</span>
                                    </button>
                                    <button class="primary-btn" type="button"
                                        @click="handleBuyNow(productChild.product_child_id)">
                                        <span>Mua ngay</span>
                                    </button>
                                </div>
                            </div>
                        </div>

                        <div class="social__sharing mt-6">
                            <ul class="social social-round"
                                style="display: flex; gap:5px; overflow: hidden; list-style: none; padding: 0;">
                                <li class="social-item">
                                    <iframe
                                        :src="`https://www.facebook.com/plugins/share_button.php?href=${url + route.fullPath}&amp;layout=button_count&amp;size=small&amp;appId=242142420396341&amp;width=119&amp;height=20`"
                                        width="90" height="20" style="border:none;overflow:hidden"
                                        allow="encrypted-media"></iframe>
                                </li>
                                <li class="social-item">
                                    <div class="zalo-share-button" :data-href="url + $route.fullPath"
                                        data-oaid="579745863508352884" data-layout="1" data-color="blue"
                                        data-customize="false"
                                        style="position: relative; display: inline-block; width: 70px; height: 20px;">
                                        <iframe id="887c8dea-edf5-4600-9946-45dcd4547db9"
                                            name="887c8dea-edf5-4600-9946-45dcd4547db9" frameborder="0"
                                            allowfullscreen="" scrolling="no" width="70px" height="20px"
                                            :src="`https://button-share.zalo.me/share_inline?id=887c8dea-edf5-4600-9946-45dcd4547db9&amp;layout=1&amp;color=blue&amp;customize=false&amp;width=70&amp;height=20&amp;isDesktop=true&amp;url=${url + route.fullPath}&amp;d=eyJ1cmwiOiJodHRwczovL3Z1YW5oaG91c2Uudm4vY2FtZXJhLWlwLWltb3UtYTIteG9heS0zNjAtZG8tZ2lhbS1zYXQtYW4tdG9hbi1jaG8tZ2lhLWRpbmgtcHIyMyJ9&amp;shareType=0`"
                                            style="position: absolute; z-index: 99; top: 0px; left: 0px;"></iframe>
                                    </div>
                                </li>
                            </ul>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!--tab start-->
        <section>
            <div class="container">
                <div class="row">
                    <div class="col-md-12">
                        <div class="tab">
                            <ul class="nav nav-tabs" id="myTab" role="tablist"
                                style="justify-content: center;border: unset;">
                                <li class="nav-item" role="presentation">
                                    <button class="nav-link active" id="home-tab" data-toggle="tab" data-target="#home"
                                        type="button" role="tab" aria-controls="home" aria-selected="true"
                                        @click="handleDesc">
                                        Mô tả</button>
                                </li>
                                <li class="nav-item" role="presentation">
                                    <button class="nav-link" id="profile-tab" data-toggle="tab" data-target="#profile"
                                        type="button" role="tab" aria-controls="profile" aria-selected="false"
                                        @click="handleSpec">
                                        Đặc điểm kỹ thuật</button>
                                </li>
                            </ul>

                            <div class="tab-content" id="myTabContent">
                                <div class="tab-pane fade show active" id="home" role="tabpanel"
                                    aria-labelledby="home-tab" v-html="data.product.product_content"
                                    v-if="data.product.product_content"></div>

                                <div class="tab-pane fade show active" id="home" role="tabpanel"
                                    aria-labelledby="home-tab" v-else>
                                    Chưa có mô tả. Vui lòng liên hệ để biết chi tiết sản phẩm.</div>

                                <div class="tab-pane fade" id="profile" role="tabpanel" aria-labelledby="profile-tab"
                                    v-html="data.product.product_specification">
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <ProductRelated :related_product="data.related_product" v-if="data.related_product.length > 0" />
                <!-- End Product Related -->

                <!-- Post Related -->
                <PostRelated :related_post="data.related_post" v-if="data.related_post.length > 0" />
                <!-- End Post Related -->
                <ModuleLayout :modules="data?.modules" />

            </div>

        </section>
        <!--tab end-->
        <div>
            <!-- Product Related -->
            <ProductRelated :related_product="data.related_product" v-if="data.related_product.length > 0" />
            <!-- End Product Related -->

            <!-- Post Related -->
            <PostRelated :related_post="data.related_post" v-if="data.related_post.length > 0" />
            <!-- End Post Related -->
            <ModuleLayout :modules="data?.modules" />
        </div>
    </div>
</template>

<style scoped>
.row {
    margin-right: unset !important;
    margin-left: unset !important;
}

.swiper {
    width: 100%;
    height: 100%;
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

.swiper {
    width: 100%;
    height: 300px;
    margin-left: auto;
    margin-right: auto;
}

.swiper-slide {
    background-size: cover;
    background-position: center;
}

.mySwiper2 {
    height: 80%;
    width: 100%;
}

.mySwiper {
    height: 20%;
    box-sizing: border-box;
    padding: 10px 0;
}

.mySwiper .swiper-slide {
    width: 25%;
    height: 100%;
    opacity: 0.4;
}

.mySwiper .swiper-slide-thumb-active {
    opacity: 1;
}

.swiper-slide img {
    display: block;
    width: 100%;
    height: 100%;
    object-fit: cover;
}

h3 {
    font-size: 30px;
    font-style: normal;
    line-height: 40px;
    font-weight: 600;
}

.list-attribute {
    display: flex;
    flex-wrap: wrap;
    flex: auto;
    gap: 5px;
    width: auto;
}

.list-attribute .item.active {
    background-color: #003789 !important;
    color: #fff;
}

.list-attribute .item {
    float: left;
    padding: .5rem;
    display: flex;
    justify-content: center;
    border: 1px solid #003789 !important;
    margin: 0 5px;
    border-radius: 5px;
    width: 40%;
    cursor: pointer;
}

.btn-product {
    background: none;
    color: #12181c;
    border: 1px solid #e1e1e1;
    height: 40px;
    width: auto;
    padding: 0 10px;
    font-size: 13px;
    cursor: pointer;
}

input.form-product {
    height: 40px !important;
    border: none;
    background: #ffffff;
    text-align: center;
    width: 150px;
    border-top: 1px solid #e1e1e1 !important;
    border-bottom: 1px solid #e1e1e1 !important;
    color: #12181c;
    vertical-align: middle;
}

.primary-btn,
.white-btn {
    position: relative;
    transition: all 0.6s;
    background: none;
    padding: 13px 35px;
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

.primary-btn span {
    transition: all 0.6s;
    z-index: 9;
    position: relative;
    color: #ffffff;
    white-space: nowrap;
}

.primary-btn:hover::before {
    opacity: 1;
    transform: scale(1, 1);
}

.primary-btn:hover::after {
    transform: scale(1, .1);
    opacity: 0;
}

.nav-tabs .nav-item.show .nav-link,
.nav-tabs .nav-link.active {
    border-color: #e1e1e1 !important;
    border-bottom-color: #003789 !important;
    color: #003789 !important;
}

.nav-link {
    font-weight: 600;
}

.tab-content {
    border: 1px solid #eaedf4;
    padding: 18px;
    border-radius: 8px;
    line-height: 1.8;
}

.tab-content:deep(img) {
    max-width: 100% !important;
    height: auto !important;
}
</style>
