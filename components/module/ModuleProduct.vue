<script setup>
import { RouterLink } from 'vue-router'

defineProps({
    module: Object,
})

const truncateString = (str, num) => {
    if (str.length <= num) {
        return str
    }
    return str.slice(0, num) + '...'
}
</script>

<template>
    <section>
        <div class="container">
            <h4 class="mb-4 fw-bold">{{ module.module_name_second ? module.module_name_second : module.module_title }}
            </h4>
            <div class="row">
                <div class="col-lg-12 col-md-12 order-lg-1">
                    <div class="row">
                        <div class="col-lg-3 d-md-none d-lg-block">
                            <img :src="module.module_avatar" class="rounded"
                                :alt="module.module_name_second ? module.module_name_second : module.module_title"
                                style="width: 100%; height: 100%;">
                        </div>

                        <div class="col-12 col-lg-9">
                            <div class="row text-center" style="gap: 20px 0;">
                                <div class="col-lg-4 col-md-6" v-for="(product, index) in module.product" :key="index">
                                    <div class="product-item box-shadow p-4" style="height:100%;">
                                        <div class="product-img">
                                            <RouterLink :to="'/' + product.product_slug + '/' + product.product_id">
                                                <img class="img-fluid rounded"
                                                    :src="product.product_avatar ? product.product_avatar : JSON.parse(product.product_image)[0]"
                                                    :alt="product.product_name">
                                            </RouterLink>
                                        </div>
                                        <div class="product-desc">
                                            <RouterLink :to="'/' + product.product_slug + '/' + product.product_id"
                                                class="product-name mt-4 mb-2 d-block link-title h6">
                                                {{ truncateString(product.product_name, 42) }}
                                            </RouterLink>

                                            <span class="product-price" style="color: #ee594c;"
                                                v-if="product.childProducts.length === 1 && product.childProducts[0].product_price_sale">
                                                <del style="margin-right: 5px;" class="text-dark">{{ new
                                                    Intl.NumberFormat('vi-VN', {
                                                        currency: 'VND'
                                                    }).format(product.childProducts[0].product_price) }} đ</del>
                                                {{ new Intl.NumberFormat('vi-VN', {
                                                    currency: 'VND'
                                                }).format(product.childProducts[0].product_price_sale) }} đ </span>

                                            <span class="product-price" style="color: #ee594c;"
                                                v-else-if="product.childProducts.length === 1 && !product.childProducts[0].product_price_sale">
                                                {{ new Intl.NumberFormat('vi-VN', {
                                                    currency: 'VND'
                                                }).format(product.childProducts[0].product_price_sale) }} đ </span>

                                            <div v-else-if="product.childProducts.length > 1">
                                                <span style="color: #ee594c;">
                                                    {{ new Intl.NumberFormat('vi-VN', {
                                                        currency:
                                                            'VND'
                                                    }).format(product.product_price_min)
                                                    }} đ</span>

                                                <span style="color: #ee594c;"> - </span>
                                                <span class="product-price" style="color: #ee594c;">
                                                    {{ new Intl.NumberFormat('vi-VN', {
                                                        currency: 'VND'
                                                    }).format(product.product_price_max)
                                                    }} đ</span>
                                            </div>

                                            <div class="product-link mt-3">
                                                <RouterLink :to="'/' + product.product_slug + '/' + product.product_id"
                                                    class="add-cart" style="font-weight: 600;">
                                                    <i class="las la-shopping-bag mr-2"></i>Thêm Giỏ Hàng
                                                </RouterLink>
                                            </div>
                                        </div>
                                    </div>
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
.img-fluid:hover {
    transform: scale(1.1);
}
</style>
