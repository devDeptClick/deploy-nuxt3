<script setup>
import ThePaginate from '../Paginate/ThePaginate.vue';
import { useRoute } from 'vue-router'

const router = useRouter()
defineProps({
    categories: Object
})

const truncateString = (str, num) => {
    if (str.length <= num) {
        return str
    }
    return str.slice(0, num) + '...'
}

const handleFilter = (e) => {
    e.preventDefault();
    router.push({
        query: {
            price_from: e.target.price_from.value,
            price_to: e.target.price_to.value,
            sorted_price: e.target.sorted_price.value,
            item_per_page: e.target.item_per_page.value,
            sorted_by: e.target.sorted_by.value,
            page: 1
        }
    })

}
</script>

<template>
    <section>
        <div class="container">
            <div class="row mb-5">
                <div class="col">
                    <form action="" @submit="handleFilter">
                        <div style="display: flex; gap:5px;justify-content: end;">
                            <div class="form-group">
                                <input type="number" class="form-control form-filter" min="0" placeholder="Giá từ"
                                    name="price_from" style="max-width: 150px;">
                            </div>

                            <div class="form-group">
                                <input type="number" class="form-control form-filter" min="0" placeholder="Giá đến"
                                    name="price_to" style="max-width: 150px;">
                            </div>

                            <div class="form-group">
                                <button type="submit" class="btn btn-primary"
                                    style="background-color: #003789 !important; border: unset;">
                                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"
                                        style="width: 15px;fill:#ffff;">
                                        <path
                                            d="M505 442.7L405.3 343c-4.5-4.5-10.6-7-17-7H372c27.6-35.3 44-79.7 44-128C416 93.1 322.9 0 208 0S0 93.1 0 208s93.1 208 208 208c48.3 0 92.7-16.4 128-44v16.3c0 6.4 2.5 12.5 7 17l99.7 99.7c9.4 9.4 24.6 9.4 33.9 0l28.3-28.3c9.4-9.4 9.4-24.6 .1-34zM208 336c-70.7 0-128-57.2-128-128 0-70.7 57.2-128 128-128 70.7 0 128 57.2 128 128 0 70.7-57.2 128-128 128z" />
                                    </svg>
                                </button>
                            </div>

                            <div class="form-group">
                                <select class="form-control form-filter" name="sorted_price">
                                    <option selected value="">Xếp theo giá</option>
                                    <option value="low2high" :selected="$route.query.sorted_price === 'low2high'">
                                        Thấp đến Cao</option>

                                    <option value="high2low" :selected="$route.query.sorted_price === 'high2low'">
                                        Cao đến thấp</option>
                                </select>
                            </div>

                            <div class="form-group">
                                <select class="form-control form-filter" style="padding-inline: 5px 30px;"
                                    name="item_per_page">
                                    <option selected value="">Hiển thị</option>
                                    <option value="20" :selected="$route.query.item_per_page === '20'">20</option>
                                    <option value="50" :selected="$route.query.item_per_page === '50'">50</option>
                                    <option value="100" :selected="$route.query.item_per_page === '100'">100</option>
                                </select>
                            </div>

                            <div class="form-group">
                                <select class="form-control form-filter" style="padding-inline: 5px 30px;"
                                    name="sorted_by">
                                    <option selected value="">Trạng thái</option>
                                    <option value="newest" :selected="$route.query.sorted_by === 'newest'">Mới nhất
                                    </option>
                                    <option value="oldest" :selected="$route.query.sorted_by === 'oldest'">Cũ nhất
                                    </option>
                                </select>
                            </div>
                        </div>
                    </form>
                </div>
            </div>

            <div class="row">
                <div class="col-lg-12 col-md-12 order-lg-1">
                    <div class="row text-center" style="gap: 20px 0;">
                        <div class="col-lg-3 col-md-6" v-for="(product, index) in categories.data.data" :key="index">
                            <div class="product-item box-shadow p-4" style="height:100%;">
                                <div class="label"
                                    style="position:absolute; width: 60px; height:25px; top:30px;left:40px; background-image: url(../images/pentair-label.png);background-repeat: no-repeat;">
                                    <span
                                        style="position: absolute;top: -2px; left: 15px; color: #ffff;font-size: 12px; font-weight: 600;">New</span>
                                </div>
                                <div class="product-img">
                                    <RouterLink :to="'/' + product.product_slug + '/' + product.product_id">
                                        <img class="img-fluid rounded" :src="product.product_avatar"
                                            :alt="product.product_name">
                                    </RouterLink>

                                </div>
                                <div class="product-desc">
                                    <RouterLink :to="'/' + product.product_slug + '/' + product.product_id"
                                        class="product-name mt-4 mb-2 d-block link-title h6">
                                        {{ truncateString(product.product_name, 39) }}
                                    </RouterLink>

                                    <div>
                                        <span class="product-price" style="color: #ee594c;"
                                            v-if="product.childProducts.length === 1 && product.childProducts[0].product_price_sale">
                                            <del style="margin-right: 5px;" class="text-dark">{{ new
                                                Intl.NumberFormat('vi-VN', {
                                                    currency: 'VND'
                                                }).format(product.childProducts[0].product_price) }} đ</del>
                                            {{ new Intl.NumberFormat('vi-VN', {
                                                currency: 'VND'
                                            }).format(product.childProducts[0].product_price_sale) }} đ
                                        </span>

                                        <span class="product-price" style="color: #ee594c;"
                                            v-else-if="product.childProducts.length === 1 && !product.childProducts[0].product_price_sale">
                                            {{ new Intl.NumberFormat('vi-VN', {
                                                currency: 'VND'
                                            }).format(product.childProducts[0].product_price_sale) }} đ
                                        </span>

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
                                    </div>

                                    <div class="product-link mt-3">
                                        <RouterLink :to="'/' + product.product_slug + '/' + product.product_id"
                                            class="add-cart" style="font-weight: 600;">
                                            <i class="las la-shopping-bag me-2"></i>Thêm Vào Giỏ Hàng
                                        </RouterLink>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>

                    <div>
                        <ThePaginate :links="categories.data.links.slice(1, -1)" />
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
