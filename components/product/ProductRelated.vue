<script setup>
defineProps({
    related_product: Object
})
</script>

<template>
    <section>
        <div class="container">
            <div class="mt-5">
                <h5 class="font-weight-bold">Sản phẩm liên quan</h5>
                <div class="row">
                    <div class="col-lg-12 col-md-12 order-lg-1">
                        <div class="row text-center" style="gap: 20px 0px;">

                            <div class="col-lg-3 col-md-6" v-for="(productRelated, index) in related_product"
                                :key="index">
                                <div class="product-item box-shadow p-3" style="height:100%;">
                                    <div class="product-img">
                                        <NuxtLink
                                            :to="'/' + productRelated.product_slug + '/' + productRelated.product_id">
                                            <img class="img-fluid" :src="productRelated.product_avatar"
                                                :alt="productRelated.product_name">
                                        </NuxtLink>
                                    </div>
                                    <div class="product-desc">
                                        <NuxtLink
                                            :to="'/' + productRelated.product_slug + '/' + productRelated.product_id"
                                            class="product-name mt-4 mb-2 d-block link-title h6">
                                            {{ productRelated.product_name }}
                                        </NuxtLink>
                                        <div>
                                            <span class="product-price" style="color: #ee594c;"
                                                v-if="productRelated.childProducts.length === 1 && productRelated.childProducts[0].product_price_sale">
                                                <del style="margin-right: 5px;" class="text-dark">{{ new
                                                    Intl.NumberFormat('vi-VN', {
                                                        currency: 'VND'
                                                    }).format(productRelated.childProducts[0].product_price) }} đ</del>
                                                {{ new Intl.NumberFormat('vi-VN', {
                                                    currency: 'VND'
                                                }).format(productRelated.childProducts[0].product_price_sale) }} đ
                                            </span>

                                            <span class="product-price" style="color: #ee594c;"
                                                v-else-if="productRelated.childProducts.length === 1 && !productRelated.childProducts[0].product_price_sale">
                                                {{ new Intl.NumberFormat('vi-VN', {
                                                    currency: 'VND'
                                                }).format(productRelated.childProducts[0].product_price_sale) }} đ
                                            </span>

                                            <div v-else-if="productRelated.childProducts.length > 1">
                                                <span style="color: #ee594c;">
                                                    {{ new Intl.NumberFormat('vi-VN', {
                                                        currency:
                                                            'VND'
                                                    }).format(productRelated.product_price_min)
                                                    }} đ</span>

                                                <span style="color: #ee594c;"> - </span>
                                                <span class="product-price" style="color: #ee594c;">
                                                    {{ new Intl.NumberFormat('vi-VN', {
                                                        currency: 'VND'
                                                    }).format(productRelated.product_price_max)
                                                    }} đ</span>
                                            </div>

                                            <div class="product-link mt-3">
                                                <span class="add-cart" style="font-weight: 600;">
                                                    <i class="las la-shopping-bag me-2"></i>Thêm Vào Giỏ Hàng </span>
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
section {
    padding: 0 !important;
}
</style>
