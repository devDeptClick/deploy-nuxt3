<script setup>
import { onMounted, ref } from "vue";
import TheBreadcrumb from "@/components/Breadcrumb/TheBreadcrumb.vue";
const router = useRouter()

const apiURL = import.meta.env.VITE_API_URL;
const provinces = await $fetch(`${apiURL}/province`)
const districts = ref([]);
const wards = ref([]);
const cart = ref([]);
const total = ref(0);

const checkoutData = ref({
    order_customer: '',
    order_phone: '',
    order_email: '',
    order_address: '',
    order_note: '',
    order_province: '',
    order_district: '',
    order_ward: '',
    order_country: 'Việt Nam',
    order_content: '',
    province_id: '',
    district_id: '',
    ward_id: '',
    payment_method: 'cod',
});

onMounted(async () => {
    await fetchCart();
})

const fetchCart = async () => {
    const product = JSON.parse(localStorage.getItem('cart')) || [];

    const response = await $fetch(`${apiURL}/gio-hang`, {
        method: "post",
        body: { product: product },
        async onRequest() {
            document.getElementById("ht-preloader").style.display = "flex";
        },
        async onResponse() {
            document.getElementById("ht-preloader").style.display = "none";
        }
    }
    )
    cart.value = response.data
    total.value = response.total
}

const handleQuantity = (name, productId) => {
    if (name === "minus") {
        if (Number(document.querySelector(`#form-product-quantity-${productId}`).value) > 1) {
            document.querySelector(`#form-product-quantity-${productId}`).value = Number(document.querySelector(`#form-product-quantity-${productId}`).value) - 1
        }
    } else {
        document.querySelector(`#form-product-quantity-${productId}`).value = Number(document.querySelector(`#form-product-quantity-${productId}`).value) + 1
    }

    let cart = JSON.parse(localStorage.getItem('cart')) || []
    let res = cart.find((item) => item.id === productId)
    if (cart.length === 0) {
        cart.push({ id: productId, quantity: Number(document.querySelector(`#form-product-quantity-${productId}`).value) || 1 });
    }
    else {
        if (res === undefined) {
            cart.push({ id: productId, quantity: Number(document.querySelector(`#form-product-quantity-${productId}`).value) || 1 });
        } else {
            for (let cartItem of cart) {
                if (cartItem.id === productId) {
                    cartItem.quantity = Number(document.querySelector(`#form-product-quantity-${productId}`).value) || 1
                }
            }
        }
    }
    localStorage.setItem('cart', JSON.stringify(cart))
    fetchCart();
}


const handleRemoveCartItem = (productId) => {
    let cart = JSON.parse(localStorage.getItem('cart')) || []
    let res = cart.find((item) => item.id === productId)

    if (res !== undefined) {
        cart = cart.filter((item) => item.id !== productId)
    }
    localStorage.setItem('cart', JSON.stringify(cart))
    fetchCart();
    document.querySelector('#header-cart-btn').setAttribute('data-cart-items', JSON.parse(cart.length));

}

const handleRemoveAll = () => {
    localStorage.setItem('cart', JSON.stringify([]))
    fetchCart();
    document.querySelector('#header-cart-btn').setAttribute('data-cart-items', 0);
}

const handleProvinceChange = async (e) => {
    if (e.target.value) {
        const provinceId = parseInt(e.target.value);
        const response = await $fetch(`${apiURL}/district?province_id=${provinceId}`)
        districts.value = response.districts
    }
}

const handleDistrictChange = async (e) => {
    if (e.target.value) {
        const districtId = parseInt(e.target.value);
        const response = await $fetch(`${apiURL}/ward?district_id=${districtId}`)
        wards.value = response.wards
    }
}
const handleCheckout = async () => {
    checkoutData.value.order_content = cart.value;
    checkoutData.value.order_province = document.getElementById("provinces").options[document.getElementById("provinces").selectedIndex].text;
    checkoutData.value.order_district = document.getElementById("districts").options[document.getElementById("districts").selectedIndex].text;
    checkoutData.value.order_ward = document.getElementById("wards").options[document.getElementById("wards").selectedIndex].text;

    if (!checkoutData.value.order_customer || !checkoutData.value.order_phone
        || !checkoutData.value.order_address || !checkoutData.value.order_email
        || !checkoutData.value.province_id || !checkoutData.value.district_id
        || !checkoutData.value.ward_id) {
        alert("Vui lòng nhập đầy đủ thông tin đặt hàng")
        return;
    }

    await $fetch(`${apiURL}/checkout`, {
        method: "post",
        body: checkoutData.value,
        async onRequest() {
            document.getElementById("ht-preloader").style.display = "flex";
        },
        async onResponse({ response }) {
            document.getElementById("ht-preloader").style.display = "none";
            localStorage.setItem('cart', JSON.stringify([]))
            document.querySelector('#header-cart-btn').setAttribute('data-cart-items', 0);
            router.push({
                path: 'xac-thuc-dat-hang',
                query: {
                    order_id: response._data.order_id
                }
            });
        }
    })
}
</script>

<template>
    <div>
        <TheBreadcrumb :data="[]" :currentName="'Giỏ hàng'" />

        <section v-if="cart.length > 0">
            <div class="container">
                <div class="row">
                    <div class="col p-0">
                        <div class="table-responsive border-bottom-0 p-5 box-shadow">
                            <table class="cart-table table text-center mb-0 table-bordered">
                                <thead>
                                    <tr>
                                        <th class="h5 mb-0 py-3 font-w-6" scope="col">Sản phẩm</th>
                                        <th class="h5 mb-0 py-3 font-w-6" scope="col">Giá</th>
                                        <th class="h5 mb-0 py-3 font-w-6" scope="col">Số lượng</th>
                                        <th class="h5 mb-0 py-3 font-w-6" scope="col">Tổng</th>
                                        <th class="h5 mb-0 py-3 font-w-6" scope="col">Xóa</th>
                                    </tr>
                                </thead>
                                <tbody class="border-top-0">
                                    <tr v-for="(product, index) in cart" :key="index">
                                        <td>
                                            <div class="d-lg-flex align-items-center">
                                                <NuxtLink :to="product.product_url">
                                                    <img class="img-fluid mr-2"
                                                        :src="JSON.parse(product.product_image)[0]"
                                                        :alt="product.product_name"
                                                        style="border-radius: 8px; width: 120px !important; height: 120px !important; max-width: unset !important;">
                                                </NuxtLink>

                                                <div class="text-start">
                                                    <NuxtLink class="product-name link-title h6"
                                                        :to="product.product_url">
                                                        {{ product.product_name }}
                                                    </NuxtLink>
                                                </div>
                                            </div>
                                        </td>
                                        <td>
                                            <h6 class="mb-0">
                                                {{ new Intl.NumberFormat('vi-VN', {
                                                    currency: 'VND'
                                                }).format(product.product_price_sale ? product.product_price_sale :
                                                    product.product_price) }}
                                            </h6>
                                        </td>
                                        <td>
                                            <div class="d-flex justify-content-center align-items-center">
                                                <button class="btn-product btn-product-up"
                                                    @click="handleQuantity('minus', product.product_child_id)">
                                                    <i class="las la-minus"></i>
                                                </button>

                                                <input class="form-product" type="number"
                                                    :id="'form-product-quantity-' + product.product_child_id"
                                                    name="form-product" :value="product.cart_quantity">

                                                <button class="btn-product btn-product-down"
                                                    @click="handleQuantity('plus', product.product_child_id)">
                                                    <i class="las la-plus"></i>
                                                </button>
                                            </div>
                                        </td>
                                        <td>
                                            <h6 class="mb-0">
                                                {{ new Intl.NumberFormat('vi-VN', {
                                                    currency: 'VND'
                                                }).format(Number(product.cart_quantity) *
                                                    Number(product.product_price_sale ? product.product_price_sale :
                                                        product.product_price))
                                                }}
                                            </h6>
                                        </td>
                                        <td class="border-right-0">
                                            <button type="submit" class="white-btn py-2 px-3 fs-3"
                                                @click="handleRemoveCartItem(product.product_child_id)">
                                                <i class="las la-times"></i>
                                            </button>
                                        </td>
                                    </tr>

                                </tbody>
                            </table>
                        </div>
                    </div>

                    <div style="width: 100%;">
                        <div class="p-3"
                            style="display: flex;flex-wrap: wrap; justify-content: space-between;gap:10px; border: 1px solid #ebebeb;background-color: #f8f8f8; ">
                            <div class="delete-cart" @click="handleRemoveAll">
                                <div
                                    style="border: 1px solid #003789; color: #003789; padding: 5px 15px; border-radius: 5px; cursor: pointer;">
                                    <svg xmlns="http://www.w3.org/2000/svg" width="25px" height="25px"
                                        viewBox="0 0 24 24" fill="none">
                                        <path d="M20.5001 6H3.5" stroke="#003789" stroke-width="1.5"
                                            stroke-linecap="round">
                                        </path>
                                        <path
                                            d="M18.8332 8.5L18.3732 15.3991C18.1962 18.054 18.1077 19.3815 17.2427 20.1907C16.3777 21 15.0473 21 12.3865 21H11.6132C8.95235 21 7.62195 21 6.75694 20.1907C5.89194 19.3815 5.80344 18.054 5.62644 15.3991L5.1665 8.5"
                                            stroke="#003789" stroke-width="1.5" stroke-linecap="round"></path>
                                        <path d="M9.5 11L10 16" stroke="#003789" stroke-width="1.5"
                                            stroke-linecap="round">
                                        </path>
                                        <path d="M14.5 11L14 16" stroke="#003789" stroke-width="1.5"
                                            stroke-linecap="round">
                                        </path>
                                        <path
                                            d="M6.5 6C6.55588 6 6.58382 6 6.60915 5.99936C7.43259 5.97849 8.15902 5.45491 8.43922 4.68032C8.44784 4.65649 8.45667 4.62999 8.47434 4.57697L8.57143 4.28571C8.65431 4.03708 8.69575 3.91276 8.75071 3.8072C8.97001 3.38607 9.37574 3.09364 9.84461 3.01877C9.96213 3 10.0932 3 10.3553 3H13.6447C13.9068 3 14.0379 3 14.1554 3.01877C14.6243 3.09364 15.03 3.38607 15.2493 3.8072C15.3043 3.91276 15.3457 4.03708 15.4286 4.28571L15.5257 4.57697C15.5433 4.62992 15.5522 4.65651 15.5608 4.68032C15.841 5.45491 16.5674 5.97849 17.3909 5.99936C17.4162 6 17.4441 6 17.5 6"
                                            stroke="#003789" stroke-width="1.5"></path>
                                    </svg>
                                    Xóa toàn bộ
                                </div>
                            </div>

                            <div style="display: flex;flex-wrap: wrap; gap: 15px;">
                                <!-- <div>
                            <strong style="font-weight: 600;">Tổng đơn</strong>
                            <div>
                                1.200.000 đ
                            </div>
                        </div>

                        <div>
                            <strong style="font-weight: 600;">Phí vận chuyển</strong>
                            <div>
                                15.000 đ
                            </div>
                        </div> -->

                                <div>
                                    <strong style="font-weight: 600;">Tổng thanh toán</strong>
                                    <div>
                                        {{ new Intl.NumberFormat('vi-VN', {
                                            currency: 'VND'
                                        }).format(total) }} đ
                                    </div>
                                </div>
                            </div>

                        </div>
                    </div>
                </div>
                <div class="row mt-5">
                    <div class="col-lg-7 p-0">
                        <div class="order-sm-1 p-4"
                            style="box-shadow: rgba(0, 0, 0, 0.05) 0px 6px 24px 0px, rgba(0, 0, 0, 0.08) 0px 0px 0px 1px;">
                            <div class="checkout-title">
                                <h4 class="text-dark mb-2">Thông tin người nhận</h4>
                            </div>
                            <div class="checkout-form">
                                <input type="hidden" name="order_type" id="order_type" value="1">
                                <div class="row">
                                    <div class="form-group col-md-12 col-sm-12">
                                        <label for="order_customer" class="form__label">Họ tên <span
                                                class="text-danger">(*)</span></label>
                                        <input type="text" id="order_customer" v-model="checkoutData.order_customer"
                                            class="isRequired form-control form-information"
                                            style="height: 40px; border: 1px solid grey; border-radius: 12px;padding: 10px;">
                                    </div>
                                </div>

                                <div class="parent-select mt-4">
                                    <label for="order_address" class="form__label mb-2">Địa chỉ <span
                                            class="text-danger">(*)</span></label>

                                    <div class="row">
                                        <div class="form-group col-md-4 col-sm-4">
                                            <select id="provinces" v-model="checkoutData.province_id"
                                                class="provinces form-control form-information isRequired"
                                                @change="(e) => handleProvinceChange(e)">
                                                <option value="">Tỉnh/thành phố</option>
                                                <option :value="province.id" v-for="(province, index) in provinces"
                                                    :key="index">{{ province.name }}</option>

                                            </select>
                                        </div>
                                        <div class="form-group col-md-4 col-sm-4">
                                            <select id="districts" v-model="checkoutData.district_id"
                                                @change="(e) => handleDistrictChange(e)"
                                                class="districts form-control form-information isRequired">
                                                <option value="">Chọn Quận/Huyện</option>
                                                <option :value="district.id" v-for="(district, index) in districts"
                                                    :key="index">{{ district.name }}</option>
                                            </select>
                                        </div>
                                        <div class="form-group col-md-4 col-sm-4">
                                            <select id="wards" v-model="checkoutData.ward_id"
                                                class="wards form-control form-information isRequired">
                                                <option value="">Chọn Phường/Xã</option>
                                                <option :value="ward.id" v-for="(ward, index) in wards" :key="index">
                                                    {{ ward.name }}</option>
                                            </select>
                                        </div>
                                    </div>
                                </div>

                                <div class="row mt-4">
                                    <div class="form-group col-md-12 col-sm-12 ">
                                        <input type="text" v-model="checkoutData.order_address" id="order_address"
                                            class=" form-control form-information isRequired" placeholder="Địa chỉ"
                                            style="height: 40px; border: 1px solid grey; border-radius: 12px; padding: 10px;">
                                    </div>
                                </div>
                                <div class="row mt-4">
                                    <div class="form-group col-md-6 mb-sm--30">
                                        <label for="order_phone" class="form__label">Số điện thoại <span
                                                class="text-danger">(*)</span></label>
                                        <input type="text" v-model="checkoutData.order_phone" id="order_phone"
                                            class="isRequired  form-control form-information isRequired"
                                            style="height: 40px; border: 1px solid grey; border-radius: 12px; padding: 10px;">
                                    </div>
                                    <div class="form-group col-md-6 mb-sm--30">
                                        <label for="order_email" class="form__label">Địa chỉ Email <span
                                                class="text-danger">(*)</span></label>
                                        <input type="email" v-model="checkoutData.order_email" id="order_email"
                                            class="isRequired form-control form-information"
                                            style="height: 40px; border: 1px solid grey; border-radius: 12px; padding: 10px;">
                                    </div>
                                </div>
                                <div class="form-row mt-4">
                                    <div class="form-group col-md-12 col-sm-12">
                                        <label for="orderNotes" class="form__label">Ghi chú</label>
                                        <textarea class=" form-control form-information" id="order_note"
                                            v-model="checkoutData.order_note"></textarea>
                                    </div>
                                </div>
                                <div class="error"></div>
                            </div>
                        </div>
                    </div>

                    <div class="col-lg-5 mt-5 mt-lg-0"
                        style="box-shadow: rgba(0, 0, 0, 0.05) 0px 6px 24px 0px, rgba(0, 0, 0, 0.08) 0px 0px 0px 1px;">
                        <div class="light-bg">
                            <div class="p-3 white-bg">
                                <h4 class="text-dark mb-2">Phương thức thanh toán</h4>
                                <div class="form-check mt-3">
                                    <input class="form-check-input" type="radio" name="flexRadioDefault"
                                        id="flexRadioDefault1" checked>
                                    <label class="form-check-label" for="flexRadioDefault1">
                                        Thanh toán khi nhận hàng (COD)
                                    </label>
                                </div>
                                <!-- <div class="form-check mt-3">
                            <input class="form-check-input" type="radio" name="flexRadioDefault"
                                id="flexRadioDefault1">
                            <label class="form-check-label" for="flexRadioDefault1">
                                Visa/Mater/JCB
                            </label>
                        </div>
                        <div class="form-check mt-3">
                            <input class="form-check-input" type="radio" name="flexRadioDefault"
                                id="flexRadioDefault1">
                            <label class="form-check-label" for="flexRadioDefault1">
                                Thanh toán qua thẻ ATM
                            </label>
                        </div> -->
                                <div class="row mt-3 p-2">
                                    <button class="primary-btn col-md-6" style="padding: 15px;text-align: center;"
                                        @click="handleCheckout">
                                        <span>Thanh toán</span>
                                    </button>
                                    <a class="white-btn mt-4 mt-md-0 col-md-6" href="product-grid.html"
                                        style="padding: 15px;text-align: center;">
                                        Tiếp tục mua sắm
                                    </a>

                                </div>

                            </div>
                        </div>
                    </div>

                </div>
            </div>
        </section>

        <!-- Giỏ hàng trống -->
        <section style="height: 50vh;" v-else>
            <div class="container">
                <h3 class="text-center">
                    <p> Giỏ hàng trống</p>
                    <NuxtLink class="btn btn-primary btn-sm btn-icon-left" to="/">
                        <span class="btn-icon-left"><i class="fas fa-angle-left"></i></span>
                        Quay lại mua sắm
                    </NuxtLink>
                </h3>
            </div>
        </section>
    </div>
</template>

<style scoped>
.row {
    gap: 20px 0;
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

.white-btn {
    background: #ffffff;
    color: #12181c;
    border: 0;
}

.white-btn:hover {
    color: #003789;
}

order-sm-1 input {
    border: 1px solid grey;
    border-radius: 12px;
    padding: 10px;
}

textarea,
select {
    border-radius: 12px;
}

h4 {
    font-size: 24px;
    font-style: normal;
    margin-bottom: 10px;
    font-weight: 600;
    line-height: 34px;
}

.form-check-input:checked {
    background-color: #003789 !important;
    border-color: #003789 !important;
}

.primary-btn {
    position: relative;
    transition: all 0.6s;
}

.primary-btn,
.white-btn {
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
</style>
